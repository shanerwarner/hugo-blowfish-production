---
title: "Essential CRSCTL Commands for Oracle Clusterware Management"
description: "Oracle RAC CRSCTL Commands"
date: "2024-10-10"
tags: ["new", "rac", "vmware","oracle"]
draft: false
---

The `CRSCTL` (Cluster Ready Services Control) utility is an important tool used to manage Oracle Clusterware resources and components. Below is a comprehensive guide to some of the most useful `CRSCTL` commands for Oracle DBAs.

## 1. Start and Stop CRS (Run as Root)

These commands allow you to start or stop the Cluster Ready Services (CRS).

```bash
$GRID_HOME/bin/crsctl stop crs
$GRID_HOME/bin/crsctl start crs
```

## 2. Enable or Disable Auto-Restart of CRS

Control the automatic restart of CRS on system boot.

```bash
$GRID_HOME/bin/crsctl disable crs
$GRID_HOME/bin/crsctl enable crs
```

## 3. Find the Cluster Name

Retrieve the name of the cluster using the following commands.

```bash
$GRID_HOME/bin/cemutlo -n
$GRID_HOME/bin/olsnodes -c
```

## 4. Check Grid Version

Find the software version of the Grid Infrastructure.

```bash
$GRID_HOME/bin/crsctl query crs softwareversion <hostname>
```

Example:

```bash
$GRID_HOME/bin/crsctl query crs softwareversion host-dbaclass1
```

## 5. Check Cluster Component Status

Use these commands to check the status of various Clusterware components.

```bash
$GRID_HOME/bin/crsctl stat res -t
$GRID_HOME/bin/crsctl check crs
$GRID_HOME/bin/crsctl check cssd
$GRID_HOME/bin/crsctl check crsd
$GRID_HOME/bin/crsctl check evmd
```

## 6. Find Voting Disk Location

Locate the voting disks used in the cluster.

```bash
$GRID_HOME/bin/crsctl query css votedisk
```

## 7. Find OCR Location

Check the Oracle Cluster Registry (OCR) location.

```bash
$GRID_HOME/bin/ocrcheck
```

## 8. View Cluster Interconnect Details

Retrieve details of the cluster interconnect.

```bash
$GRID_HOME/bin/oifcfg getif
```

Sample output:

```bash
app-ipmp0 172.21.39.128 global public
loypredbib0 172.16.3.192 global cluster_interconnect
loypredbib1 172.16.4.0 global cluster_interconnect
```

You can also query the interconnect IP addresses from the database.

```sql
SELECT NAME, IP_ADDRESS FROM v$cluster_interconnects;
```

## 9. Check CRS Status on Local Node

Verify the status of CRS on the local node.

```bash
crsctl check crs
```

Example output:

```bash
CRS-4638: Oracle High Availability Services is online
CRS-4537: Cluster Ready Services is online
CRS-4529: Cluster Synchronization Services is online
CRS-4533: Event Manager is online
```

## 10. Check Status of All CRS Resources

View the status of all Clusterware resources.

```bash
$GRID_HOME/bin/crsctl stat res -t
$GRID_HOME/bin/crsctl stat res -t -init
```

## 11. Check Active Cluster Version

Get the active version of the cluster.

```bash
crsctl query crs activeversion
```

Example output:

```bash
Oracle Clusterware active version on the cluster is [12.1.0.2.0]
```

## 12. Start and Stop High Availability Service (HAS)

Use these commands to manage the Oracle High Availability Services.

```bash
crsctl stop has
crsctl start has
```

## 13. Check CRS Status on Remote Nodes

Check the status of CRS on all cluster nodes.

```bash
crsctl check cluster
crsctl check cluster -all
```

## 14. View Disk Timeout Settings

Check the timeout setting for disk communication to the voting disk.

```bash
crsctl get css disktimeout
```

Example output:

```bash
CRS-4678: Successful get disktimeout 200 for Cluster Synchronization Services.
```

## 15. Check Network Latency (Misscount)

Check the network latency between nodes in the interconnect.

```bash
crsctl get css misscount
```

Example output:

```bash
CRS-4678: Successful get misscount 30 for Cluster Synchronization Services.
```

## 16. Move Voting Disk to Another Disk Group

Move the voting disk to a new disk group.

```bash
crsctl replace votedisk +OCRVD
```

Example output:

```bash
CRS-4266: Voting file(s) successfully replaced
```

## 17. Add a New Voting Disk

Add a new voting disk to the cluster.

```bash
crsctl add css votedisk
```

## 18. Delete a Voting Disk

Remove a voting disk from the cluster.

```bash
crsctl delete css votedisk
```

## 19. View OCR Backup Information

Get details about OCR backups.

```bash
ocrconfig -showbackup
```

## 20. Check Cluster Mode

Determine whether the cluster is running in **standard** or **flex** mode.

```bash
crsctl get cluster mode status
```

Example output:

```bash
Cluster is running in "standard" mode
```

## 21. View Cluster Configuration

Retrieve detailed cluster configuration information.

```bash
crsctl get cluster configuration
```

Example output:

```bash
Name                : dbaclass-cluster
Configuration       : Cluster
Class               : Standalone Cluster
Type                : flex
```

## 22. Check Node Roles in the Cluster

View the active roles of all nodes in the cluster.

```bash
crsctl get node role status -all
```

Example output:

```bash
Node 'dbhost1' active role is 'hub'
Node 'dbhost2' active role is 'hub'
```

## 23. Manage HAS in Standalone Grid Infrastructure

These commands are useful for managing High Availability Services in a standalone Grid Infrastructure environment.

```bash
crsctl check has
crsctl config has
crsctl disable has
crsctl enable has
crsctl query has releaseversion
crsctl query has softwareversion
crsctl start has
crsctl stop has
```

---

This guide provides a comprehensive set of `CRSCTL` commands to efficiently manage your Oracle Clusterware environment. By using these commands, you can control cluster resources, check the health of various components, and manage cluster configurations seamlessly.