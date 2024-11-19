---
title: "Step-by-Step Guide to Creating a Windows Server 2025 Virtual Machine"
date: 2024-10-19
draft: false
description: "ASM Disk for Oracle RAC on Windows Server 2025"
tags: ["new", "docs", "patching", "19c", "ASM", "oracle"]
showRecent : true
showHero: false
showDate: true
showSummary: true
---

# Create a Windows Server 2025 Virtual Machine from the Following Guide

{{< article link="/windows/02-windows_srv_2025_vm_guide/">}}
---

---

## Step 1: Add an Additional Hard Disk for Oracle ASM Disks.
1. Return to the main VMware Workstation Pro window.
2. Locate your new VM from the **Library** and select it. **(Windows Server 2022)**
3. Right Click and Select **Settings**.
![Step 01](/windows/02-windows_srv_2025_vm_guide/screenshots/99-Add_extra_drive.png)
4. In the **Virual Machine Settings** Window, Click **Add**.
![Step 02](/windows/02-windows_srv_2025_vm_guide/screenshots/16_Create_a_VM.png)
5. In the **Add Hardware Wizard** Window, select **Hard Disk**. Click **Next**, select **NVMe** and Click **Next**
![Step 02](/windows/02-windows_srv_2025_vm_guide/screenshots/17_Create_a_VM.png)
![Step 02](/windows/02-windows_srv_2025_vm_guide/screenshots/18_Create_a_VM.png)
6. In the **Select a Disk** window, choose **Create a new virtual disk**. Click **Next**.
![Step 02](/windows/02-windows_srv_2025_vm_guide/screenshots/19_Create_a_VM.png)
7. In the **Specify Disk Capacity** window, choose a disk size appropriate for your VM. **60 GB** is recommended.
![Step 02](/windows/02-windows_srv_2025_vm_guide/screenshots/20_Create_a_VM.png)
Select **Store virtual disk as a single file** for simplicity (you may also choose **Split virtual disk into multiple files** if needed).
8. In the **Specify Disk File** window, choose a disk file location of your choice.
![Step 02](/windows/02-windows_srv_2025_vm_guide/screenshots/21_Create_a_VM.png)
9. Click **Next**.


## Step 2: Power On the Virtual Machine

1. Return to the main VMware Workstation Pro window.
2. Locate your new VM from the **Library** and select it.
3. Click **Power on this virtual machine**.


