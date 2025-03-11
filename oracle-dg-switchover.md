# Oracle DataGuard SwitchOver
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






## SQLPLUS

## 1. Check the status of both Primary and Standby
**On the Primary database**, run:

````sql
SELECT SWITCHOVER_STATUS FROM V$DATABASE;
````
Expected result:
- TO STANDBY or SESSIONS ACTIVE is acceptable.
- If you see SESSIONS ACTIVE, you might need to shut down sessions or wait briefly.

**On the Standby database**, run:

````sql
SELECT SWITCHOVER_STATUS FROM V$DATABASE;
````
Expected result:
-TO PRIMARY or NOT ALLOWED (if it shows NOT ALLOWED, re-check configuration).
