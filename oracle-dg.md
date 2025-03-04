# Config Oracle Data Guard
## PRIMARY DATABASE  - Dataguard configuration steps Primary side

**Resumen**
- Se habilita ARCHIVELOG y FORCE LOGGING en la Primary.
- Se crean Standby Redo Logs para permitir la aplicación en tiempo real.
- Se configuran TNS y Listener en ambas bases de datos.
- Se establecen parámetros esenciales de Data Guard (log_archive_dest, fal_client, fal_server).
- Se configura el archivo de contraseña y se habilita FRA para gestionar logs.


### 1. Habilitar Archivelog Mode y Force Logging
 **Verificar el modo de log actual**
```sql
SELECT log_mode FROM v$database;
```
Si está en **NOARCHIVELOG**, cambiar a **ARCHIVELOG**
````sql
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
````
Habilitar el archivado de redo logs
````sql
ALTER DATABASE ARCHIVELOG;
````
Forzar que todas las transacciones generen redo logs.
````sql
ALTER DATABASE FORCE LOGGING;
````
Verificar el cambio
````sql
SELECT name, force_logging, log_mode FROM v$database;
````


### 2. Crear Standby Redo Logs (SRL)

Agregar Standby Redo Logs en la Primary
````sql
ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 '/u02/app/oracle/oradata/DIGITAL/redo04.log' SIZE 50M;
ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 '/u02/app/oracle/oradata/DIGITAL/redo05.log' SIZE 50M;
ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 '/u02/app/oracle/oradata/DIGITAL/redo06.log' SIZE 50M;
ALTER DATABASE ADD STANDBY LOGFILE GROUP 7 '/u02/app/oracle/oradata/DIGITAL/redo07.log' SIZE 50M;
````
Verificar la creación
````sql
SELECT group#, member, type, status FROM v$logfile;
````

#### **Diferencia entre `REDO LOGFILE` y `STANDBY LOGFILE`**

| **Característica**         | **Redo Logfile (Primary)** | **Standby Logfile (Standby)** |
|---------------------------|---------------------------|-------------------------------|
| **Ubicación**             | Base de datos Primary     | Base de datos Standby         |
| **Función**               | Guarda los cambios antes de archivarlos | Recibe y aplica los redo logs en tiempo real |
| **Modo de uso**           | Requerido siempre         | Opcional, pero altamente recomendado |
| **Impacto en recuperación** | No afecta Data Guard   | Sin SRLs, la Standby se retrasa |
| **Modo de escritura**     | Escrito por la Primary    | Escrito por el proceso RFS de la Standby |



### 3. Configurar TNS y Listener para Primary y Standby
Editar tnsnames.ora en ambas bases
````ini
DIGITAL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = ocptechnology.localhost)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = digital)
    )
  )

DIGITALDR =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = ocpstandby.localhost)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = digitaldr)
    )
  )
````
### 4. Configurar el Listener

Listener en la Primary (listener.ora)
````ini
SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = digital)
      (ORACLE_HOME = /u02/app/oracle/product/19.3.0/db_home)
      (SID_NAME = digital)
    )
  )

LISTENER =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = ocptechnology.localhost)(PORT = 1521))
  )

ADR_BASE_LISTENER = /u02/app/oracle
````

Listener en la Standby (listener.ora)

````ini
SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = digitaldr)
      (ORACLE_HOME = /u02/app/oracle/product/19.3.0/db_home)
      (SID_NAME = digitaldr)
    )
  )

LISTENER =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = ocpstandby.localhost)(PORT = 1521))
  )

ADR_BASE_LISTENER = /u02/app/oracle
````

### 5. Probar la conectividad con tnsping
````bash
tnsping digital
tnsping digitaldr
````
Si tnsping no funciona:
- Revisar /etc/hosts
- Verificar tnsnames.ora
- Deshabilitar el firewall:
````bash
sudo systemctl stop firewalld
sudo systemctl disable firewalld
````
### 6. Configurar Parámetros de Data Guard en la Primary


