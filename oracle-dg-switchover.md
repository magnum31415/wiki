# ✅Oracle DataGuard SwitchOver
## DGMGRL

**📝 Quick Summary of Steps:**
1. Connect: dgmgrl /
2. Check status: show configuration;
3. Switchover: switchover to db_standby;
4. Confirm new roles: show configuration;
5. Confirm via SQL (optional): select database_role from v$database;

### Step 1: Connect to the Data Guard Broker (from any node)
````bash
dgmgrl /
````
### Step 2: Check current configuration
````sql
DGMGRL> SHOW CONFIGURATION;
````
You should see a result similar to:

````yaml
Configuration - DG_Config

  Protection Mode: MaxPerformance
  Members:
  db_primary - Primary database
    db_standby - Physical standby database

Fast-Start Failover: DISABLED

Configuration Status:
SUCCESS
````

Ensure everything shows as **SUCCESS**.

### Step 3: Initiate the Switchover
Run the following command to perform switchover (assuming db_standby is your standby):

````sql
DGMGRL> SWITCHOVER TO db_standby;
````
This command will automatically:

- Convert your current primary into standby.
- Convert your standby into the new primary.
- Handle shutdown/startup sequences automatically.
- 
Note: If sessions are active and it doesn't allow switchover, use:

````sql
DGMGRL> switchover to db_standby force;
````

### Step 4: Verify the new configuration
Check the configuration again after completion:

````sql
DGMGRL> show configuration;
````

It should look similar to this:

````yaml
Configuration - DG_Config

  Protection Mode: MaxPerformance
  Members:
  db_standby - Primary database  <-- (now primary)
    db_primary - Physical standby database  <-- now standby

Fast-Start Failover: DISABLED

Configuration Status:
SUCCESS
````

### Step 5: (Optional) Verify Database Roles Using SQL
Connect to both databases and confirm their roles:

````sql
SELECT DATABASE_ROLE FROM V$DATABASE;
````
On the new primary, the result must be:

````sql
DATABASE_ROLE
---------------
PRIMARY
````
On the new standby, the result must be:

````sql
DATABASE_ROLE
-----------------
PHYSICAL STANDBY
````

### 🔧 Common troubleshooting tips:
Check alert.log files for any errors if something goes wrong during switchover.
Ensure both databases are communicating correctly before and after switchover.



# ✅Starting the Oracle Standby Database (Data Guard)
Perform these steps on your standby database server:

## Quick summary (commands to run):
````sql
sqlplus / as sysdba
STARTUP MOUNT;
ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;
SELECT PROCESS, STATUS FROM V$MANAGED_STANDBY WHERE PROCESS LIKE 'MRP%';
````
Your standby database should now be up and running, applying logs automatically from the primary.


## Step 1: Start the instance in MOUNT mode
From your Linux shell:

````bash
sqlplus / as sysdba
````
Then, inside SQL*Plus:

````sql
STARTUP MOUNT;
````
This command will start the Oracle database instance and mount the database (required for standby).

## Step 2: Verify and Start (if it is stopped) the Managed Recovery Process (MRP)
Check

````sql
 SELECT PROCESS, STATUS FROM V$MANAGED_STANDBY WHERE PROCESS LIKE 'MRP%';
--or
 SELECT PROCESS, STATUS, THREAD#, SEQUENCE#, BLOCK# FROM V$MANAGED_STANDBY;
````

Run this SQL command to activate managed recovery (apply redo logs automatically):

````sql
ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;
````
This starts the Managed Recovery Process (MRP) and allows the standby database to apply redo logs received from the primary automatically.

## Step 3: Verify MRP is running correctly
Check the status of MRP by running:

````sql
SELECT PROCESS, STATUS FROM V$MANAGED_STANDBY WHERE PROCESS LIKE 'MRP%';
````
You should see output similar to:

````
PROCESS   STATUS
--------- --------------------
MRP0      APPLYING_LOG
````

This confirms that your standby database is applying logs correctly.

## Step 4: Confirm database status
Confirm the current role and status of the standby database:

````sql
SELECT DATABASE_ROLE, OPEN_MODE FROM V$DATABASE;
````

Expected result:

````
DATABASE_ROLE      OPEN_MODE
------------------ --------------------
PHYSICAL STANDBY   MOUNTED
````





