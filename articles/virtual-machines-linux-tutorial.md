<properties 
	pageTitle="Create a virtual machine running Linux in Azure" 
	description="Learn to create Azure virtual machine (VM) running Linux by using an image from Azure." 
	services="virtual-machines" 
	documentationCenter="" 
	authors="KBDAzure" 
	manager="timlt" 
	editor="tysonn"/>

<tags 
	ms.service="virtual-machines" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-linux" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/19/2015" 
	ms.author="kathydav"/>

#Create a Virtual Machine Running Linux 

Creating an Azure Virtual Machine (VM) that runs Linux is easy to do. This topic shows you how to use the Azure portal to create one quickly without installing anything, but you can also create VMs from any platform by using the [Azure Cross-Platform Command-Line Interface (xplat-cli)](http://www.windowsazure.com/en-us/documentation/articles/xplat-cli/), [Azure PowerShell](http://azure.microsoft.com/en-us/documentation/articles/install-configure-powershell/), or the [Server Explorer in Visual Studio](https://msdn.microsoft.com/en-us/library/azure/dn569263.aspx).

You can also create Linux VMs using [your own images as templates](../virtual-machines-linux-create-upload-vhd/). 


> [AZURE.NOTE] Even though you don't need any experience with Azure VMs to finish this tutorial, you do need an Azure account. You can create a free trial account in just a couple of minutes. For details, see [Create an Azure account](http://www.windowsazure.com/en-us/develop/php/tutorials/create-a-windows-azure-account/). 

This tutorial covers:

- [About virtual machines in Azure] []
- [How to create the virtual machine] []
- [How to log on to the virtual machine after you create it] []
- [How to attach a data disk to the new virtual machine] []

**Important**: This tutorial creates a virtual machine that is not connected to a virtual network. If you want your virtual machine to use a virtual network, you must specify the virtual network when you create the virtual machine. For more information about virtual networks, see [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).

## <a id="virtualmachine"> </a>About virtual machines in Azure ##

A virtual machine in Azure is a server in the cloud that you can control and manage. After you create a virtual machine in Azure, you can delete and re-create it whenever you need to, and you can access the virtual machine just as you do with a server in your office. Virtual hard disk (VHD) files are used to create a virtual machine. The following types of VHDs are used for a virtual machine:

- **Image** - A VHD that is used as a template to create a new virtual machine. An image is a template because it doesn't have specific settings like a running virtual machine, such as the computer name and user account settings. If you create a virtual machine using an image, an operating system disk is automatically created for the new virtual machine.
- **Disk** - A disk is a VHD that you can boot and mount as a running version of an operating system. After an image is provisioned, it becomes a disk. A disk is always created when you use an image to create a virtual machine. Any VHD that is attached to virtualized hardware and that is running as part of a service is a disk

The following options are available for using images to create a virtual machine:

- Create a virtual machine by using an image that is provided in the Image Gallery of the Azure Management Portal.
- Create and upload a .vhd file that contains an image to Azure, and then create a virtual machine using the image. For more information about creating and uploading a custom image, see [Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System](/en-us/manage/linux/common-tasks/upload-a-vhd/).

Each virtual machine resides in a cloud service, either by itself, or grouped with other virtual machines. You can place virtual machines in the same cloud service to enable the virtual machines to communicate with each other, to load-balance network traffic among virtual machines, and to maintain high availability of the machines. For more information about cloud services and virtual machines, see the "Execution Models" section in [Introducing Azure](http://go.microsoft.com/fwlink/p/?LinkId=311926).

## <a id="custommachine"> </a>How to create the virtual machine ##

This tutorial uses the **From Gallery** method to create a virtual machine because it gives you more options than the **Quick Create** method. You can choose connected resources, the DNS name, and the network connectivity if needed.

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../includes/virtual-machines-create-LinuxVM.md)]

[AZURE.INCLUDE [virtual-machines-Linux-tutorial-log-on-attach-disk](../includes/virtual-machines-Linux-tutorial-log-on-attach-disk.md)]

> [AZURE.NOTE] You can also connect to your Linux virtual machine using an SSH key for identification. For more details on how to configure and connect with an SSH key, see [How to Use SSH with Linux on Azure](./virtual-machines-linux-use-ssh-key/).

##Next Steps 

To learn more about Linux on Azure, see the following articles:

- [Introduction to Linux on Azure](http://www.windowsazure.com/en-us/documentation/articles/introduction-linux/)

- [How to use the Azure Command-Line Tools for Mac and Linux](http://www.windowsazure.com/en-us/documentation/articles/xplat-cli/)

- [About Azure VM configuration settings](http://msdn.microsoft.com/library/azure/dn763935.aspx)

- [The Docker Virtual Machine Extension for Linux on Azure](../virtual-machines-docker-vm-extension/)


[Next Steps]: #next
[About virtual machines in Azure]: #virtualmachine
[How to create the virtual machine]: #custommachine
[How to log on to the virtual machine after you create it]: #logon
[How to attach a data disk to the new virtual machine]: #attachdisk