| **Comando** | **Propósito** |
|------------|-------------|
| `log_archive_config` | Define la relación entre Primary (`digital`) y Standby (`digitaldr`). |
| `fal_server` | Especifica desde dónde solicitar redo logs faltantes (Standby). |
| `fal_client` | Define el nombre de la base de datos local en FAL (Primary). |
| `standby_file_management` | Permite la creación automática de archivos en Standby al agregar datafiles en la Primary. |
| `log_archive_dest_1` | Configura el almacenamiento de redo logs locales en la **Fast Recovery Area (FRA)**. |
| `log_archive_dest_2` | Envía redo logs desde la Primary (`digital`) a la Standby (`digitaldr`) de forma asíncrona. |
| `LOG_ARCHIVE_DEST_STATE_2` | Habilita la replicación de redo logs hacia la Standby (`digitaldr`). |

 
#### Configurar `log_archive_config`
- Define las bases de datos participantes en Data Guard.
- "digital" es el Primary y "digitaldr" es la Standby.
- Permite la replicación de redo logs entre ambas bases de datos.

```sql
ALTER SYSTEM SET log_archive_config='dg_config=(digital,digitaldr)' SCOPE=both;
````
#### Configurar `fal_server` (Fetch Archive Log Server)
- Especifica desde dónde la base de datos primaria pedirá archivos de redo logs perdidos.
- "digitaldr" es el servidor de la Standby.
- Si la Primary pierde un archivo de redo, lo buscará en "digitaldr".

````sql
ALTER SYSTEM SET fal_server='digitaldr' SCOPE=both;
````

#### Configurar `fal_client` (Fetch Archive Log Client)
- Define el nombre de la base de datos local en el contexto de FAL.
- "digital" se usa para que la Standby se identifique al solicitar redo logs a la Primary.
````sql
ALTER SYSTEM SET fal_client='digital' SCOPE=both;
````

#### Configurar `standby_file_management`
- Permite la creación automática de archivos de datos en la Standby.
- Si en la Primary se agrega un Datafile, se creará automáticamente en la Standby.
````sql
ALTER SYSTEM SET standby_file_management='AUTO' SCOPE=both;
````

#### Configurar `log_archive_dest_1` (Destino local de logs)
- Define el destino de archivos de redo logs locales.
- "use_db_recovery_file_dest" usa la Fast Recovery Area (FRA) para almacenar los logs.
- "valid_for=(all_logfiles,all_roles)" aplica a todos los logs en Primary y Standby.
- "db_unique_name=digital" especifica que esta configuración pertenece a "digital".

````sql
ALTER SYSTEM SET log_archive_dest_1='location=use_db_recovery_file_dest valid_for=(all_logfiles,all_roles) db_unique_name=digital' SCOPE=both;
````
#### Configurar `log_archive_dest_2` (Destino de logs a la Standby)
- Define el destino de los redo logs enviados a la Standby.
- "service=digitaldr" indica que los logs se enviarán a la Standby.
- "async" envía los redo logs de manera asíncrona, sin retrasar las transacciones en la Primary.
- "valid_for=(online_logfiles,primary_role)" aplica solo a online redo logs en modo Primary.
- "db_unique_name=digitaldr" especifica que el destino es la Standby.

````sql
ALTER SYSTEM SET log_archive_dest_2='service=digitaldr async valid_for=(online_logfiles,primary_role) db_unique_name=digitaldr' SCOPE=both;
````
#### Habilitar el destino de replicación LOG_ARCHIVE_DEST_STATE_2
- Activa el destino log_archive_dest_2 para enviar redo logs a la Standby.
- Sin este comando, la Primary no enviará logs a "digitaldr".
````sql
ALTER SYSTEM SET LOG_ARCHIVE_DEST_STATE_2=ENABLE;
````

#### Verificar los valores definidos
````sql
col name for a30
col value for a85
set lin 300 pagesize 300

select name, value from v$parameter
where name in ('db_name','db_unique_name','log_archive_config','log_archive_dest_1','log_archive_dest_2',
'log_archive_dest_state_1′,’log_archive_dest_state_2','remote_login_passwordfile','log_archive_format','log_archive_max_processes',
'fal_server','fal_client','db_file_name_convert','log_file_name_convert','standby_file_management');
````

### 7. Configurar el Password File en la Standby
Oracle usa el password file (orapw<db_name>) para permitir la autenticación remota de usuarios con privilegios SYSDBA o SYSOPER 
sin requerir la contraseña explícita en la conexión. Esto es esencial en Data Guard, RMAN, y conexiones administrativas remotas.

````bash
scp orapwdigital oracle@192.168.74.160:$ORACLE_HOME/dbs/
````
Renombrar el archivo en la Standby:
````bash
cd $ORACLE_HOME/dbs
mv orapwdigital orapwdigitaldr
````

### 8. Habilitar FRA `db_recovery_file_dest` y `db_recovery_file_size` en la Primary 

- Define el tamaño máximo de la Fast Recovery Area (FRA).
- Especifica la cantidad de espacio en disco que Oracle puede usar para almacenar archivos de recuperación.
- Si la FRA se llena, Oracle no podrá generar archived redo logs, lo que puede detener la base de datos.
````sql
ALTER SYSTEM SET db_recovery_file_dest_size=10G;
````
- Especifica la ubicación física donde Oracle almacenará los archivos de recuperación en la FRA.
- Puede ser un directorio en disco o una ubicación ASM (Automatic Storage Management).
````sql
ALTER SYSTEM SET db_recovery_file_dest='/u02/app/oracle/fast_recovery_area/';
`````

