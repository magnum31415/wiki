# Restore database

## Define 

* ORIGIN SERVERS: db.mycompany.com
* DESTINATION SERVERS: dbrestoring.mycompany.com


## 1. (Original Server) Previous tasks

**Get the original database size_gb**

````sql
SQL> show con_name;

CON_NAME
------------------------------
DBNAME

SQL> SELECT ROUND(SUM(bytes) / 1024 / 1024 / 1024, 2) AS size_gb FROM dba_data_files;

   SIZE_GB
----------
      3.59
Disconnected from Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.21.0.0.0
````


## 2. (Original Server) Veeam plugin agent

* If It is required to config the veeam plugin to access rman backup: Install and configure Veeam Plug-in for Oracle RMAN 
* To enable access to rman data from another server  copy: /opt/veeam/VeeamPluginforOracleRMAN/veeam_config.xml on new servers  

**Backup**
````bash 
cp /opt/veeam/VeeamPluginforOracleRMAN/veeam_config.xml{,.`date +%F`}
cat /opt/veeam/VeeamPluginforOracleRMAN/veeam_config.xml
<Config>
    <CAVerificationParameters />
    <AdditionalCertificatePaths />
    <ProxyConfig />
    <VBRConnectionParams vbrHostName="DC-REPO1" vbrPort="10006" vbrUser="rman_DBNAME" vbrDomain="" vbrPassword="TopSecretPasswd==" />
    <Certificate />
    <BackupForRestoreParams />
    <AgentParams />
    <RepositoryParams parallelism="1">
        <Repository repositoryName="DC-REPO1" repositoryID="445242435346y54624245245" />
    </RepositoryParams>
    <PluginParameters customServerName="db.mycompany.com" />
</Config>

OracleRMANConfigTool --show-config

OracleRMANConfigTool --set-credentials "DC-REPO1\rman_DBNAME" "TopSecretPasswd"
Please ensure the following roles are assigned to the user in Users and Roles settings on the backup server:
Backup Administrator, or
Backup Operator and Restore Operator

Alternatively, select a backup repository that does not have existing application backups created from this server under another user account.

Proceed anyway? (y/N): y
````
**Check if we can access to backups**
````rman
rman target / 
list backups;
````

## 3. (Original Server) Get DBID and Oracle Version


**Get DBID**
````
rman target /

Recovery Manager: Release 19.0.0.0.0 - Production on Tue Jul 8 08:31:21 2025
Version 19.21.0.0.0

Copyright (c) 1982, 2019, Oracle and/or its affiliates.  All rights reserved.

connected to target database: CDBNAME (DBID=1836280148)
````

**Get Oracle Version**

````
SQL> SELECT * FROM product_component_version WHERE product LIKE 'Oracle%';
PRODUCT
Oracle Database 19c Enterprise Edition
19.0.0.0.0
19.21.0.0.0
Production
````

**IMPORTANT:** The same Oracle Version mus be installed in the DESTINATION SERVER dbrestoring.mycompany.com


## 4. (Original Server) Copy original initDBNAME.ora to the restore server

**Define correct ENV variables**
````bash
export ORACLE_HOME=/u01/app/oracle/product/19.21.0.0/cDBNAME
export ORACLE_SID=cDBNAME
export ORACLE_BASE=/u01/app/oracle
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/opt/veeam/VeeamPluginOracle/lib:$LD_LIBRARY_PATH
````

**Create required dirs**
````bash
mkdir  $ORACLE_HOME
mkdir  $ORACLE_HOME/dbs
````

**Copy init file from origin server to destination server**
````bash
scp initcDBNAME.ora dbrestoring.mycompany.com:/u01/app/oracle/product/19.21.0.0/cDBNAME/dbs/initcDBNAME.ora
````

**Adjust init file, deleting utotuning parameters “__” and internal parameters “_”**

````bash
##adjust with a minimal amount of resources
*.pga_aggregate_limit=2G
*.pga_aggregate_target=1G
*.sga_max_size=2G
*.sga_target=2G
*.processes=300
````


**Copy PASSWORD FILE with SID**
````bash
cd $ORACLE_HOME/dbs
cp orapwcORIGINAL orapwcDBNAME
````


## 3. (Restorte Server) Copy original initDBNAME.ora to the restore server

### 🔧 Firstly, Restore Controlfile
````bash
rman target / catalog /@rmancat
startup nomount;
set dbid 1836280148;  
list backup;
restore controlfile;
exit
````

### ♻️  Secondly, Restore and recover database
````bash
rman target /
startuo mount;
restore database;
recover database;
exit;
````

### 🩺 Thirdly, open database resetlogs and restart db
````bash
sqlplus / as sysdba
alter database open resetlogs;
shutdown immadiate;
startup;
````
### 🚀 Fourthly, create a new DBID for the restored database
````bash
Get the initial dbid
SELECT DBID FROM V$DATABASE;
Stop and start un mount
shutdown immediate;
startup mount;
````
Generate a new dbid. It will stop the instance
````bash
nid TARGET=/
Open database with openreset logs
alter database open resetlogs;
````
Check new dbid
````bash
rman target /
````
### 🔒 Fifthly, Recreate password file
````bash
/u01/app/oracle/product/19.21.0.0/cdbname/bin/orapwd file=/u01/app/oracle/product/19.21.0.0/cdbname/dbs/orapwcdbname password=SYSPASSWD force=y
````

