---
title: "Step-by-Step Oracle 19c RAC Installation on RHEL 7.9"
date: 2024-01-16
draft: false
tags: ["new", "docs", "vmware"]
showRecent : true
showHero: false
showAuthor: true
showDate: true
showSummary: true
---

Here's a detailed and unique step-by-step guide for **Oracle 19c RAC Installation on Red Hat Enterprise Linux 7.9**. This refined guide breaks down the process in depth to give you a solid understanding of each phase of the installation.

---

# Step-by-Step Oracle 19c RAC Installation on Oracle Linux 7.9

**Oracle Real Application Clusters (RAC)** provides database services with high availability, scalability, and flexibility by allowing multiple servers to run Oracle software and work together as a single system. This guide will take you through the step-by-step process of installing Oracle RAC 19c on Red Hat Enterprise Linux 7.9, from pre-requisites to the final setup.

---

## Prerequisites

Before beginning, ensure the following prerequisites are met:

1. **Two or More Linux Nodes:**
   - The setup assumes at least two nodes (`node1`, `node2`).
   - Ensure the servers have access to shared storage (either through iSCSI or NFS).

2. **Operating System:**
   - Red Hat Enterprise Linux 7.9 should be installed on both nodes.
   - Disable the firewall and SELinux as it may interfere with the RAC setup:
     ```bash
	systemctl stop firewalld.service
	systemctl disable firewalld.service
     	setenforce 0
     ```

3. **Hosts File Configuration:**
   Add public and private IPs for each node to `/etc/hosts` on both servers. Example:
   ```bash
	# Public
	192.168.1.201 DCNODE1.ORACLE.COM DCNODE1
	192.168.1.202 DCNODE2.ORACLE.COM DCNODE2

	# Private
	192.168.10.201 DCNODE1-PRIV.ORACLE.COM DCNODE1-PRIV
	192.168.10.202 DCNODE2-PRIV.ORACLE.COM DCNODE2-PRIV

	# Virtual
	192.168.1.70 DCNODE1-VIP.ORACLE.COM DCNODE1-VIP
	192.168.1.80 DCNODE2-VIP.ORACLE.COM DCNODE2-VIP

	# SCAN
	192.168.1.41 RAC-SCAN.ORACLE.COM RAC-SCAN
	192.168.1.42 RAC-SCAN.ORACLE.COM RAC-SCAN
	192.168.1.43 RAC-SCAN.ORACLE.COM RAC-SCAN
   ```

4. **Shared Storage:**
   Provision and configure shared disks for the Oracle RAC setup. Ensure that both nodes can access the shared storage without any issues.

5. **Oracle Prerequisite Packages:**
   	```bash
	wget "https://yum.oracle.com/repo/OracleLinux/OL7/latest/x86_64/getPackage/oracle-database-preinstall-19c-1.0-3.el7.x86_64.rpm"
	chmod 775 oracle-database-preinstall-19c-1.0-3.el7.x86_64.rpm
	yum install oracle-database-preinstall-19c-1.0-3.el7.x86_64.rpm
	```
   	```bash
	wget "https://yum.oracle.com/repo/OracleLinux/OL7/latest/x86_64/getPackage/oracleasm-support-2.1.11-2.el7.x86_64.rpm"
	chmod 775 oracleasm-support-2.1.11-2.el7.x86_64.rpm
	yum install oracleasm-support-2.1.11-2.el7.x86_64.rpm -y
	yum install oracleasm -y
	```
---

## Part 1: Grid Infrastructure Installation

### Step 1: User and Group Creation.

Group Creation: -
```bash
groupadd -g 5001 oinstall
groupadd -g 5002 dba
groupadd -g 5003 oper
groupadd -g 5004 asmadmin 
groupadd -g 5005 asmdba 
groupadd -g 5006 asmoper
```
User Creation: -
```bash
useradd -u 5007 -g oinstall -G dba,oper,asmdba oracle
useradd -u 5008 -g oinstall -G asmadmin,asmdba,asmoper,dba grid
usermod -g oinstall -G dba,oper,asmdba oracle
usermod -g oinstall -G asmadmin,asmdba,asmoper,dba grid
passwd oracle
passwd grid
```

### Step 2: Preparing ASM Disks

For Oracle RAC, shared disks will be managed by Oracle ASM (Automatic Storage Management). First, create and label ASM disks on both nodes.

Install the system Linux prerequisites

First check internet is working fine or not
ping google.com

The package oracle-database-preinstall-19c contains all the prerequisites on Oracle Linux using the Oracle Unbreakable Enterprise Kernel (UEK).

