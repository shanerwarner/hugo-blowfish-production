---
title: "Enabling Archive Log Mode in Oracle 19c RAC Database "
date: 2024-01-09
draft: false
description: "In this article, we will go through the steps to enable Archive Log Mode in an Oracle 19c RAC (Real Application Clusters) database."
tags: ["new", "docs", "vmware"]
showRecent : true
showHero: false
showAuthor: true
showDate: true
showSummary: true
---


# In this article, we will go through the steps to enable Archive Log Mode in an Oracle 19c RAC (Real Application Clusters) database.

## Step 1: Check Current Archive Log Mode Status

Before changing the Archive Log mode, check the current status of the database.

```sql
SQL> select log_mode, name from v$database;

LOG_MODE    NAME
----------  --------
NOARCHIVELOG EKONE

SQL> archive log list;

Database log mode              No Archive Mode
Automatic archival             Disabled
Archive destination            USE_DB_RECOVERY_FILE_DEST
Oldest online log sequence      4
Current log sequence            5

SQL> exit
```

The database is currently in **NOARCHIVELOG** mode, and automatic archival is disabled.

## Step 2: Stop the RAC Database Service

Stop the database service before making changes.

```bash
[oracle@rac1 ~]$ srvctl stop database -d EKONE
[oracle@rac1 ~]$ srvctl status database -d EKONE

Instance EKONE1 is not running on node rac1
Instance EKONE2 is not running on node rac2
```

## Step 3: Start the RAC Database in Mount State

Start the database in **mount** state to make the changes.

```bash
[oracle@rac1 ~]$ srvctl start database -d EKONE -o mount
[oracle@rac1 ~]$ srvctl status database -d EKONE

Instance EKONE1 is running on node rac1
Instance EKONE2 is running on node rac2
```

## Step 4: Enable Archive Log Mode and Set Archive Destination

Now, connect to the database as `sysdba` and enable the **ARCHIVELOG** mode, setting the archive destination to an ASM disk group.

```bash
[oracle@rac1 ~]$ sqlplus / as sysdba

SQL> alter system set log_archive_dest_1='LOCATION=+DATA/' scope=both sid='*';

System altered.

SQL> alter database archivelog;

Database altered.
```

## Step 5: Stop the RAC Database Service

After enabling **ARCHIVELOG** mode, stop the database service again.

```bash
[oracle@rac1 ~]$ srvctl stop database -d EKONE
```

## Step 6: Restart the RAC Database

Restart the RAC database to apply the changes.

```bash
[oracle@rac1 ~]$ srvctl start database -d EKONE
[oracle@rac1 ~]$ srvctl status database -d EKONE

Instance EKONE1 is running on node rac1
Instance EKONE2 is running on node rac2
```

## Step 7: Verify Archive Log Mode

Check if the **ARCHIVELOG** mode has been enabled successfully.

```sql
SQL> archive log list;

Database log mode              Archive Mode
Automatic archival             Enabled
Archive destination            +DATA
Oldest online log sequence      4
Next log sequence to archive    5
Current log sequence            5

SQL> select log_mode, name from v$database;

LOG_MODE    NAME
----------  --------
ARCHIVELOG  EKONE
```

The database is now in **ARCHIVELOG** mode, with automatic archival enabled and the archive destination set to the ASM disk group.

---

By following these steps, you've successfully enabled Archive Log mode in your Oracle 19c RAC database. This change helps ensure that your database logs are archived, which is crucial for point-in-time recovery.