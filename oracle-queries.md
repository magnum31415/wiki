# 📌 PDBs
✅**Setup sqlplus prompt **
````sql
SET SQLPROMPT "_USER'@'_CONNECT_IDENTIFIER> "
````


✅**Show PBDs**
````sql
show pdbs
````

✅**Show connected pdb**
````sql
show con_name
````

✅**Connect to a specific pdb**
````sql
alter session set container=PDBNAME;
````
# 📌 Transparent Data Encryption (TDE)



✅**Tablespace online encryption**
````sql
ALTER TABLESPACE users_non_enc ENCRYPTION ONLINE USING 'AES256' ENCRYPT;
````

✅**Oracle Wallet Status**
````sql
col WRL_PARAMETER format a40
SELECT * FROM v$encryption_wallet;
````

✅**List Encryption Keys**
````sql
set linesize 200
col tag format a20
col KEY_ID format a52
col ACTIVATION_TIME format a22
col CREATION_TIME format a22
SELECT key_id, tag, creation_time, activation_time, key_use, keystore_type, backed_up FROM v$encryption_keys ORDER BY creation_time;
````

✅**Open Oracle Wallet**
````sql
ADMINISTER KEY MANAGEMENT SET KEYSTORE OPEN IDENTIFIED BY "<wallet_password>" CONTAINER=ALL;
````
✅**Close Oracle Wallet**
````sql
ADMINISTER KEY MANAGEMENT SET KEYSTORE CLOSE IDENTIFIED BY "<wallet_password>" CONTAINER=ALL;
````


# 📌 DataGuard

✅**Query database cuurent Role**
````sql
SET LINESIZE 200
COL DB_UNIQUE_NAME FORMAT A15
COL CDB_NAME FORMAT A15
COL CONTAINER_NAME FORMAT A15
COL DATABASE_ROLE FORMAT A16
COL OPEN_MODE FORMAT A20
COL PROTECTION_MODE FORMAT A25
COL PROTECTION_LEVEL FORMAT A25
COL SWITCHOVER_STATUS FORMAT A20

SELECT 
    SYS_CONTEXT('USERENV', 'DB_NAME') AS CDB_NAME,  -- Shows the CDB name
    SYS_CONTEXT('USERENV', 'CON_NAME') AS CONTAINER_NAME, -- Current container (CDB or PDB)
    DATABASE_ROLE,
    DB_UNIQUE_NAME,
    CDB,
    OPEN_MODE,
    PROTECTION_MODE,
    PROTECTION_LEVEL,
    SWITCHOVER_STATUS
FROM v$database;
````

## Redo Apply

✅**Check Redo Apply**
````sql
SELECT PROCESS, STATUS, THREAD#, SEQUENCE# 
FROM V$MANAGED_STANDBY
WHERE PROCESS IN ('MRP0', 'RFS', 'LNS');
````
-Si el **MRP0 (Managed Recovery Process)** no aparece o su estado es WAITING FOR GAP o STOPPED, el Redo Apply está detenido.

-Si **MRP=** está en estado **APPLYING_LOG**, el Redo Apply está en ejecución.

-**RFS (Remote File Server)** Receives redo logs from Primary.

✅**Stop Redo Apply**
````sql
ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;
````
✅**Start Redo Apply**
````sql
ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;
````

 ✅**Check Redo Apply Status**
````sql
SELECT ARCH.THREAD#, ARCH.SEQUENCE# AS "LAST_RECEIVED", 
       APPL.SEQUENCE# AS "LAST_APPLIED", 
       (ARCH.SEQUENCE# - APPL.SEQUENCE#) AS "LAG"
FROM   (SELECT THREAD#, MAX(SEQUENCE#) SEQUENCE# 
        FROM V$ARCHIVED_LOG 
        WHERE APPLIED = 'YES' GROUP BY THREAD#) APPL,
       (SELECT THREAD#, MAX(SEQUENCE#) SEQUENCE# 
        FROM V$ARCHIVED_LOG GROUP BY THREAD#) ARCH
WHERE  ARCH.THREAD# = APPL.THREAD#;
````

# 📌 Tablespaces

✅**Total, Free, Used space in tablespaces**

````sql
SET LINESIZE 200
COL TABLESPACE_NAME FORMAT A20
COL total_space_mb FORMAT 999,999,999.99
COL used_space_mb FORMAT 999,999,999.99
COL free_space_mb FORMAT 999,999,999.99
COL used_percentage FORMAT 999.99
COL ENCRYPTED FORMAT A10

SELECT 
    df.tablespace_name,
    df.total_space_mb,
    NVL(fs.free_space_mb, 0) AS free_space_mb,
    (df.total_space_mb - NVL(fs.free_space_mb, 0)) AS used_space_mb,
    ROUND((1 - NVL(fs.free_space_mb, 0) / df.total_space_mb) * 100, 2) AS used_percentage,
    NVL(te.encrypted, 'NO') AS encrypted
FROM 
    (SELECT tablespace_name, SUM(bytes) / 1024 / 1024 AS total_space_mb 
     FROM dba_data_files 
     GROUP BY tablespace_name) df
LEFT JOIN 
    (SELECT tablespace_name, SUM(bytes) / 1024 / 1024 AS free_space_mb 
     FROM dba_free_space 
     GROUP BY tablespace_name) fs
ON df.tablespace_name = fs.tablespace_name
LEFT JOIN 
    (SELECT tablespace_name, encrypted 
     FROM dba_tablespaces) te
ON df.tablespace_name = te.tablespace_name
ORDER BY df.tablespace_name;
````

✅**List datafile size**

````sql
SET LINESIZE 200
col FILE_NAME format a50
SELECT 
    FILE_NAME,
    TABLESPACE_NAME,
    ROUND(BYTES / 1024 / 1024, 2) AS SIZE_MB,
    AUTOEXTENSIBLE,
    ROUND(MAXBYTES / 1024 / 1024, 2) AS MAX_SIZE_MB
FROM DBA_DATA_FILES
ORDER BY SIZE_MB DESC;
````

✅**Tables in a tablespace**
````sql
SET LINESIZE 200
col TABLESPACE_NAME format a20
col TABLE_NAME format a20
col OWNER format a20

SELECT 
    OWNER, 
    TABLE_NAME, 
    TABLESPACE_NAME, 
    NUM_ROWS 
FROM DBA_TABLES 
WHERE TABLESPACE_NAME = '&tablespace_name'
ORDER BY NUM_ROWS DESC;

````

# 📌 Instance 

✅**Instance startup time**
````sql
SELECT INSTANCE_NAME, TO_CHAR(STARTUP_TIME, 'DD-MON-YYYY HH24:MI:SS') AS STARTUP_TIME
FROM V$INSTANCE;
````
✅**Database files**
````sql
SELECT substr(MEMBER,1,40) AS LOG FROM V$LOGFILE;
SELECT substr(NAME,1,40) AS DATAFILE FROM V$DATAFILE;
SELECT substr(NAME,1,40) AS TEMPFILE FROM V$TEMPFILE;
SELECT substr(NAME,1,40) AS CONTROLFILE FROM V$CONTROLFILE;
````



# 📌 ControlFiles
✅**Control File Location **
````sql
SELECT NAME FROM V$CONTROLFILE;
````
✅**Control File Backup **
````sql
ALTER DATABASE BACKUP CONTROLFILE TO '/ruta/backup_control.ctl';
````