```bash
yum update -y
yum install -y oracle-database-preinstall-19c.x86_64
yum install oracleasm-support -y
yum install oracleasm
```

```bash
oracleasm configure -i
oracleasm init
oracleasm createdisk DATA1 /dev/sdb
oracleasm createdisk FRA1 /dev/sdc
```

Ensure both nodes recognize the ASM disks by running:
```bash
oracleasm scandisks
oracleasm listdisks
```

```bash
/usr/sbin/oracleasm configure -i
```

Default user to own the driver interface []: `grid`
Default group to own the driver interface []: `asmadmin`
Start Oracle ASM library driver on boot (y/n) [n]: `y`
Scan for Oracle ASM disks on boot (y/n) [y]: `y`


### Step 3: Installing Oracle Grid Infrastructure

#### Download and Install Grid Software

1. Download the Oracle Grid Infrastructure software from Oracle’s website and extract the files.
2. Run the installer on **Node1**:
   ```bash
   ./runInstaller
   ```

During the installation:
- Select **Configure Oracle Grid Infrastructure for a New Cluster**.
- Enter the cluster name and SCAN name (e.g., `myrac-cluster` and `myrac-scan`).
- Configure the ASM disk groups (`DATA` for database files, `FRA` for recovery files).
- Configure redundancy as per your requirement (normal, high, etc.).

After the installation, synchronize ASM on Node2:
```bash
oracleasm scandisks
```

#### Step 4: Verify Installation
Check that the Clusterware and ASM are running:
```bash
crsctl check crs
crsctl check cssd
```

---

## Part 2: Installing Oracle Database Software

### Step 1: Pre-Installation Checks

Before installing the database software, run the Oracle preinstallation package to ensure your system meets all the Oracle prerequisites:
```bash
yum install -y oracle-database-preinstall-19c
```

This will install necessary packages and adjust kernel parameters.

### Step 2: Installing Oracle Database Software

1. Download Oracle 19c database software and extract it on both nodes.
2. Run the Oracle Universal Installer (OUI) on **Node1**:
   ```bash
   ./runInstaller
   ```
   - Select **Oracle RAC Database** as the installation type.
   - Include both nodes (Node1 and Node2) in the cluster configuration.
   - Choose **Oracle ASM** for storage, and select the `DATA` and `FRA` disk groups created during the Grid Infrastructure setup.
   
3. The installer will guide you through database-specific settings, including SID, character set, memory allocations, and more.

### Step 3: Post-Installation Configuration

Once the installation completes, perform the following steps to finalize the database setup:

#### Modify SQLNET.ORA (if needed)
Modify the `sqlnet.ora` file on both nodes to avoid any authentication errors:
```bash
SQLNET.ALLOWED_LOGON_VERSION_SERVER=8
SQLNET.ALLOWED_LOGON_VERSION_CLIENT=8
```

---

## Part 3: Configuring and Starting the RAC Database

### Step 1: Creating the RAC Database Using DBCA

Run **Database Configuration Assistant (DBCA)** on Node1 to create the RAC database.

1. Launch the DBCA utility:
   ```bash
   dbca
   ```
2. Select **Create Database**, then choose **Oracle RAC Database**.
3. Follow the prompts to configure the RAC database, choosing the `DATA` and `FRA` ASM disk groups for storage.
4. Set up listener configurations, instance settings, and configure the database initialization parameters.

### Step 2: Verifying RAC Setup

After creating the database, verify that the RAC instances are running on both nodes:
```bash
srvctl status database -d orcl
```
This will show the status of all RAC instances.

### Step 3: Manage RAC Services

You can use **SRVCTL** to manage the RAC services:
- Start the database:
  ```bash
  srvctl start database -d orcl
  ```
- Stop the database:
  ```bash
  srvctl stop database -d orcl
  ```

### Step 4: Testing the RAC Database

Once everything is up and running, test the RAC database using SQL*Plus or any Oracle database management tool (like SQL Developer).

Example:
```bash
sqlplus / as sysdba
SQL> select instance_name from v$instance;
```

You should see both RAC instances listed, confirming that the RAC setup is complete.

---

## Conclusion

With the completion of this step-by-step guide, your **Oracle 19c RAC** on **Oracle Linux 7.9** is successfully installed and running. This setup provides high availability, load balancing, and scalability for your Oracle databases. Regularly monitor and manage the database and clusterware to ensure smooth operations.

For further tuning and advanced configurations, you can explore Oracle's documentation and online resources.