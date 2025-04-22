# 📌 Multithenant CDB & PDB


- Verificar el contenedor actual: ``SHOW CON_NAME;``
- Cambiar la sesión al PDB "DB1": ``ALTER SESSION SET CONTAINER = <nombre_del_PDB>;``
- Ver los PDBs disponibles:
````sql
SHOW PDBS;
SELECT name, open_mode FROM v$pdbs;
````
 
- Cerrar solo una PDB y mantener el CDB activo: ``ALTER PLUGGABLE DATABASE <nombre_del_PDB> CLOSE IMMEDIATE;``
- Abrir una PDB: ``ALTER PLUGGABLE DATABASE <nombre_del_PDB> OPEN`READ WRITE;`
- Configurar una PDB para que se abrá: ``ALTER PLUGGABLE DATABASE <nombre_del_PDB> SAVE STATE;``

  
✅**Setup sqlplus prompt**
````sql
SET SQLPROMPT "_USER'@'_CONNECT_IDENTIFIER> "
````


# 📌 Transparent Data Encryption (TDE)

✅**Procedure to enable TDE**

1. **Create a spfile backup**
   
   ````sql
   CREATE PFILE='${ORACLE_HOME}/dbs/initSID_YYYYMMDD_HHMM.ora' FROM SPFILE;
   ````
2. **Define defaults: encryption for new tablespaces encryption algorithm**
   
   ````sql
   ALTER SYSTEM SET ENCRYPT_NEW_TABLESPACES = 'ALWAYS' SCOPE = SPFILE SID = '*';
   ALTER SYSTEM SET "_tablespace_encryption_default_algorithm" = AES256 SCOPE=SPFILE SID='*';
    ````
3. **Set wallet_root**
   
    ````sql
    ALTER SYSTEM SET wallet_root='/u01/app/oracle/wallet' SCOPE=SPFILE;
    !mkdir -p /u01/app/oracle/wallet/tde
    ````
4. **Restart Database**

    ````sql
    SHUTDOWN IMMEDIATE;
    STARTUP;
    ````
    
5. **Check parameters**
   
    ````sql
    SHOW PARAMETER wallet_root;
    SHOW PARAMETER ENCRYPT_NEW_TABLESPACES;
    SHOW PARAMETER tablespace_encryption_default_algorithm;
    ````
7. **Set set TDE_CONFIGURATION to FILE (to use OracleWallet)**
   
    ````sql
    ALTER SYSTEM SET tde_configuration='KEYSTORE_CONFIGURATION=FILE' SCOPE=BOTH;
    SHOW PARAMETER tde_configuration;
    ````
   
9. **Create keystore (oracle wallet)**

    ````sql
    ALTER SESSION SET CONTAINER = CDB$ROOT;
    ADMINISTER KEY MANAGEMENT 
    CREATE KEYSTORE 
    '/u01/app/oracle/wallet/tde' 
    IDENTIFIED BY "<wallet_password>";
    ````
10. **Open keystore on all containers**

    ````sql
    ADMINISTER KEY MANAGEMENT 
    SET KEYSTORE OPEN 
    IDENTIFIED BY "<wallet_password>"
    CONTAINER=ALL;
    ````
11. **Create new Master Encryption Key on all CONTAINERS**
    ````sql
    ADMINISTER KEY MANAGEMENT 
    SET KEY 
    USING TAG "INITIAL_MASTER_KEY" 
    IDENTIFIED BY "<wallet_password>" 
    WITH BACKUP USING "INITIAL-MEK"
    CONTAINER=ALL;
    ````
11. **Create auto-login Keystore**

    ````sql
    ADMINISTER KEY MANAGEMENT 
    CREATE AUTO_LOGIN KEYSTORE 
    FROM KEYSTORE '/u01/app/oracle/wallet/tde' 
    IDENTIFIED BY "<wallet_password>";
    ````

✅**Tablespace online encryption**
````sql
ALTER TABLESPACE <tablespace_name> ENCRYPTION ONLINE USING 'AES256' ENCRYPT;
````

