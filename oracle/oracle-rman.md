# RMAN

# Commandos RMAN

- Show configuraction:  ````show all;````
- Backup redo archivados y una copia del control file y del SPFILE: ````BACKUP DATABASE PLUS ARCHIVELOG;````
- Verificar backups existented: ````LIST BACKUP;````

## ✅Componentes Clave de RMAN

- **Target Database**  Es la base de datos a la que RMAN se conecta y de la cual realiza backups o recuperaciones.
- **Recovery Catalog (Opcional)**  Base de datos separada que almacena metadatos de los backups y ayuda en la recuperación.
- **Flash Recovery Area (FRA)** Espacio en disco destinado para backups, logs de recuperación y snapshots.

- El Recovery Catalog NO almacena los backups, solo los metadatos.
- Los backups pueden guardarse en FRA, en disco local, en cinta o en la nube.
- Usar RMAN permite gestionar y restaurar los backups fácilmente sin importar dónde estén almacenados.

## ✅ Almacenaje de los backups

### 1. Flash Recovery Area (FRA)

- Ubicación en disco gestionada automáticamente por Oracle.
- Facilita la organización y retención de backups.
- También almacena archivos de redo logs archivados, control files y snapshots de la base de datos.

🔹 Ejemplo de configuración FRA:

````sql
alter system set db_recovery_file_dest_size = 2G SCOPE = BOTH;
alter system set db_recovery_file_dest = '/u03/fra' SCOPE = BOTH;
show parameter db_Recovery_file_dest
````
🔹 Backup en FRA con RMAN:

````sql
BACKUP DATABASE FORMAT '%d_%T_%U.bkp';
````

#### What is the Right Size of the FRA
Ideally in production environment you should set the size of your Fast recovery area 5-to-10 times larger than the size of your database.
So ideally you should set the size of your FRA to at least the size of your database. But it’s still not recommended.

#### How to check FRA size?
````sql
SELECT 
    name, 
    ROUND(space_limit / POWER(1024,3), 2) AS space_limit_gb, 
    ROUND(space_used / POWER(1024,3), 2) AS space_used_gb
FROM v$recovery_file_dest;
````

### 2. Backups en una Ubicación Personalizada en Disco
En este caso, el DBA define manualmente el directorio donde se almacenarán los backups.

🔹 Ejemplo de backup en un directorio específico:

````sql
BACKUP DATABASE FORMAT '/backup/oracle/%d_%T_%U.bkp';
````
- %d → Nombre de la base de datos.
- %T → Fecha del backup en formato YYYYMMDD.
- %U → Identificador único del backup.

### 3. Backups en un Media Manager (Cinta o Cloud)
Se usa para almacenar backups en dispositivos externos como:
- Cintas magnéticas (utilizando software como Oracle Secure Backup o soluciones de terceros).
- Cloud Storage (como Oracle Cloud, AWS S3, Google Cloud Storage).
🔹 Ejemplo de backup en cinta (requiere configuración previa del Media Manager):
````sql
BACKUP DATABASE DEVICE TYPE sbt FORMAT 'db_backup_%U';
````
🔹 Ejemplo de backup en Oracle Cloud:

````sql
CONFIGURE CHANNEL DEVICE TYPE sbt PARMS 'SBT_LIBRARY=oracle.disksbt, ENV=(OPC_PFILE=/home/oracle/opc_config)';
BACKUP DATABASE;
````

## ✅¿Cómo Ver Dónde Están los Backups Guardados?
### Paso 1: Iniciar RMAN
Abre una terminal en el servidor donde está la base de datos y ejecuta:````rman target / ````

Si utilizas un catálogo de recuperación: ````rman target / catalog usuario/REPLACE_WITH_RMAN_PASSWORD@catdb````

### Paso 2: Ejecutar el Comando
Dentro de RMAN, simplemente escribe:
````sql
LIST BACKUP SUMMARY;
````
Mostrará un resumen de los backups disponibles, incluyendo:

