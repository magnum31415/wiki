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







````sql
ALTER SYSTEM SET log_archive_config='dg_config=(digital,digitaldr)' SCOPE=both;
ALTER SYSTEM SET fal_server='digitaldr' SCOPE=both;
ALTER SYSTEM SET fal_client='digital' SCOPE=both;
ALTER SYSTEM SET standby_file_management='AUTO' SCOPE=both;
ALTER SYSTEM SET log_archive_dest_1='location=use_db_recovery_file_dest valid_for=(all_logfiles,all_roles) db_unique_name=digital' SCOPE=both;
ALTER SYSTEM SET log_archive_dest_2='service=digitaldr async valid_for=(online_logfiles,primary_role) db_unique_name=digitaldr' SCOPE=both;
ALTER SYSTEM SET LOG_ARCHIVE_DEST_STATE_2=ENABLE;
````
Verificar configuración
````sql
SELECT name, value FROM v$parameter WHERE name IN (
  'db_name','db_unique_name','log_archive_config','log_archive_dest_1','log_archive_dest_2',
  'log_archive_dest_state_1','log_archive_dest_state_2','remote_login_passwordfile',
  'log_archive_format','log_archive_max_processes','fal_server','fal_client',
  'db_file_name_convert','log_file_name_convert','standby_file_management'
);
````

### 7. Configurar el Password File en la Standby
````bash
scp orapwdigital oracle@192.168.74.160:$ORACLE_HOME/dbs/
````
Renombrar el archivo en la Standby:

````bash
cd $ORACLE_HOME/dbs
mv orapwdigital orapwdigitaldr
````

### 8: Habilitar FRA en la Primary
````
sql
ALTER SYSTEM SET db_recovery_file_dest_size=10G;
ALTER SYSTEM SET db_recovery_file_dest='/u02/app/oracle/fast_recovery_area/';
````

### 9: Verificar Ubicación de Datafiles y Audit Logs
````sql
SELECT name FROM v$datafile;
SHOW PARAMETER audit;
````


## STANDBY DATABASE - Configuración de la Base de Datos Standby en Oracle Data Guard

### 1. Verificación del archivo de inicialización en la Standby
```bash
cat /u02/app/oracle/product/19.3.0/db_home/dbs/initdigitaldr.ora
db_name=digital
enable_pluggable_database=true
```

### 2 .Arrancar la instancia en modo NOMOUNT
````
export ORACLE_SID=digitaldr
sqlplus / as sysdba
startup nomount pfile=/u02/app/oracle/product/19.3.0/db_home/dbs/initdigitaldr.ora;
````

### 3. Clonación de la Primary a la Standby con RMAN

````
rman target sys/sys@digital auxiliary sys/sys@digitaldr

RUN {
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
