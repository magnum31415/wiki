#  ✅Oracle TDE (Transparent Data Encryption)
Is an Oracle Advance security feature that help us to protect our data from being stolen. So you may ask why Encryption? Encryption will secure your data by prevent the attacker to steal your data if he gain access through:

- Direct access to database storage
- Theft of database backup
- Compromise of database export

# ✅Configuring TDE on a Single Database in a multithenant db

## 1. Set the default Parameters

Before doing any changes we will make a backup of spfile.
````sql
SHOW PARAMETER pfile;
CREATE PFILE='${ORACLE_HOME}/dbs/initSID_YYYYMMDD_HHMM.ora' FROM SPFILE;
````

Encrypt by default all new tablespaces 
````sql
ALTER SYSTEM SET ENCRYPT_NEW_TABLESPACES = 'ALWAYS' SCOPE = SPFILE SID = '*';
````
and define the encryption algorithm
````sql
ALTER SYSTEM SET "_tablespace_encryption_default_algorithm" = "AES256" SCOPE=SPFILE SID='*’
````

## 2.	On CDB$ROOT set WALLET_ROOT (Specifies the top directory for keystore files). 
````sql
ALTER SESSION SET CONTAINER = CDB$ROOT;

ALTER SYSTEM SET wallet_root='/u01/app/oracle/dbname/wallet' SCOPE=SPFILE;
````
You can verify it after database restart:
````sql
SHOW PARAMETER wallet_root;
````

## 3.	On CDB$ROOT restart database performing  a shutdown and startup:
````sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
SHUTDOWN IMMEDIATE;
STARTUP;
````
 
## 4.	On CDB$ROOT set TDE_CONFIGURATION (Specifies the keystore configuration type (set to FILE for Oracle Wallet).
````sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
ALTER SYSTEM SET tde_configuration='KEYSTORE_CONFIGURATION=FILE' SCOPE=BOTH;
````

Verify with:
````sql
SHOW PARAMETER tde_configuration;
````
## 5.	Create TDE directory
````
mkdir -p /u01/app/oracle/dbname/wallet/tde
chown oracle:oinstall  /u01/app/oracle/dbname/wallet/tde
````


## 6.	On CDB$ROOT Create Keystore 
````sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
ADMINISTER KEY MANAGEMENT 
CREATE KEYSTORE 
'/u01/app/oracle/dbname/wallet/tde' 
IDENTIFIED BY "S@cr@tP@ssw@rd";
````
Current wallet STATUS should be CLOSED
 

## 7.	On CDB$ROOT Open Keystore on all CONTAINERS
IMPORTANT: before executing these commands ensure the PDB_NAME is in STATUS=OPEN. 

````sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
ADMINISTER KEY MANAGEMENT 
SET KEYSTORE OPEN 
IDENTIFIED BY "S@cr@tP@ssw@rd"
CONTAINER=ALL;
````
Current wallet STATUS should be OPEN_NO_MASTER_KEY
 

## 8.	On CDB$ROOT create new Master Encryption Key on all CONTAINERS
````sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
ADMINISTER KEY MANAGEMENT 
SET KEY 
USING TAG "INITIAL_MASTER_KEY" 
IDENTIFIED BY "S@cr@tP@ssw@rd" 
WITH BACKUP USING "INITIAL-MEK"
CONTAINER=ALL;
````
Verify the wallet STATUS it should be  OPEN
````sql
SELECT * FROM v$encryption_wallet;
```` 

Verify a new Master Encryption Key is created.
````sql
set pages 200
set line 200
col KEY_ID format  a52
col TAG format a31
col creation_time format a17
col activation_time format a17
col con_name format a8
SELECT ek.key_id,ek.tag, ek.creation_time, ek.activation_time, ek.key_use, ek.keystore_type,       ek.backed_up, c.name AS con_name
  FROM v$encryption_keys ek
  JOIN v$containers c ON ek.con_id = c.con_id
 ORDER BY ek.creation_time;
 ````


## 9.	On CDB$ROOT create auto-login Keystore 
````sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
ADMINISTER KEY MANAGEMENT 
CREATE AUTO_LOGIN KEYSTORE 
FROM KEYSTORE '/u01/app/oracle/dbname/wallet/tde' 
IDENTIFIED BY "S@cr@tP@ssw@rd";
````
Verify a new file cwallet.sso has been created.
````bash
ls -la /u01/app/oracle/dbname/wallet/tde/cwallet.sso
````
Note: LOCAL enables you to create a local auto-login software keystore. Otherwise, omit this clause if you want the keystore to be accessible by other computers. LOCAL creates a local auto-login wallet file, cwallet.sso, and this wallet will be tied to the host on which it was created






[Oracle Quick TDE Setup and FAQ Doc ID 1251597.1](https://support.oracle.com/epmos/faces/DocumentDisplay?parent=SrDetailText&sourceId=3-39774480751&id=1251597.1)

