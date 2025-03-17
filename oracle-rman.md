# RMAN

## Componentes Clave de RMAN

- **Target Database**  Es la base de datos a la que RMAN se conecta y de la cual realiza backups o recuperaciones.
- **Recovery Catalog (Opcional)**  Base de datos separada que almacena metadatos de los backups y ayuda en la recuperación.
- **Flash Recovery Area (FRA)** Espacio en disco destinado para backups, logs de recuperación y snapshots.

### Los backups pueden almacenarse en diferentes lugares, dependiendo de la configuración de la base de datos y las necesidades del negocio. Hay dos opciones principales:**

#### 1. Flash Recovery Area (FRA)
- Ubicación en disco gestionada automáticamente por Oracle.
- Facilita la organización y retención de backups.
- También almacena archivos de redo logs archivados, control files y snapshots de la base de datos.
🔹 Ejemplo de configuración FRA:

````sql
ALTER SYSTEM SET DB_RECOVERY_FILE_DEST = '/u01/app/oracle/flash_recovery_area';
ALTER SYSTEM SET DB_RECOVERY_FILE_DEST_SIZE = 100G;
````
🔹 Backup en FRA con RMAN:

````sql
BACKUP DATABASE FORMAT '%d_%T_%U.bkp';
````

#### 2. Backups en una Ubicación Personalizada en Disco
En este caso, el DBA define manualmente el directorio donde se almacenarán los backups.

🔹 Ejemplo de backup en un directorio específico:

````sql
BACKUP DATABASE FORMAT '/backup/oracle/%d_%T_%U.bkp';
````
- %d → Nombre de la base de datos.
- %T → Fecha del backup en formato YYYYMMDD.
- %U → Identificador único del backup.

#### 3. Backups en un Media Manager (Cinta o Cloud)
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