✅**Tablespace offline encryption**
````sql
ALTER DATABASE /* 28300MB */ DATAFILE '/u02/oradata/cdbname/dbname/<tablesapce_name>001.dbf' ENCRYPT;
````

✅**To monitor the progress of tablespace encryption**
````sql
SELECT sid,
       serial#,
       sofar,
       totalwork,
       ROUND((sofar / totalwork) * 100, 2) AS pct_complete,
       elapsed_seconds,
       time_remaining,
       opname,
       message
  FROM v$session_longops
 WHERE opname LIKE '%Encrypt%'
   AND sofar <> totalwork
 ORDER BY start_time DESC;

````
✅**List tbalespaces**

**Encrypted tablespaces**
````sql
SELECT C.NAME CONTAINER_NAME, TS.NAME TABLESPACE_NAME ,ET.ENCRYPTIONALG ENCRYPTION_ALGORITHM
FROM V$TABLESPACE TS, V$CONTAINERS C, V$ENCRYPTED_TABLESPACES ET
WHERE TS.CON_ID=C.CON_ID
AND TS.CON_ID=ET.CON_ID
AND TS.TS#=ET.TS#;
````
**Unencrypted tablespaces**
````sql
col CONTAINER_NAME format a25;
col TABLESPACE_NAME format a25;
SELECT C.NAME CONTAINER_NAME, TS.NAME TABLESPACE_NAME
FROM V$CONTAINERS C, V$TABLESPACE TS
WHERE C.NAME!='PDB$SEED'
AND C.CON_ID=TS.CON_ID
AND TS.CON_ID||':'||TS.TS# NOT IN (SELECT TSE.CON_ID||':'||TSE.TS# FROM V$ENCRYPTED_TABLESPACES TSE)
ORDER BY 1,2;
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

✅**Query database current Role**
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

## Parameters

✅**Create Standby from Primary with RMAN**

First, start standby in nomount
````sql
BASH> sqlplus / as sysdba
SQL>  startup nomount pfile=/u01/app/oracle/product/19.21.0.0/dbname/dbs/initdbname.ora
````
Second, duplicate primary
````sql
rman target=sys/SYSPASSWD@dbname_1 auxiliary sys/SYSPASSWD@dbname_2
RMAN > DUPLICATE TARGET DATABASE FOR STANDBY FROM ACTIVE DATABASE NOFILENAMECHECK;
````

✅**List DataGuard Parameter**
````sql
show parameter log_archive_config
show parameter log_archive_dest_1
show parameter log_archive_dest_2
show parameter dg_broker_start
show parameter dg_broker_config_file1
show parameter dg_broker_config_file2
````



✅**Data Guard Configuration**

🔹 **These parameters must be defined as in the table  (Primary vs Standby)**
| **Parameter**            | **Primary**                                                                                 | **Standby**                                                                                 |
|--------------------------|-------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **log_archive_config**   | `dg_config=(dbname_1,dbname_1)`                                                           | `dg_config=(dbname_1,dbname_1)`                                                           |
| **log_archive_dest_1**   | `location=USE_DB_RECOVERY_FILE_DEST valid_for=(all_logfiles,all_roles) db_unique_name=dbname_1` | `location=USE_DB_RECOVERY_FILE_DEST valid_for=(all_logfiles,all_roles) db_unique_name=dbname_2` |
| **log_archive_dest_2**   | `service="dbname_2", ASYNC NOAFFIRM delay=0 optional compression=disable max_failure=0 reopen=300 db_unique_name="dbname_2" net_timeout=30, valid_for=(online_logfile,all_roles)` |  |
| **dg_broker_start**      | `TRUE`                                                                                    | `TRUE`                                                                                    |
| **dg_broker_config_file1** | `/u01/app/oracle/product/19.21.0.0/dbname/dbs/dr1dbname_1.dat`                          | `/u01/app/oracle/product/19.21.0.0/dbname/dbs/dr1dbname_2.dat`                          |
| **dg_broker_config_file2** | `/u01/app/oracle/product/19.21.0.0/dbname/dbs/dr2dbname_1.dat`                          | `/u01/app/oracle/product/19.21.0.0/dbname/dbs/dr1dbname_2.dat`                          |

