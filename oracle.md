

- [Oracle DataGuard Configuration](https://github.com/magnum31415/wiki/blob/main/oracle-dg.md)
- [Oracle DataGuard SwitchOver](https://github.com/magnum31415/wiki/blob/main/oracle-dg-switchover.md)
- [Oracle Install](https://github.com/magnum31415/wiki/blob/main/oracle-install.md)
- [Oracle TDE](https://github.com/magnum31415/wiki/blob/main/oracle-tde.md)

## ✅Improve user exprience with sqlplus

rlwrap with sqlplus: ``rlwrap sqlplus scott/tiger@orcl``

**rlwrap** This command invokes the wrapper that adds readline functionalities, such as command history, line editing, and autocomplete.
sqlplus scott/tiger@orcl: This starts SQL*Plus, connecting with the username "scott", password "tiger" to the database identified as "orcl".
Using this command, SQL*Plus will now support:

Navigating through command history using the arrow keys.
* Interactive editing of the command line.
* An overall more comfortable experience similar to shells that have native readline support.
* This is especially useful when SQL*Plus doesn't natively include these features.

## ✅Multithenant CDB & PDB
* Verificar el contenedor actual: ``SHOW CON_NAME;``
* Cambiar la sesión al PDB "DB1": ``ALTER SESSION SET CONTAINER = DB1;``
* Cambiar del PDB actual a la base de datos contenedora (CDB): ``ALTER SESSION SET CONTAINER = CDB$ROOT;``
* Abrir el PDB "DB1": ``ALTER PLUGGABLE DATABASE DB1 OPEN;``
* Ver los PDBs disponibles: ``SHOW PDBS;`` o  ``SELECT name, open_mode FROM v$pdbs;``
* Si deseas cerrar solo un PDB y mantener el CDB activo, conéctate al CDB y ejecuta: ``ALTER PLUGGABLE DATABASE <nombre_del_PDB> CLOSE;``

## ✅Listener

*Parar Listener: ``/u01/app/oracle/product/19.21.0.0/CDB1/bin/lsnrctl stop LISTENER_CDB1``

## ✅Archive Logs space

### Check the Archive Destination Parameter

This command will display the values of parameters like LOG_ARCHIVE_DEST_1, LOG_ARCHIVE_DEST_2, etc. These parameters indicate the directory paths where the archive logs are being stored.
````sql
SHOW PARAMETER log_archive_dest;
````
### Examine the Archive Log List
````sql
ARCHIVE LOG LIST;
````
If the output shows Archive destination: USE_DB_RECOVERY_FILE_DEST, it means the archive logs are stored in the Fast Recovery Area.

### Check the Fast Recovery Area

If your archive logs are using the Fast Recovery Area, verify the actual directory by running:
The value returned is the physical directory used by the Fast Recovery Area where your archive logs are stored.
````sql
SHOW PARAMETER db_recovery_file_dest;
````

### Check Log Status
You can query the database to review the status of each archive log:

````sql
SELECT thread#, sequence#, applied
FROM v$archived_log
ORDER BY sequence#;
````
The **APPLIED** column helps you determine if the logs have been applied in a Data Guard environment or are no longer needed.

### Consider Fast Recovery Area (FRA) Management
If your archive logs are stored in the Fast Recovery Area, Oracle automatically manages the deletion of old files based on space and retention policies. You can check the FRA details with:

````sql
SELECT space_limit, space_used, space_reclaimable, number_of_files
FROM v$recovery_file_dest;
````

## ✅Oracle Filesystem Standard Organization

🚩 Typical Directory Layout Summary
Directory	Typical Use	Example
| Directory | Typical Use                                 | Example                                      |
|-----------|---------------------------------------------|----------------------------------------------|
| `/u01`    | Oracle software binaries                    | `/u01/app/oracle/product/19.21.0/dbhome_1`   |
| `/u02`    | Database datafiles & redo logs              | `/u02/oradata/ctest/`                        |
| `/u03`    | Archived redo logs                          | `/u03/archivelog/ctest/`                     |
| `/u04`    | RMAN backup files                           | `/u04/backup/ctest/`                         |
| `/u05`    | Fast Recovery Area (FRA) or miscellaneous   | `/u05/fra/ctest/`                            |


1. **/u01 – Oracle Software Binaries**  ``/u01/app/oracle/product/<version>/dbhome``
2. **/u02 – Oracle Database Files (Datafiles)** ``/u02/oradata/<dbname>/``
    Stores database files such as:
    - Data files (.dbf)
    - Control files (control01.ctl, etc.)
    - Online redo log files (redo01.log, etc.)
3. **/u03 – Oracle Archived Redo Logs** ``/u03/archivelog/<dbname>/``
4. **/u04 – Oracle Backups (RMAN Backup Files)**  ``/u04/backup/<dbname>/``
5. **/u05 – Miscellaneous or Flash Recovery Area (FRA)** ``/u05/fra/<dbname>/``
   Often designated for the Flash Recovery Area (FRA), which holds:
   - Flashback logs ``/u05/fra/ctest/flashbacklog/``
   - Backup files (if configured to use FRA)
   - Archived logs (if configured to use FRA) ``/u05/fra/ctest/archivelog/``


## ✅Oracle RedoLog

**Why Multiplex redo log files?**
- **Enhanced protection:** If a redo log file becomes corrupt or is accidentally deleted, the database can seamlessly continue operations using the redundant copy.
- **Performance benefit:** Having redo logs on separate disks can slightly enhance write performance.
  
**Best Practices:**
- Ensure each redo log group has at least two members on separate storage locations.
- Use clear naming conventions (redoXXa.log, redoXXb.log) to quickly identify multiplexed members.

**What is Oracle Redo Log Multiplexing?**
- Redo log multiplexing means that each redo log group has two or more identical redo log files placed in separate physical locations or disks. This redundancy protects against potential loss or corruption of redo - logs, greatly improving database recoverability.

Typically, multiplexed redo log files are identified clearly using naming conventions such as:

- **Group 1:** redo01a.log, redo01b.log
- **Group 2:** redo02a.log, redo02b.log

Here, you clearly have  (files ending with a.log and b.log stored separately in /u02 and /u03):
````
"a" logs on /u02/oradata/ctest
"b" logs on /u03/fra
````

**Periodically verify status using:**
````sql
SELECT GROUP#, MEMBER FROM V$LOGFILE ORDER BY GROUP#;
SELECT GROUP#, STATUS FROM V$LOG;
````





