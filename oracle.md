

- [Oracle DataGuard Configuration](https://github.com/magnum31415/wiki/blob/main/oracle-dg.md)
- [Oracle DataGuard SwitchOver](https://github.com/magnum31415/wiki/blob/main/oracle-dg-switchover.md)
- [Oracle Install](https://github.com/magnum31415/wiki/blob/main/oracle-install.md)
- [Oracle TDE](https://github.com/magnum31415/wiki/blob/main/oracle-tde.md)
- [Oracle Queries](https://github.com/magnum31415/wiki/blob/main/oracle-queries.md)
- [Oracle RMAN](https://github.com/magnum31415/wiki/blob/main/oracle-rman.md)
- [Oracle RMAN RESTORE](https://github.com/magnum31415/wiki/blob/main/oracle-RMAN-Restore.md) 


## ✅Improve user exprience with sqlplus

rlwrap with sqlplus: ``rlwrap sqlplus scott/tiger@orcl``

**rlwrap** This command invokes the wrapper that adds readline functionalities, such as command history, line editing, and autocomplete.
sqlplus scott/tiger@orcl: This starts SQL*Plus, connecting with the username "scott", password "tiger" to the database identified as "orcl".
Using this command, SQL*Plus will now support:

Navigating through command history using the arrow keys.
* Interactive editing of the command line.
* An overall more comfortable experience similar to shells that have native readline support.
* This is especially useful when SQL*Plus doesn't natively include these features.

## ✅display the elapsed time for each command executed

````sql
SET TIMING ON
SET TIMING OFF
````

## ✅spool commands and output
````sql
-- Configura el entorno para mostrar los comandos
SET ECHO ON         -- Muestra los comandos ejecutados
SET FEEDBACK ON     -- Muestra cuántas filas se seleccionaron/afectaron
SET TERMOUT ON      -- Muestra resultados en pantalla
SET VERIFY ON       -- Muestra sustituciones si usas variables
SET TIMING ON       -- (opcional) Muestra tiempo de ejecución

SPOOL /tmp/mi_log.txt


SPOOL OFF
````

## ✅Oracle error message lookup utility
- **facility** → The component generating the error (e.g., ORA, TNS, EXP, IMP).
- **error_number** → The numerical part of the Oracle error code.
````bash
oerr <facility> <error_number>
oerr ora 16698
````
## ✅Database startup
| Estado        | Qué hay levantado                         | Qué puedes hacer (ejemplos)                                                                                                   | RMAN / Recuperación                                                                                         | Vistas útiles disponibles                                                                                                    | No puedes                                                |
|---------------|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------|
| **NOMOUNT**   | Instancia (SGA + procesos). **Sin** controlfile. | `CREATE DATABASE`, `CREATE CONTROLFILE`; crear/usar **SPFILE/PFILE**; cambiar parámetros; autenticar con password file.       | **Restaurar controlfile** desde backup/autobackup (`RESTORE CONTROLFILE`), setear `DBID`.                   | `V$INSTANCE`, `V$PARAMETER`, `V$SPPARAMETER`, `V$PROCESS`, `V$OPTION`…                                                      | Acceso a datos/objetos; ver metadatos que requieren controlfile; abrir DB. |
| **MOUNT**     | Instancia + **controlfile montado**. **Sin** datafiles abiertos. | Cambiar a `ARCHIVELOG/NOARCHIVELOG`; gestionar **datafiles** y **redo logs** (`ALTER DATABASE RENAME FILE`); standby ops.     | **RESTORE/RECOVER DATABASE** completo; restaurar datafiles; recuperación hasta SCN/fecha/log; abrir con `RESETLOGS`.         | + a NOMOUNT: `V$DATABASE`, `V$CONTROLFILE`, `V$DATAFILE`, `V$TABLESPACE`, `V$LOGFILE`, `V$ARCHIVE_DEST`, `V$RECOVERY_FILE_DEST`… | Consultar/usar objetos; DML/DDL; servicios para usuarios. |
| **OPEN**      | Instancia + controlfile + **datafiles abiertos**. | Operación normal: DDL/DML; crear/alter/drop **tablespaces**, usuarios, índices; estadísticas; jobs; TDE; AWR/ADDM; servicios. | Backups **en caliente** (ONLINE) en ARCHIVELOG; recover tablespace/datafile/block online; duplicaciones DB abierta; flashback. | Todo lo anterior + diccionario completo: `DBA_*`, `ALL_*`, `CDB_*`, `V$*`, `GV$*`                                           | Acciones que exijan MOUNT (ej. cambiar a ARCHIVELOG).    |

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