**How to setup**
````sql
alter system set log_archive_dest_1='location=USE_DB_RECOVERY_FILE_DEST valid_for=(all_logfiles,all_roles) db_unique_name=dbname_1';
alter system set log_archive_dest_2='service="dbname_1"','ASYNC NOAFFIRM delay=0 optional compression=disable max_failure=0 reopen=300 db_unique_name="dbname_1" net_timeout=30','valid_for=(online_logfile,all_roles)';
````

**Config Datagruard with dataguard Mager**
````sql
dgmgrl sys/SYSPASSWD
create configuration cdname_dg_config as primary database is cdbname_1 connect identifier is cdbname_1;
add database cdbname_2 as connect identifier is cdbname_2 maintained as physical;
enable configuration;
show configuration;
````



✅**Create Dataguard Broker configuration**

# Data Guard Configuration Commands Summary

From DataGuard Manager command line ````dgmgrl sys/SYSPASSWD````

| **Order** | **Command** | **Action** |
|---------|------------|-----------|
| **1** | `CREATE CONFIGURATION dbname_dg_config AS PRIMARY DATABASE IS dbname_1 CONNECT IDENTIFIER IS dbname_1;` | Creates a **Data Guard Configuration** with `dbname_1` as the **Primary Database**. |
| **2** | `ADD DATABASE dbname_2 AS CONNECT IDENTIFIER IS dbname_2 MAINTAINED AS PHYSICAL;` | Adds `dbname_1` as a **Physical Standby Database**. |
| **3** | `EXIT;` | Exits **DGMGRL** (Data Guard Manager). |
| **4** | `SHOW CONFIGURATION;` | Displays the current Data Guard configuration status. |
| **5** | `SHOW DATABASE dbname_1;` | Displays details about the **Primary Database** (`dbname_1`). |
| **6** | `SHOW DATABASE dbname_1;` | Displays details about the **Standby Database** (`dbname_2`). |
| **7** | `ENABLE CONFIGURATION;` | Enables Data Guard Broker monitoring and management. |
| **8** | `EDIT DATABASE cotde_2 SET STATE='APPLY-ON';` | Starts **Redo Log Apply** on the standby database (`cotde_2`). |



## Redo Apply

✅**List RedoLof Files **
````sql
SELECT GROUP#, STATUS, MEMBER FROM V$LOGFILE;
````
✅**Estado de los Grupos de Redo Logs **
````sql
SELECT GROUP#, SEQUENCE#, BYTES/1024/1024 AS SIZE_MB, MEMBERS, STATUS, ARCHIVED 
FROM V$LOG;
````

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
✅**Restart managed recovery**
In Data Guard Broker (DGMGRL), you can restart managed recovery to apply a redo gap by toggling the apply state. 

````sql
Copiar
DGMGRL> EDIT DATABASE <standby_instance_name> SET STATE=APPLY-OFF;
DGMGRL> EDIT DATABASE <standby_instance_name> SET STATE=APPLY-ON;
````

Equivalent commands but is better to  maintain configuration integrity.
````sql
DGMGRL> EDIT DATABASE <standby> SET STATE=APPLY-OFF;
SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;
````

# 📌 Tablespaces
✅**Objects in a tablespace**
````sql
SELECT
 DISTINCT OWNER, TABLESPACE_NAME
FROM
 DBA_SEGMENTS
WHERE
 TABLESPACE_NAME = 'NAME_TABLESPACE_TO_SEARCH'
ORDER BY OWNER;
````

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

````sql
SELECT 
    owner,
    segment_name AS table_name,
    ROUND(SUM(bytes)/1024/1024, 2) AS size_mb
