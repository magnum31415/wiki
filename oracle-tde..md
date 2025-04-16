#  ✅Oracle TDE (Transparent Data Encryption)
Is an Oracle Advance security feature that help us to protect our data from being stolen. So you may ask why Encryption? Encryption will secure your data by prevent the attacker to steal your data if he gain access through:

- Direct access to database storage
- Theft of database backup
- Compromise of database export

## Setup

-	Keystore: **Oracle Wallet**
-	Wallet Type: **Local auto-login wallet**
-	Encryption Algorithm: **AES256**
-	Mode: **United Mode**
-	Management Tool: **Administer Key Management**
-	Tested Architectures: **Oracle Multitenant (CDB with a single PDB), DataGuard, RMAN**
-	Oracle Releases: **Oracle 19c onwards**
-	Encryption Target: **Application tablespaces (not  system tablespaces: SYSAUX, SYSTEM, TEMP  and UNDO.)**
-	Encrypt Tablespace strategy: **Online**

**Single Key in United Mode:** In a united mode setup, all PDBs share one TDE master key administered at the CDB level. 

Specifying **CONTAINER=ALL** confirms the operation applies across the entire CDB environment.

**Ensures Key Propagation:** Even though in united mode, Oracle defaults to a single key for all PDBs, including CONTAINER=ALL explicitly makes it clear and forces any relevant dictionary updates for all containers in one statement.

**Best Practice:** Oracle documentation recommends using CONTAINER=ALL for TDE key rotation in a multitenant environment to avoid confusion and to make sure the new master key is recognized consistently across all PDBs.


# ✅Procedure for configuring TDE on a Single Database in a multithenant db

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


# ✅Procedure for Tablespace Online Encryption
 **Free Space:** For online encryption having auxiliary space of largest datafile size in the tablespace should be enough

## 1. Encrypt tablespace
````sql
ALTER TABLESPACE users_2_non_enc ENCRYPTION ONLINE USING 'AES256' ENCRYPT;
````
Monitor the progress of tablespace encryption

````sql
Verify 
SELECT sid, serial#, sofar, totalwork, ROUND( (sofar / totalwork) * 100, 2 ) AS pct_complete, elapsed_seconds, time_remaining, opname, message FROM v$session_longops WHERE opname LIKE '%Encrypt%' AND sofar <> totalwork ORDER BY start_time DESC;
````

Tablespaces and temporary encryption (master key ID for temporary tablespaces can be seen in the system tablespace data):
````sql
set linesize 150
column name format a40
column masterkeyid_base64 format a60
select name,utl_raw.cast_to_varchar2( utl_encode.base64_encode('01'||substr(mkeyid,1,4))) || utl_raw.cast_to_varchar2( utl_encode.base64_encode(substr(mkeyid,5,length(mkeyid)))) masterkeyid_base64 FROM (select t.name, RAWTOHEX(x.mkid) mkeyid from v$tablespace t, x$kcbtek x where t.ts#=x.ts#);
````

# ✅ Procedure for Master Encryption Key Rotation (Rekeying)
In Oracle TDE, performing a rekey operation does not physically remove or delete the old master encryption key (MEK) from the keystore. Instead, the rekey operation creates a new master key and then re-wraps (re-encrypts) the existing data encryption keys (DEKs) with this new MEK. The old MEK remains in the keystore as a historical record or backup but is no longer used for encrypting new data. This behavior is by design for audit, compliance, and recovery purposes. In a united mode environment, the rekey operation is performed at the CDB level (with CONTAINER=ALL), ensuring that all PDBs start using the new master key, while the old keys are preserved in the keystore.
Tablespace Data Encryption Key should be encrypted with the new Master Encrytion Key. 


## 1.	Temporarily Disable the Auto-Login Wallet
````bash
mv /u01/app/oracle/dbname/wallet/tde/cwallet.sso /u01/app/oracle/dbname/wallet/tde/cwallet.sso.OLD
````
## 2.	On CDB$ROOT restart the database

````
shutdown immediate ;
startup ;
````

## 3.	On CDB$ROOT open the password-based wallet explicitly:
````sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
ADMINISTER KEY MANAGEMENT 
SET KEYSTORE OPEN 
IDENTIFIED BY "S@cr@tP@ssw@rd"
CONTAINER=ALL;
````

## 4.	On CDB$ROOT check current created Master Encryption KEYS
````
set pages 200
set line 200
col KEY_ID format  a52
col TAG format a31
col creation_time format a17
col activation_time format a17
col con_name format a8
SELECT ek.key_id, ek.tag, ek.creation_time, ek.activation_time,       ek.key_use, ek.keystore_type, ek.backed_up, c.name AS con_name
  FROM v$encryption_keys ek
  JOIN v$containers c ON ek.con_id = c.con_id
 ORDER BY ek.creation_time;
````

## 5.	On CDB$ROOT create a New Master Key “MEK” (rekeying) 
````sql
ADMINISTER KEY MANAGEMENT 
SET ENCRYPTION KEY 
USING TAG 'REKEY_MEK_2025_02_28' 
IDENTIFIED BY "S@cr@tP@ssw@rd" 
WITH BACKUP USING 'REKEY_MEK_2025_02_28' 
CONTAINER=ALL;
````
## 6.	On CDB$ROOT check again Master Encryption KEYS, one new per each PDB must appear.
````sql
set pages 200
set line 200
col KEY_ID format  a52
col TAG format a31
col creation_time format a17
col activation_time format a17
col con_name format a8
SELECT ek.key_id, ek.tag, ek.creation_time, ek.activation_time,       ek.key_use, ek.keystore_type, ek.backed_up, c.name AS con_name
  FROM v$encryption_keys ek
  JOIN v$containers c ON ek.con_id = c.con_id
 ORDER BY ek.creation_time;
````

## 7.	On CDB$ROOT Recreate the auto-login wallet once your operation is complete
````
ADMINISTER KEY MANAGEMENT 
CREATE AUTO_LOGIN KEYSTORE 
FROM KEYSTORE '/u01/app/oracle/synlab/wallet/tde' 
IDENTIFIED BY "S@cr@tP@ssw@rd";
````





[Oracle Quick TDE Setup and FAQ Doc ID 1251597.1](https://support.oracle.com/epmos/faces/DocumentDisplay?parent=SrDetailText&sourceId=3-39774480751&id=1251597.1)

