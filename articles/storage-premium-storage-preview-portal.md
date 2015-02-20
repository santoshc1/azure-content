<properties 
	pageTitle="Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads" 
	description="Learn Azure Premium Storage for Disks. Learn how to create a Premium Storage account." 
	services="storage" 
	documentationCenter="" 
	authors="Selcin" 
	manager="adinah" 
	editor="tysonn"/>

<tags 
	ms.service="storage" 
	ms.workload="storage" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/06/2015" 
	ms.author="selcint"/>


# Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads

## Overview

Welcome to the Public Preview of **Azure Premium Storage for Disks**!

With the introduction of new Premium Storage, Microsoft Azure now offers two types of durable storage: **Premium Storage** and **Standard Storage**. Premium Storage stores data on the latest technology Solid State Drives (SSDs) whereas Standard Storage stores data on Hard Disk Drives (HDDs). 

Premium Storage delivers high-performance, low-latency disk support for I/O intensive workloads running on Azure Virtual Machines. You can attach several Premium Storage disks to a virtual machine (VM). With Premium Storage, your applications can have up to 32 TB of storage per VM and achieve 50,000 IOPS (input/output operations per second) per VM with extremely low latencies for read operations. Premium Storage is currently available only for storing data on disks used by Azure Virtual Machines. 