**Verificar**

````sql
SHOW PARAMETER DB_RECOVERY_FILE_DEST;
SHOW PARAMETER DB_RECOVERY_FILE_DEST_SIZE;
SELECT name, space_limit, space_used, space_reclaimable  FROM v$recovery_area_usage;
````


**Archivos almacenados en la Fast Recovery Area (FRA)**

| **Archivo**             | **Descripción** |
|-------------------------|---------------|
| **Archived Redo Logs**  | Logs generados en **modo ARCHIVELOG** para recuperación. |
| **Flashback Logs**      | Permiten el **Flashback Database** para restaurar la base de datos a un punto anterior. |
| **Control Files**       | Copia multiplexada del archivo de control. |
| **Redo Logs**          | Opcionalmente, Oracle puede almacenar **online redo logs** en la FRA. |
| **RMAN Backups**       | Backups creados por **RMAN** pueden almacenarse automáticamente en la FRA. |




### 9. Verificar Ubicación de Datafiles y Audit Logs
````sql
SELECT name FROM v$datafile;
SHOW PARAMETER audit;
````



## STANDBY DATABASE - Configuración de la Base de Datos Standby en Oracle Data Guard
### 1.  Create the required directory on the standby side as below:
````bash
mkdir -p /u02/app/oracle/oradata/DIGITALDR/
mkdir -p /u02/app/oracle/oradata/DIGITALDR/pdbseed/
mkdir -p /u02/app/oracle/oradata/DIGITALDR/shri/	
mkdir -p /u02/app/oracle/admin/digitaldr/adump
mkdir -p /u02/app/oracle/fast_recovery_area/
````

### 2. Crear pfile en la Standby
```bash
cat /u02/app/oracle/product/19.3.0/db_home/dbs/initdigitaldr.ora
db_name=digital
enable_pluggable_database=true
```

### 3. Arrancar la instancia en modo NOMOUNT
````
export ORACLE_SID=digitaldr
sqlplus / as sysdba
startup nomount pfile=/u02/app/oracle/product/19.3.0/db_home/dbs/initdigitaldr.ora;
````

### 4. Clonación de la Primary a la Standby con RMAN
si usas RMAN Duplicación desde la base de datos activa con la opción SPFILE y parameter_value_convert, ya no es necesario configurar 
manualmente los parámetros en la Standby, ya que RMAN los aplica automáticamente en el SPFILE de la Standby.