FROM dba_segments
WHERE segment_type = 'TABLE'
  AND tablespace_name = 'SANDBOX'
GROUP BY owner, segment_name
ORDER BY size_mb DESC;

````






# 📌 Instance 
✅**Active sessions**
````sql
COL SID        FOR 99999
COL SERIAL#    FOR 99999
COL USERNAME   FOR A25
COL STATUS     FOR A10
COL OSUSER     FOR A25
COL PROGRAM    FOR A25
COL MACHINE    FOR A25
COL MODULE     FOR A25
COL ACTION     FOR A25
COL EVENT      FOR A25
COL SQL_ID     FOR A15

SELECT 
    s.sid,
    s.serial#,
    s.username,
    s.status,
    s.osuser,
    s.program,
    s.machine,
    s.module,
    s.action,
    s.event,
    s.sql_id
FROM 
    v$session s
WHERE 
    s.status = 'ACTIVE'
  AND s.username IS NOT NULL;
````

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

# 📌Database

✅**Oracle Database Startup Modes Comparison**

| **Startup Mode**       | **Description**                                     | **What Happens at This Stage?**                                       | **Can Users Connect?**        | **RMAN Backups Possible?** |
|-----------------------|-------------------------------------------------|------------------------------------------------------------------------|------------------------------|---------------------------|
| **NOMOUNT**           | Instance started, but database is not mounted.   | - The **instance starts** (SGA allocated, background processes started).<br>- The **control file is NOT read**.<br>- The database files are **not yet accessed**. | **No** (Only SYSDBA/SYSOPER) | **Control File Backup Only** |
| **MOUNT**             | The database is mounted, but not open.           | - Oracle **reads the control file**.<br>- The **datafiles and redo logs are known**, but not yet accessible.<br>- The database is ready for recovery operations. | **No** (Only SYSDBA)         | **Control File and Archive Log Backup** |
| **OPEN**              | The database is fully open and available.         | - The database opens and **datafiles are available for read/write**.<br>- Users can perform **queries and transactions**.<br>- Redo logs and undo segments are fully operational. | **Yes** (All users)          | **Full Database Backup** |
| **MOUNT (RESTRICTED)** | Database mounted but only privileged users can access. | - Same as `MOUNT`, but **only SYSDBA users** can perform maintenance tasks.<br>- Used for maintenance or performing special recovery. | **No** (Only SYSDBA)         | **Control File and Archive Log Backup** |
| **OPEN (READ ONLY)**  | Database is open but in read-only mode.           | - The database is open, but **no modifications are allowed**.<br>- Used for reporting, backups, or testing. | **Yes (Read-Only Access)**   | **Datafile Backups** |
| **OPEN (RESTRICTED)** | Database is open, but access is limited.         | - The database is open, but **only privileged users** can connect.<br>- Used for maintenance tasks like upgrading. | **Yes (Only SYSDBA/SYSOPER)** | **Full Database Backup** |
| **FORCE STARTUP**     | Used to restart a crashed instance.               | - If the instance is running, Oracle first **shuts it down with ABORT**.<br>- Then, it restarts the instance. | **Depends on mode after restart.** | **Depends on mode after restart.** |


✅**Database configuration**
````sql
SELECT name, database_role, open_mode, log_mode, flashback_on FROM v$database;
````


# 📌 ControlFiles
✅**Control File Location**
````sql
SELECT NAME FROM V$CONTROLFILE;
SHOW PARAMETER CONTROL_FILES;
````
RMAN
````
RMAN> SHOW CONTROLFILE AUTOBACKUP;
RMAN> SHOW PARAMETER CONTROL_FILE_RECORD_KEEP_TIME;
RMAN> SHOW PARAMETER DB_RECOVERY_FILE_DEST;
````
✅**Control File Backup**
````sql
ALTER DATABASE BACKUP CONTROLFILE TO '/ruta/backup_control.ctl';
````
✅**RMAN  Control File Backup**
Si quieres ver en qué backups se ha guardado el Control File, ejecuta en RMAN:
````rman
LIST BACKUP OF CONTROLFILE;
````
````rman
LIST BACKUP OF CONTROLFILE;
````