To sign up for the Azure Premium Storage preview, visit the [Azure Preview](http://azure.microsoft.com/services/preview/) page.

This article provides an in-depth overview of Azure Premium Storage.

## Important Things to Know About Premium Storage

The following is a list of important things to consider before or when using Premium Storage:

- To use Premium Storage, you need to have a Premium Storage account. To learn how to create a Premium Storage account, see [Creating and using Premium Storage Account for Disks](#creating-and-using-premium-storage-account-for-disks).

- Premium Storage is currently available in the [Microsoft Azure Preview Portal](https://portal.azure.com/) and accessible via the following SDK libraries: [Storage REST API](http://msdn.microsoft.com//library/azure/dd179355.aspx) version 2014-02-14 or later; [Service Management REST API](http://msdn.microsoft.com/library/azure/ee460799.aspx) version 2014-10-01 or later; and [Azure PowerShell](http://azure.microsoft.com/documentation/articles/install-configure-powershell/) version 0.8.10 or later. 

- Premium Storage is available for limited preview in the following regions: West US, East US 2, and West Europe.

- Premium Storage supports only Azure page blobs, which are used to hold persistent disks for Azure Virtual Machines (VMs). For information on Azure page blobs, see [Understanding Block Blobs and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx). Premium Storage does not support the Azure Block Blobs, Azure Files, Azure Tables, or Azure Queues.

- A Premium Storage account is locally redundant (LRS) and keeps three copies of the data within a single region.  For considerations regarding geo replication when using Premium Storage, see the [Snapshots and Copy Blob when using Premium Storage](#snapshots-and-copy-blob-wh/ing-premium-storage) section in this article.

- If you want to use a Premium Storage account for your VM disks, you need to use the DS-series of VMs. You can use both Standard and Premium storage disks with DS-series of VMs. But you cannot use Premium Storage disks with non-DS-series of VMs. For information on available Azure VM disk types and sizes, see [Virtual Machine and Cloud Service Sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). 

- The process for setting up Premium Storage disks for a VM is similar to Standard Storage disks. You need to choose the most appropriate Premium Storage option for your Azure disks and VM. The VM size should be suitable for your workload based on the performance characteristics of the Premium offerings. For more information, see [Scalability and Performance Targets when using Premium Storage](#scalability-and-performance-targets-wh/ing-premium-storage).

- A premium storage account cannot be mapped to a custom domain name.

- Storage analytics is not currently supported for Premium Storage. To analyze the performance metrics of VMs using disks on Premium Storage accounts, use the operating system based tools, such as [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) for Windows VMs and [IOSTAT](http://linux.die.net/man/1/iostat) for Linux VMs. 

## Using Premium Storage for Disks
You can use Premium Storage for Disks in one of two ways:

- Create a new premium storage account first and then use it when creating the VM.
- Create a new DS-series VM. While creating the VM, you can select a previously created Premium Storage account, create a new one, or let the Azure Portal to create a default premium account.

Azure uses the storage account as a container for your operating system (OS) and data disks. In other words, if you create an Azure DS-series VM and select an Azure Premium Storage account, your operating system and data disks are stored in that storage account.

To leverage the benefits of Premium Storage, create a Premium Storage account using an account type of *Premium_LRS* first. To do this, you can use the [Microsoft Azure Preview Portal](https://portal.azure.com/), [Azure PowerShell](http://azure.microsoft.com/documentation/articles/install-configure-powershell/), or the [Service Management REST API](http://msdn.microsoft.com/library/azure/ee460799.aspx). For step-by-step instructions, see [Creating and using Premium Storage Account for Disks](#creating-and-using-premium-storage-account-for-disks).

<h4>Important notes:</h4>

- To learn the capacity and bandwidth preview limits for Premium Storage accounts, see the [Scalability and Performance Targets when using Premium Storage](#scalability-and-performance-targets-wh/ing-premium-storage) section. If the needs of your application exceed the scalability targets of a single storage account, build your application to use multiple storage accounts, and partition your data across those storage accounts. For example, if you want to attach 51 terabytes (TB) disks across a number of VMs, spread them across two storage accounts since 32 TB is the limit for a single Premium storage account. Make sure that a single Premium Storage account has never more than 32 TB of provisioned disks.
- By default, disk caching policy is "Read-Only" for all the Premium data disks, and "Read-Write" for the Premium operating system disk attached to the VM. This configuration setting is recommended to achieve the optimal performance for your application’s I/Os. For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.
- Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic. For example, a STANDARD_DS1 VM has 32 MB per second dedicated bandwidth available for Premium Storage disk traffic. That means, a P10 Premium Storage disk attached to this VM can only go up to 32 MB per second but not up to 100 MB per second that the P10 disk can provide. Similarly, a STANDARD_DS13 VM can go up to 256 MB per second across all disks. Currently, the largest VM on DS-series is STANDARD_DS14 and it can provide up to 512 MB per second across all disks. You might get higher throughput than this depending on the mix of read and write IOs in your disk traffic.

	Note that these limits are for disk traffic alone. There is a separate bandwidth available for VM network traffic, which is different from the dedicated bandwidth for Premium Storage disks. The following table lists the current maximum IOPS and throughput (bandwidth) values per DS-series VM across all the disks attached to the VM:

	<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
	<td><strong>VM Size</strong></td>
	<td><strong>CPU cores</strong></td>
	<td><strong>Max. IOPS</strong></td>
	<td><strong>Max. Disk Bandwidth</strong></td>
</tr>
<tr>
	<td><strong>STANDARD_DS1</strong></td>
	<td>1</td>
	<td>3,200</td>
	<td>32 MB per second</td>
</tr>
<tr>
	<td><strong>STANDARD_DS2</strong></td>
	<td>2</td>
	<td>6,400</td>
	<td>64 MB per second</td>
</tr>
<tr>
	<td><strong>STANDARD_DS3</strong></td>
	<td>4</td>
	<td>12,800</td>
	<td>128 MB per second</td>
</tr>
<tr>
	<td><strong>STANDARD_DS4</strong></td>
	<td>8</td>
	<td>25,600</td>
	<td>256 MB per second</td>
</tr>
<tr>
	<td><strong>STANDARD_DS11</strong></td>
	<td>2</td>
	<td>6,400</td>
	<td>64 MB per second</td>
</tr>
<tr>
	<td><strong>STANDARD_DS12</strong></td>
	<td>4</td>
	<td>12,800</td>
	<td>128 MB per second</td>
</tr>
<tr>
	<td><strong>STANDARD_DS13</strong></td>
	<td>8</td>
	<td>25,600</td>
	<td>256 MB per second</td>
</tr>
<tr>
	<td><strong>STANDARD_DS14</strong></td>
	<td>16</td>
	<td>50,000</td>
	<td>512 MB per second</td>
</tr>
</tbody>
</table>
	
	For the most up-to-date information, see [Virtual Machine and Cloud Service Sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). To learn about the Premium storage disks and their IOPs and throughput limits, see the table in the [Scalability and Performance Targets when using Premium Storage](#scalability-and-performance-targets-wh/ing-premium-storage) section in this article.

- You can use both Premium and Standard storage disks in the same DS-series VM.
- With Premium Storage, you can provision a DS-series VM and attach several persistent data disks to a VM. If needed, you can stripe across the disks to increase the capacity and performance of the volume. If you stripe Premium Storage data disks using [Storage Spaces](http://technet.microsoft.com/library/hh831739.aspx), you should configure it with one column for each disk that is used. Otherwise, overall performance of the striped volume may be lower than expected due to uneven distribution of traffic across the disks. By default, the Server Manager user interface (UI) allows you to setup columns up to 8 disks. But if you have more than 8 disks, you need to use PowerShell to create the volume and also specify the number of columns manually. Otherwise, the Server Manager UI continues to use 8 columns even though you have more disks. For example, if you have 32 disks in a single stripe set, you should specify 32 columns. You can use the *NumberOfColumns* parameter of the [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell cmdlet to specify the number of columns used by the virtual disk. For more information, see [Storage Spaces Overview](http://technet.microsoft.com/library/jj822938.aspx) and [Storage Spaces Frequently Asked Questions](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
- [Azure Preview Portal](https://portal.azure.com/) does not support saving and deploying custom VM Images currently. To perform these tasks, use the Azure PowerShell cmdlets, such as [Save-AzureVMImage](https://msdn.microsoft.com/library/azure/dn495108.aspx).
- Avoid adding DS-series VMs to an existing cloud service that includes non-DS-series VMs. A possible workaround is to migrate your existing VHDs to a new cloud service running only DS-series VMs.  If you want to retain the same virtual IP address (VIP) for the new cloud service that hosts your DS-series VMs, use the [Reserved IP Addresses](https://msdn.microsoft.com/library/azure/dn690120.aspx) feature.
- The DS-series of Azure virtual machines can be configured to use an operating system (OS) disk hosted either on a Standard Storage account or on a Premium Storage account. If you use the OS disk only for booting, you may consider using a Standard Storage based OS disk. This provides cost benefits and similar performance results similar to the Premium Storage after booting up. If you perform any additional tasks on the OS disk other than booting, use Premium Storage as it provides better performance results. For example, if your application reads/writes from/to the OS disk, using Premium Storage based OS disk provides better performance for your VM.
- You can use [Azure Cross-Platform Command-Line Interface (xplat-cli)](http://azure.microsoft.com/documentation/articles/xplat-cli/) with Premium Storage. To change the cache policy on one of your disks using xplat-cli, run the following command:

	`$ azure vm disk attach -h ReadOnly <VM-Name> <Disk-Name>`

	Note that the caching policy options can be ReadOnly, None, or ReadWrite. For more options, see the help by running the following command:

	`azure vm disk attach --help`


## Scalability and Performance Targets when using Premium Storage
When you provision a disk against a Premium Storage account, how much input/output operations per second (IOPS) and throughput (bandwidth) it can get depends on the size of the disk. Currently, there are three types of Premium Storage disks: P10, P20, and P30. Each one has specific limits for IOPS and throughput as specified in the following table:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
	<td><strong>Premium Storage Disk Type</strong></td>
	<td><strong>P10</strong></td>
	<td><strong>P20</strong></td>
	<td><strong>P30</strong></td>
</tr>
<tr>
	<td><strong>Disk size</strong></td>
	<td>128 GB</td>
	<td>512 GB</td>
	<td>1024 GB (1 TB)</td>
</tr>
<tr>
	<td><strong>IOPS per disk</strong></td>
	<td>500</td>
	<td>2300</td>
	<td>5000</td>
</tr>
<tr>
	<td><strong>Throughput per disk</strong></td>
	<td>100 MB per second *</td>
	<td>150 MB per second *</td>
	<td>200 MB per second *</td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] *Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic as explained in the [Using Premium Storage for Disks](#using-premium-storage-for-disks) section earlier in this article. Otherwise, your disk throughput and IOPS will be constrained to lower values based on the VM limits rather than the disk limits mentioned in the previous table.  

Azure maps the disk size (rounded up) to the nearest Premium Storage Disk option as specified in the table. For example, a disk of size 100 GB is classified as a P10 option and can perform up to 500 IO units per second, and with up to 100 MB per second throughput. Similarly, a disk of size 400 GB is classified as a P20 option, and can perform up to 2300 IO units per second and up to 150 MB per second throughput.

The input/output (I/O) unit size is 256 KB. If the data being transferred is less than 256 KB, it is considered a single I/O unit. The larger I/O sizes are counted as multiple I/Os of size 256 KB. For example, 1100 KB I/O is counted as five I/O units.

The throughput limit includes both reads and writes to the disk. For example, the sum of reads and writes should be smaller or equal to 100 MB per second for a P10 disk.  Similarly, a single P10 disk can have either 100 MB per second of reads or 100 MB per second of writes, or 60 MB per second of reads with 40 MB per second of writes as an example.
 
When you create your disks in Azure, select the most appropriate Premium Storage disk offering based on the needs of your application in terms of capacity, performance, scalability, and peak loads.

The following table describes the scalability targets for Premium storage accounts:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
	<td><strong>Total Account Capacity</strong></td>
	<td><strong>Total Bandwidth for a Locally Redundant Storage Account</strong></td>
</tr>
<tr>
	<td>
	<ul>
       <li type=round>Disk capacity: 32 TB</li>
       <li type=round>Snapshot capacity: 10 TB</li>
    </ul>
	</td>
	<td>Up to 50 gigabits per second for Inbound + Outbound</td>
</tr>
</tbody>
</table>

- Inbound refers to all data (requests) being sent to a storage account.
- Outbound refers to all data (responses) being received from a storage account.

For more information, see [Azure Storage Scalability and Performance Targets](http://msdn.microsoft.com/library/azure/dn249410.aspx).

## Throttling when using Premium Storage
You may see throttling if your application’s IOPS or throughput exceed the allocated limits for a Premium Storage disk or if your total disk traffic across all disks on the VM exceeds the disk bandwidth limit available for the VM. To avoid throttling, we recommend that you limit the number of pending I/O requests for disk based on based on the scalability and performance targets for the disk you have provisioned and based on the disk bandwidth available to the VM.  

Your application can achieve the lowest latency when it is designed to avoid throttling. On the other hand, if the number of pending I/O requests for the disk is too small, your application cannot take advantage of the maximum IOPS and throughput levels that are available to the disk.

The following examples demonstrate how to calculate the throttling levels:

<h4>Example 1:</h4>
Your application has done 495 I/Os of 16 KB disk size (, which is equal to 495 I/O Units) in one second on a P10 disk. If you try a 2 MB I/O in the same second, the total of I/O units is equal to 495 + 8. This is because 2 MB I/O results in 2048 KB / 256 KB = 8 I/O Units when the I/O unit size is 256 KB. Since the sum of 495 + 8 exceeds the 500 IOPS limit for the disk, throttling occurs. 

<h4>Note:</h4> 
All calculations are based on the I/O unit size of 256 KB.

<h4>Example 2:</h4>
Your application has done 400 I/Os of 256 KB disk size on a P10 disk. The total bandwidth consumed is (400 * 256) / 1024 = 100 MB/sec. Note that 100 MB per second is the throughput limit of a P10 disk. If your application tries to perform more I/O in that second, it gets throttled because it exceeds the allocated limit.

<h4>Example 3:</h4>
You have a DS4 VM with two P30 disks attached. Each P30 disk is capable of 200 MB per second throughput. However, a DS4 VM has a total disk bandwidth capacity of 256 MB per second. Therefore, you cannot drive the attached disks to the maximum throughput on this DS4 VM at the same time. To resolve this, you can sustain traffic of 200 MB per second on one disk and 56 MB per second on the other disk. If the sum of your disk traffic goes over 256 MB per second, the disk traffic gets throttled.

<h4>Note:</h4>
If the disk traffic mostly consists of small I/O sizes, it is highly likely that your application will hit the IOPS limit before the throughput limit. On the other hand, if the disk traffic mostly consists of large I/O sizes, it is highly likely that your application will hit the throughput limit instead of the IOPS limit. You can maximize your application’s IOPS and throughput capacity by using optimal I/O sizes and also by limiting the number of pending I/O requests for disk.

## Snapshots and Copy Blob when using Premium Storage
You can create a snapshot for Premium Storage in the same way as you create a snapshot when using Standard Storage. Since Premium Storage is locally redundant, we recommend that you create snapshots and then copy those snapshots to a geo-redundant standard storage account. For more information, see [Azure Storage Redundancy Options](http://msdn.microsoft.com/library/azure/dn727290.aspx).

If a disk is attached to a VM, certain API operations are not permitted on the page blob backing the disk. For example, you cannot perform a [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) operation on that blob as long as the disk is attached to a VM. Instead, first create a snapshot of that blob by using the [Snapshot Blob](http://msdn.microsoft.com/library/azure/ee691971.aspx) REST API method, and then perform the [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) of the snapshot to copy the attached disk. Alternatively, you can detach the disk and then perform any necessary operations on the underlying blob.

<h4>Important Notes:</h4>

- If the copy blob operation on Premium Storage overwrites an existing blob at the destination, the blob being overwritten must not have any snapshots.  A copy within or between premium storage accounts require that the destination blob does not have snapshots when initiating the copy.
- The number of snapshots for a single blob is limited to 100. A snapshot can be taken every 10 minutes at most.
- 10 TB is the maximum capacity for snapshots per Premium Storage account. Note that the snapshot capacity is the unique data that exists in the snapshots. In other words, the snapshot capacity does not include the base blob size.
- You can copy snapshots from a premium storage account to a geo-redundant standard storage account by using AzCopy or Copy Blob. For more information, see [How to use AzCopy with Microsoft Azure Storage](http://azure.microsoft.com/documentation/articles/storage-use-azcopy/) and [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx).
- For detailed information on performing REST operations against page blobs in Premium Storage accounts, see [Using Blob Service Operations with Azure Premium Storage](http://go.microsoft.com/fwlink/?LinkId=521969) in the MSDN library.

## Pricing and Billing when using Premium Storage
When using Premium Storage, the following billing considerations apply:

- Billing for a Premium Storage disk depends on the provisioned size of the disk. Azure maps the disk size (rounded up) to the nearest Premium Storage Disk option as specified in the table given in the [Scalability and Performance Targets when using Premium Storage](#scalability-and-performance-targets-wh/ing-premium-storage) section. Billing for any provisioned disk is prorated hourly using the monthly price for the Premium Storage offer. For example, if you provisioned a P10 disk and deleted it after 20 hours, you are billed for the P10 offering prorated to 20 hours. This is regardless of the amount of actual data written to the disk or the IOPS/throughput used.
- Snapshots on Premium Storage are billed for the additional capacity used by the snapshots. For information on snapshots, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).
- [Outbound data transfers](http://azure.microsoft.com/pricing/details/data-transfers/) (data going out of Azure data centers) incur billing for bandwidth usage. 

For detailed information on pricing for Premium Storage and DS-series VMs, see:

- [Azure Storage Pricing](http://azure.microsoft.com/pricing/details/storage/)
- [Virtual Machines Pricing](http://azure.microsoft.com/pricing/details/virtual-machines/)

## Creating and using Premium Storage Account for Disks
This section demonstrates how to create a Premium Storage account using Azure Preview Portal and Azure PowerShell. In addition, it demonstrates a sample use case for premium storage accounts: creating a virtual machine and attaching a data disk to a virtual machine when using Premium Storage.

In this section:

* [Create a Premium Storage account in the Azure Preview Portal](#create-a-premium-storage-account-in-the-azure-preview-portal)	

* [Create a Premium Storage account with Azure PowerShell](#create-a-premium-storage-account-with-azure-powershell)	
 
### Create a Premium Storage account in the Azure Preview Portal
This section shows how to create a Premium Storage account using Azure Preview Portal.

1.	Sign in to the [Azure Preview Portal](https://portal.azure.com/). Check out the [Free Trial](http://azure.microsoft.com/pricing/free-trial/) offer if you do not have a subscription yet. 


    > [AZURE.NOTE] If you log in to the Azure Management Portal, click your user account name at the top right corner of the portal. Then, click **Switch to new portal**.
        

2.	On the Hub menu, click **New**.

3.	Under **New**, click **Everything**. Select **Storage, cache, +backup**. From there, click **Storage** and then click **Create**.

4.	On the Storage Account blade, type a name for your storage account. Click **Pricing Tier**. On the **Recommended pricing tiers** blade, click **Browse All Pricing Tiers**. On the **Choose your pricing tier** blade, choose **Premium Locally Redundant**. Click **Select**. Note that the **Storage account** blade shows **Standard-GRS** as the **Pricing Tier** by default. After you click **Select**, the **Pricing Tier** is shown as **Premium-LRS**.
	
	![Pricing Tier][Image1]

	
5.	On the **Storage Account** blade, keep the default values for **Resource Group**, **Subscription**, **Location**, and **Diagnostics**. Click **Create**.

For a complete walkthrough inside an Azure environment, see [Create a Virtual Machine Running Windows in the Azure Preview Portal](http://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial-azure-preview/).

### Create a Premium Storage account with Azure PowerShell
This section shows how to create a Premium Storage account and how to use it while creating a virtual machine and attaching a data disk to a VM using Azure PowerShell.

1. Setup your PowerShell environment by following the steps given at [How to install and configure Azure PowerShell](http://azure.microsoft.com/documentation/articles/install-configure-powershell/).
2. Start the PowerShell console, connect to your subscription, and run the following PowerShell cmdlet in the console window. As seen in this PowerShell statement, you need to specify the **Type** parameter as **Premium_LRS** when you create a premium storage account.

		New-AzureStorageAccount -StorageAccountName "yourpremiumaccount" -Location "West US" -Type "Premium_LRS"

3. Next, create a new DS-Series VM and specify that you want Premium Storage by running the following PowerShell cmdlets in the console window:

    	$storageAccount = "yourpremiumaccount"
    	$adminName = "youradmin"
    	$adminPassword = "yourpassword"
    	$vmName ="yourVM"
    	$location = "West US"
    	$imageName = "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201409.01-en.us-127GB.vhd"
    	$vmSize ="Standard_DS2"
    	$OSDiskPath = "https://" + $storageAccount + ".blob.core.windows.net/vhds/" + $vmName + "_OS_PIO.vhd"
    	$vm = New-AzureVMConfig -Name $vmName -ImageName $imageName -InstanceSize $vmSize -MediaLocation $OSDiskPath
    	Add-AzureProvisioningConfig -Windows -VM $vm -AdminUsername $adminName -Password $adminPassword
    	New-AzureVM -ServiceName $vmName -VMs $VM -Location $location

4. If you want more disk space for your VM, attach a new data disk to an existing DS-series VM after it is created by running the following PowerShell cmdlets in the console window:

    	$storageAccount = "yourpremiumaccount"
    	$vmName ="yourVM"
    	$vm = Get-AzureVM -ServiceName $vmName -Name $vmName
    	$LunNo = 1
    	$path = "http://" + $storageAccount + ".blob.core.windows.net/vhds/" + "myDataDisk_" + $LunNo + "_PIO.vhd"
    	$label = "Disk " + $LunNo
    	Add-AzureDataDisk -CreateNew -MediaLocation $path -DiskSizeInGB 128 -DiskLabel $label -LUN $LunNo -HostCaching ReadOnly -VM $vm | Update-AzureVm

## Additional Resources

[Using Blob Service Operations with Azure Premium Storage](http://go.microsoft.com/fwlink/?LinkId=521969)

[Create a Virtual Machine Running Windows](http://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial-azure-preview/)

[Virtual Machine and Cloud Service Sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx)

[Storage Documentation](http://azure.microsoft.com/documentation/services/storage/)

[MSDN Reference](http://msdn.microsoft.com/library/azure/gg433040.aspx)

[Image1]: ./media/storage-premium-storage-preview-portal/Azure_pricing_tier.png