-Base de datos respaldada
-Tipo de backup (full, incremental, archivelog, etc.)
-Fecha de creación
-Estado del backup
-Destino donde está almacenado (FRA, disco, cinta, etc.)

### Paso 3: Ver Detalles Adicionales

Si necesitas más detalles, puedes usar: ````LIST BACKUP;````
Para ver solo los archivos de control: ````LIST BACKUP OF CONTROLFILE;````
Para ver sólo los archive logs:````LIST BACKUP OF ARCHIVELOG ALL;````

## ✅Configuring the Fast Recovery Area (FRA)
Configuring the Fast Recovery Area (FRA) should be the first step of your Database and recovery strategy. 
This is because FRA is the place, where Oracle Recovery Manager (RMAN) saves all the backups of the database and necessary configuration files along with metadata.

Improper configured FRA will cause error, which will lead to database backup failure. And, the backup failure is the nightmare of every DBA’s life.

# ✅Tipos de Backups

## 🚀Offline Backup:

**Base de Datos Apagada**: La base de datos debe estar en estado SHUTDOWN IMMEDIATE, SHUTDOWN TRANSACTIONAL o SHUTDOWN NORMAL.
**Copia de Archivos Físicos:** Se respaldan los archivos esenciales como:
- Datafiles (.dbf)
- Control files (.ctl)
- Redo log files (.log)
- Archivos de parámetros (spfile o pfile)

**Consistencia Garantizada:** No se requiere aplicar redo logs o archive logs para restaurar, ya que la base de datos se encuentra en un estado consistente en el momento del backup.
**Método Manual:** Normalmente se realiza copiando físicamente los archivos con herramientas como cp, tar, rsync en sistemas Unix/Linux o copy en Windows.

### Cómo hacer un Offline/Cold Backup  manual
1. Apagar la base de datos: ````SHUTDOWN IMMEDIATE;````
2. Copiar los archivos físicos de la base de datos:
````bash
cp -r /u01/oracle/oradata/MYDB /backup/location/
cp /u01/oracle/controlfile.ctl /backup/location/
cp /u01/oracle/redo01.log /backup/location/
````
3. Encender la base de datos nuevamente: ````STARTUP;````

### Cómo hacer un Offline/Cold Backup con RMAN

MAN (Recovery Manager) permite realizar un Offline Backup (Cold Backup) en Oracle, pero **solo si la base de datos está en estado MOUNT**. 
Es una alternativa segura y recomendada en entornos donde se puede detener la base de datos.

1️1. Apagar la base de datos y montarla sin abrirla
````sql
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
````
2. Realizar el backup con RMAN :
Opcionalmente, también puedes incluir el control file y spfile:

````sql
rman target /
BACKUP DATABASE;
````
También puedes incluir el control file y spfile:
````
BACKUP DATABASE INCLUDE CURRENT CONTROLFILE SPFILE;
````

Si deseas comprimir el backup para ahorrar espacio:
````
BACKUP AS COMPRESSED BACKUPSET DATABASE INCLUDE CURRENT CONTROLFILE SPFILE;
````

Verificar backup: ````LIST BACKUP;````

Si quieres especificar una ubicación personalizada, usa:
````rman
CONFIGURE CHANNEL DEVICE TYPE DISK FORMAT '/backup/rman_%d_%T_%U.bkp';
````
Esto guardará los backups en /backup/ con un nombre único.

3️. Abrir la base de datos nuevamente
````sql
ALTER DATABASE OPEN;
````

📌 Ventajas de usar RMAN para Offline Backup
- Copia consistente sin necesidad de aplicar redo logs o archive logs.
- Backup administrado y catalogado en RMAN.
- Compresión y optimización del tamaño del backup.
- Automatización posible mediante scripts.


   

📌 Ventajas:
- Copia de seguridad simple y rápida.
- Restauración fácil y sin riesgo de corrupción.
- No depende de los archive logs.

📌 Desventajas:
Tiempo de inactividad: No es viable para bases de datos en producción 24/7.
No permite backup en caliente: Se requiere apagar la base de datos.

