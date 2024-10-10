---
title: "Create a VM in VMware for RHEL 7.9 🖥️ "
date: 2024-01-16
draft: false
description: "Create a virtual machine in vmware for RHEL 7.9"
tags: ["new", "docs", "vmware"]
series: ["Oracle 19c Installation and Single Instance Database Creation"]
series_order: 1
showRecent : true
showHero: false
showAuthor: false
showDate: true
showSummary: true
---

{{< lead >}}
#### In this rich guide we will walk you through a step-by-step process to installing Red Hat Enterprise Linux (RHEL) 7.9 on VMware Workstation Pro.
{{< /lead >}}

## Prerequisites:
### VMware Workstation
 - You can download VMware Workstation from the here: 

### Red Hat 7.9 ISO
 - You can download the ISO from Red Hat Offical Website here.

### Host System Requirements
 - Minimum 8 GB of RAM.
 - A Dual Core Processor.
 - 128 GB+ of Hard Drive Storage

# Step 1
## Create a Virtual Machine

1. Open VMware Workstation and Click on **File** and select **New Virtual Machine**, Select **Custom (advanced)** option and click **Next**.
![Step 01](screenshots/01.png)

2. Keep the Default Hardware Compatibility **Workstation 16.x** and click **Next**
![Step 02](screenshots/02.png)

3. Select **I will install the operating system later.** as we will be mounting the ISO seperately once the Virtual Machine has been created.
![Step 03](screenshots/03.png)

4. Select **Linux** as the Guest Operating System and Version **Red Hat Enterprise Linux 7 64-bit**.
![Step 04](screenshots/04.png)

5. Give your Virtual Machine a Name and a Location.
![Step 05](screenshots/05.png)


6. Select the Number of Available Cores per processor depending on your Host systems availability. In this case we have selected **02 Cores**.
![Step 06](screenshots/06.png)

7. Specify the Amount of RAM as per your requirement. In this Case we had 16 GB of Avilable Memory on host, Hence we have selected **8 GB**.
![Step 07](screenshots/07.png)


8. Select **Use bridged networking** as we will be using the external LAN for our Networking.
![Step 08](screenshots/08.png)


9. Keep the default selection **LSI Logic**.
![Step 09](screenshots/09.png)


10. Keep the default selection for virtual disk type **SCSI**.
![Step 10](screenshots/010.png)


11. Selelct **Create a new virtal disk**.
![Step 11](screenshots/011.png)

12. Allocate a disk size of **64 GB** and select **Store virtual disk as a single file** and complete the wizard.
![Step 12](screenshots/012.png)

13. Once the Virtual machine has been created, **Edit the virtual machine settings**
![Step 13](screenshots/edit-vm.png)


14. Under the **Hardware** tab, select the Virtual Disk Drive and locate the RHEL ISO for the Installation.
![Step 14](screenshots/iso.png)

15. Under the **Options** tab, select the **Advance** and under **Firware Type** select **UEFI** and click ok.
![Step 14](screenshots/015.png)

16. Now **Power on the Virtual Machine**


# Step 2
## Install RHEL 7.9

1. Select **English** as the Language and click on **Continue**.
![RHEL Setup Steps 01](screenshots/S01.png)

2. Select **Software Selection** and under *Base Environment* select **Minimal Install**.
![RHEL Setup Steps 02](screenshots/S02.png)

3. Select **Installation Destinaion** and under *System* select **I will configure the partitioning**, Click **Done**. Select **Standard partitioning** and *create the partitions* as follows:
{{< lead >}}
| System      | Size                      | 
| :---        | :----                     |
| /boot/efi   | 512 MiB                   |
| /swap       | 8192 MiB (8GB)            |
| /           | Remaning space available  |
{{< /lead >}}
Click **Done** and **Accept Changes**.
![RHEL Setup Steps 03](screenshots/S03.png)

4. Select **KDUMP**, uncheck the **enable KDUMP** option and click **Done**. 
![RHEL Setup Steps 04](screenshots/S04.png)

5. Select **Security Policy**, uncheck the **Apply security policy** option and click **Done**. 
![RHEL Setup Steps 05](screenshots/S05.png)

6. Select **Network & Hostname**. Add a Hostname and Click Apply.
Click on **Configure**, select the **IPv4 Settings**, Change the *Method* to **Manual** and assign a Static IP address which is available in your LAN Subnet.
Add DNS **8.8.8.8,1.1.1.1** in the DNS Servers Field.
![RHEL Setup Steps 06](screenshots/S06.png)

7. Click **Begin Installation**.
![RHEL Setup Steps 07](screenshots/S07.png)

8. Set a *Root Password* and *Create a User* .
![RHEL Setup Steps 08](screenshots/S08.png)
9. Once the Installation has been Completed, Click **Reebot**.
![RHEL Setup Steps 09](screenshots/S09.png)