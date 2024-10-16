---
title: "ORA-38760: This database instance failed to turn on flashback database"
description: "ORA-38760 Solution"
date: "2024-01-16"
tags: ["new", "rhel", "vmware","oracle"]
draft: false
---

## Problem:

ORA-38760: This database instance failed to turn on flashback database


The flashback logs were accidentally deleted and when you tried to open the database, resulting in ORA-38760, leaving the database in mount mode.

```bash
alter database open;
```

alter database open

>ERROR at line 1:ORA-38760: This database instance failed to turn on flashback database


Attempting to turn off/on flashback also failed.

```bash
alter database flashback off;
```

Database altered.
```bash
alter database flashback on;
```

alter database flashback on
*
ERROR at line 1:
ORA-38706: Cannot turn on FLASHBACK DATABASE logging.


FLASHBACK_ON has a value of "RESTORE POINT ONLY"


```bash
select flashback_on from v$database;
```

```bash
FLASHBACK_ON
----------------------------
RESTORE POINT ONLY
```
You get errors below when you tried to query v$restore_point.

```sql
select * from v$restore_point;
```

> ERROR at line 1:
>ORA-38701: Flashback database log 1 seq 1 thread 1:
>"+FLASH/prod/flashback/log_1.289.1004540057"
>ORA-17503: ksfdopn:2 Failed to open file
>+FLASH/prod/flashback/log_1.289.1004540057
>ORA-15012: ASM file '+FLASH/prod/flashback/log_1.289.1004540057' >does not exist

## Solution:

Use RMAN to find the name of the restore point.

```sql
RMAN> list restore point all;
```

```sql
using target database control file instead of recovery catalog
SCN              RSP Time  Type       Time      Name
---------------- --------- ---------- --------- ----
15462819861                GUARANTEED 02-APR-19 PRE_SWITCH
```



In SQLPLUS, drop the restore point you found from RMAN above.

```sql
SQL> drop restore point PRE_SWITCH;
```
Restore point dropped.




Now open the database.

```sql
SQL> alter database open;
```
Database altered.