📌 Alternativa:
Si necesitas respaldos sin interrumpir la base de datos, usa Online Backup con RMAN y modo ARCHIVELOG.

## 🚀Online Backup (Inconsistent Backup):

Un **backup se considera inconsistente** si los datafiles no están sincronizados con los redo logs y control files. Esto ocurre cuando la base de datos estaba en funcionamiento durante el backup.

🔹 ¿Cuándo un backup es inconsistente?
- La base de datos estaba abierta y en uso durante el backup.
- Se hizo un Online Backup sin detener la base de datos.
- Hay transacciones en curso que no fueron confirmadas o descartadas.


### Requisitos

#### 1. La base de datos debe estar en Modo ARCHIVELOG
   
   Para verificar si está activado:
````sql
SELECT LOG_MODE FROM V$DATABASE;
````
Si el resultado es NOARCHIVELOG, se debe habilitar:
````sql
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE ARCHIVELOG;
ALTER DATABASE OPEN;
````
Verifica nuevamente:
````sql
ARCHIVE LOG LIST;
````

#### 2. Configurar la Destinación de Archive Logs en Oracle
Oracle permite configurar la ubicación de los Archive Logs de dos maneras:

🔹**Mediante LOG_ARCHIVE_DEST_n** → Define rutas específicas para almacenar los Archive Logs.
🔹**Mediante DB_RECOVERY_FILE_DEST (Flash Recovery Area - FRA)** → Oracle administra automáticamente el almacenamiento de los Archive Logs y otros archivos de recuperación.

Cuando habilitas ARCHIVELOG mode, Oracle guarda los redo logs archivados en una ubicación específica. 
Puedes configurar esta ubicación de diferentes maneras según tus necesidades.

Hay varias maneras:

#### 2.1. Configurar una Única Destinación para Archive Logs *LOG_ARCHIVE_DEST_n*

**n** can define up tp10 destinations for saving the archive logs.

It can be:
- A Local Directory
- A Network Directory
- A NAS Location
- A Service Name


🔹opción MANDATORY en LOG_ARCHIVE_DEST_n
- Si un destino es **MANDATORY**, Oracle no continuará con la operación de la base de datos si no se puede escribir el archivo de archive log en ese destino.
- Si un destino es opcional (**DEFAULT**), Oracle puede continuar con el procesamiento aunque falle la escritura del archive log en ese destino.

🔹parametro **LOG_ARCHIVE_MIN_SUCCEED_DEST**
Define el número mínimo de destinos en los que se debe escribir un archive log exitosamente antes de continuar.
````sql
ALTER SYSTEM SET LOG_ARCHIVE_MIN_SUCCEED_DEST=1 SCOPE=BOTH;
````

🔹Si deseas almacenar los Archive Logs en una sola ubicación, usa el siguiente comando:

````sql
ALTER SYSTEM SET LOG_ARCHIVE_DEST_1='LOCATION=/u01/oracle/archivelogs MANDATORY' SCOPE=BOTH;
SHOW PARAMETER LOG_ARCHIVE_DEST_1;
````

🔹Si deseas que Oracle guarde los archive logs en dos ubicaciones diferentes (ej. disco local y red):

````sql
ALTER SYSTEM SET LOG_ARCHIVE_DEST_1='LOCATION=/u01/oracle/archivelogs MANDATORY' SCOPE=BOTH;
ALTER SYSTEM SET LOG_ARCHIVE_DEST_2='LOCATION=/mnt/backup/archivelogs OPTIONAL' SCOPE=BOTH;
SELECT DEST_ID, STATUS, DESTINATION FROM V$ARCHIVE_DEST WHERE STATUS='VALID';
````

🔹Si quieres enviar los archive logs a otro servidor mediante Oracle Data Guard, usa:

````sql
ALTER SYSTEM SET LOG_ARCHIVE_DEST_2='SERVICE=standby_db' SCOPE=BOTH;
````

**SERVICE=standby_db** → Indica que los archive logs se enviarán a la base de datos de Standby (Data Guard).

#### 2.2. Co