**¿Qué hace RMAN automáticamente?**
- Copia la base de datos Primary a la Standby.
- Configura los parámetros del SPFILE en la Standby con los valores correctos (como db_unique_name, fal_client, fal_server, log_archive_dest_2, etc.).
- Convierte rutas de archivos (db_file_name_convert, log_file_name_convert).
- Crea los archivos de control y redo logs de la Standby.

**¿Cuándo NO necesitas modificar manualmente los parámetros en la Standby?**
✅ Si en el bloque DUPLICATE ya incluyes:
- set db_unique_name='digitaldr'
- set fal_client='digitaldr'
- set fal_server='digital'
- set log_archive_dest_2='service=digital ...'
- set log_archive_config='dg_config=(digital,digitaldr)'


````
rman target sys/sys@digital auxiliary sys/sys@digitaldr

run {
  ALLOCATE CHANNEL c1 TYPE DISK;
  ALLOCATE AUXILIARY CHANNEL c3 TYPE DISK;
  DUPLICATE TARGET DATABASE FOR STANDBY FROM ACTIVE DATABASE
    SPFILE
    PARAMETER_VALUE_CONVERT 'digital','digitaldr'
    SET DB_NAME='digital'
    SET DB_UNIQUE_NAME='digitaldr'
    SET AUDIT_FILE_DEST='/ocptechnology/app/oracle/admin/digital/adump'
    SET DIAGNOSTIC_DEST='/ocptechnology/app/oracle/admin/digital/adump'
    SET DB_FILE_NAME_CONVERT='/u02/app/oracle/oradata/DIGITAL/','/ocptechnology/app/oracle/oradata/DIGITALDR/'
    SET LOG_FILE_NAME_CONVERT='/u02/app/oracle/oradata/DIGITAL/','/ocptechnology/app/oracle/oradata/DIGITALDR/'
    SET CONTROL_FILES='/ocptechnology/app/oracle/oradata/DIGITALDR/standby1.ctl'
    SET LOG_ARCHIVE_MAX_PROCESSES='5'
    SET FAL_CLIENT='digitaldr'
    SET FAL_SERVER='digital'
    SET STANDBY_FILE_MANAGEMENT='AUTO'
    SET LOG_ARCHIVE_CONFIG='dg_config=(digital,digitaldr)'
    SET COMPATIBLE='19.0.0.0.0'
    NOFILENAMECHECK;
}
````
### 5. Abrimos la base de datos standby y arrancamos el Managed Recovery Process (MRP) 
Abrimos la standby database y arrancamos el proceso MRP.

````sql
alter database open;
````

### 6. Arrancamos el Managed Recovery Process (MRP)

- Se ejecuta en la Standby y aplica los cambios de la Primary en los archived redo logs.
- Mantiene la Standby actualizada en tiempo real en modo Real-Time Apply.
- Requiere que la base de datos Standby esté en modo MOUNT para operar.
- Puede trabajar en dos modos:
  - MRP normal (ARCHIVELOG APPLY MODE): Aplica redo logs archivados.
  - MRP en tiempo real (REAL-TIME APPLY): Aplica redo logs en tiempo real desde los Standby Redo Logs (SRLs).

Arrancamos MRP
````sql
alter database recover managed standby database disconnect nodelay;
````
Verificamos MRP
````sql
 select sequence#,process,status from v$managed_standby

 SEQUENCE# PROCESS   STATUS
---------- --------- ------------
         0 ARCH      CONNECTED
         0 DGRD      ALLOCATED
         0 DGRD      ALLOCATED
         0 ARCH      CONNECTED
         0 ARCH      CONNECTED
         0 ARCH      CONNECTED
         0 ARCH      CONNECTED
         0 RFS       IDLE
         0 RFS       IDLE
        21 MRP0      WAIT_FOR_LOG
         0 RFS       IDLE
         0 RFS       IDLE
        21 RFS       IDLE
````

Check the standby database role and open status.
````sql
SQL> select name,open_mode,database_role from v$database;

NAME      OPEN_MODE            DATABASE_ROLE
--------- -------------------- ----------------
DIGITAL   READ ONLY WITH APPLY PHYSICAL STANDBY
````