# 📌 Restore Database

## 1. **(Original Server) Get DBID from the Original Database**
   On the original db get the dbid.
   DBID appears as a header *"connected to target database: DBNAME (**DBID=############**)"*
   ````rman
   rman target /   
   ````
## 2. **(Restore Server) Copy original pfile in  $ORACLE_HOME/dbs/initDBNAME.ora**
   - Delete internal parameters (--)
   - Ajust these parameters (this values are minimun required)
````sql
*.pga_aggregate_limit=2G
*.pga_aggregate_target=1G
*.sga_max_size=2G
*.sga_target=2G
*.processes=300
````
## 3. **(Restore Server) Restore database**
🔹 **Firstly, Restore Controlfile**
````sql
rman target / catalog /@rmancat
startup nomount;
set dbid #############;  
list backup;
restore controlfile;
exit
````
🔹 **Secondly, Restore Controlfile**
````sql
rman target /
restore database;
recover database;
exit;
````
🔹 **Thirdly, open database resetlogs and restart db**
````sql
sqlplus / as sysdba
alter database open resetlogs;
shutdown immadiate;
startup;
````
🔹 **Fourthly, create a new DBID for the restored database**

**Get the initial dbid**
````sql
SELECT DBID FROM V$DATABASE;
````
**Stop and start un mount**
````sql
shutdown immediate;
startup mount;
````
 ⚠️**Generate a new dbid. It will stop the instance**
````bash
nid TARGET=/
````
🚀**Open database with openreset logs**
````sql
alter database open resetlogs;
````
**Check new dbid**
````bash
rman target /
````
🔹 **Fifthly, Recreate password file**
````bash
 /u01/app/oracle/product/19.21.0.0/cdbname/bin/orapwd file=/u01/app/oracle/product/19.21.0.0/cdbname/dbs/orapwcdbname password=SYSPASSWD force=y
````

# 📌 Redolog
**list redologs**
````sql
select * from v$log order by group#;
select * from v$logfile order by griup#;
````

**Add redolog**
````sql
ALTER DATABASE ADD LOGFILE MEMBER'/path/xxx/redo01_b.log' TO GROUP 1;
````

**Forces a log switch in the database**
````sql
ALTER DATABASE SWITCH LOGFILE;
````
**Forces an immediate checkpoint**

- Solo se puede ejecutar cuando la base de datos está abierta.
- Función: Obliga a Oracle a escribir todos los buffers modificados (dirty buffers) en disco y a actualizar la información de checkpoint en el archivo de control, asegurando que la base de datos tenga un punto consistente en disco.
````sql
ALTER SYSTEM CHECKPOINT;
````
- Uso: Se puede utilizar tanto cuando la base de datos está montada (antes de abrirla) como cuando está abierta.
- Función: Realiza la misma operación que ALTER SYSTEM CHECKPOINT, forzando la escritura de la memoria caché en los datafiles y actualizando el checkpoint. Esto resulta especialmente útil cuando la base de datos se encuentra en modo MOUNT y aún no está abierta.
````sql
ALTER DATABASE CHECKPOINT;
````

# 📌 Archives

**Check if databse is in archivelog mode**
````sql
archive log list;
select log_mode from v$database;

````
**Set database in archivelog mode**
During a checkpoint, Oracle writes all dirty buffers (modified data in memory) to the datafiles and updates the checkpoint information in the control file.
````sql
alter database archivelog;
````

**Define FRA to allow online rman backup**
````sql
ALTER SYSTEM SET DB_RECOVERY_FILE_DEST_SIZE=10G SPCOPE=BOTH;
ALTER SYSTEM SET DB_RECOVERY_FILE_DEST = '/db/oradata/fra' SCOPE=BOTH;
````

