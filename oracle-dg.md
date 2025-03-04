# Config Oracle Data Guard
## PRIMARY SIDE - Dataguard configuration steps Primary side


## STANDBY SIDE - Configuración de la Base de Datos Standby en Oracle Data Guard

### 1. Verificación del archivo de inicialización en la Standby
````
cat /u02/app/oracle/product/19.3.0/db_home/dbs/initdigitaldr.ora
db_name=digital
enable_pluggable_database=true
````

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
