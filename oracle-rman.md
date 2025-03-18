# RMAN

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

Si utilizas un catálogo de recuperación: ````rman target / catalog usuario/password@catdb````

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
