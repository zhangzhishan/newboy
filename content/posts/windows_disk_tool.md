---
title: Widnows 硬盘配置查询工具/命令[7/15]
tag: performance
date: 2020-06-24 18:44:39
---


## fsutil fsinfo
该命令主要是可以列出当前电脑上所包含的各个盘的信息, 类型, 以及一些文件系统相关的信息, 对于Windows主要是 NTFS的信息.

### Syntax

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <volumepath>
fsutil fsinfo [ntfsinfo] <rootpath>
fsutil fsinfo [statistics] <volumepath>
fsutil fsinfo [volumeinfo] <rootpath>
```

### Parameters

| Parameter | Description |
| --------- |------------ |
| drives | Lists all drives in the computer. |
| drivetype | Queries a drive and lists its type, for example CD-ROM drive. |
| ntfsinfo | Lists NTFS specific volume information for the specified volume, such as the number of sectors, total clusters, free clusters, and the start and end of the MFT Zone. |
| sectorinfo | Lists information about the hardware's sector size and alignment. |
| statistics | Lists file system statistics for the specified volume, such as metadata, log file, and MFT reads and writes. |
| volumeinfo | Lists information for the specified volume, such as the file system, and whether the volume supports case-sensitive file names, unicode in file names, disk quotas, or is a DirectAccess (DAX) volume. |
| `<volumepath>:` | Specifies the drive letter (followed by a colon). |
| `<rootpath>:` | Specifies the drive letter (followed by a colon) of the root drive. |

### Examples

To list all of the drives in the computer, type:

```
fsutil fsinfo drives
```

Output similar to the following displays:

```
Drives: A:\ C:\ D:\ E:\
```

To query the drive type of drive C, type:

```
fsutil fsinfo drivetype c:

C:\>fsutil fsinfo drivetype D:
D: - Fixed Drive

C:\>fsutil fsinfo drivetype Z:
Z: - Remote/Network Drive
```

Possible results of the query include:

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

To query the volume information for volume E, type:

```
fsinfo volumeinfo e:\
```

Output similar to the following displays:

```
Volume Name : Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
Supports Named Streams
Is DAX Volume
```

To query drive F for NTFS-specific volume information, type:

```
fsutil fsinfo ntfsinfo f:
```

Output similar to the following displays:

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors : 0x00000000010ea04f
Total Clusters : 0x000000000021d409
Mft Zone End : 0x0000000000004700
```

To query the file system's underlying hardware for sector information, type:

```
fsinfo sectorinfo d:
```

Output similar to the following displays:

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector : 4096
PhysicalBytesPerSectorForAtomicity : 4096
Trim Not Supported
DAX capable
```

To query the file system statistics for drive E, type:

```
fsinfo statistics e:
```

Output similar to the following displays:

```
File System Type : NTFS
Version : 1
UserFileReads : 75021
UserFileReadBytes : 1305244512
LogFileWriteBytes : 180936704
```



## Get-PhysicalDisk

The `Get-PhysicalDisk` cmdlet gets a list of all PhysicalDisk objects visible across any available Storage Management Providers, or optionally a filtered list of disks.

Gets a list of all PhysicalDisk objects visible across any available Storage Management Providers, or optionally a filtered list.

### SYNTAX

#### ByUniqueId (Default)
```
Get-PhysicalDisk [-UniqueId <String>] [-Usage <PhysicalDiskUsage>] [-Description <String>]
 [-Manufacturer <String>] [-Model <String>] [-CanPool <Boolean>] [-HealthStatus <PhysicalDiskHealthStatus>]
 [-CimSession <CimSession>] [<CommonParameters>]
```

#### ByObjectId
```
Get-PhysicalDisk [-ObjectId <String>] [-Usage <PhysicalDiskUsage>] [-Description <String>]
 [-Manufacturer <String>] [-Model <String>] [-CanPool <Boolean>] [-HealthStatus <PhysicalDiskHealthStatus>]
 [-CimSession <CimSession>] [<CommonParameters>]
```

#### ByName
```
Get-PhysicalDisk [[-FriendlyName] <String>] [[-SerialNumber] <String>] [-Usage <PhysicalDiskUsage>]
 [-Description <String>] [-Manufacturer <String>] [-Model <String>] [-CanPool <Boolean>]
 [-HealthStatus <PhysicalDiskHealthStatus>] [-CimSession <CimSession>] [<CommonParameters>]
```

#### ByInputObject
```
Get-PhysicalDisk -InputObject <CimInstance> [-CimSession <CimSession>] [<CommonParameters>]
```

#### ByStorageSubsystem
```
Get-PhysicalDisk -StorageSubsystem <CimInstance> [-Usage <PhysicalDiskUsage>] [-Description <String>]
 [-Manufacturer <String>] [-Model <String>] [-CanPool <Boolean>] [-HealthStatus <PhysicalDiskHealthStatus>]
 [-CimSession <CimSession>] [<CommonParameters>]
```

#### ByStorageEnclosure
```
Get-PhysicalDisk -StorageEnclosure <CimInstance> [-Usage <PhysicalDiskUsage>] [-Description <String>]
 [-Manufacturer <String>] [-Model <String>] [-CanPool <Boolean>] [-HealthStatus <PhysicalDiskHealthStatus>]
 [-CimSession <CimSession>] [<CommonParameters>]
```

#### ByStorageNode
```
Get-PhysicalDisk -StorageNode <CimInstance> [-PhysicallyConnected] [-Usage <PhysicalDiskUsage>]
 [-Description <String>] [-Manufacturer <String>] [-Model <String>] [-CanPool <Boolean>]
 [-HealthStatus <PhysicalDiskHealthStatus>] [-CimSession <CimSession>] [<CommonParameters>]
```

#### ByStoragePool
```
Get-PhysicalDisk -StoragePool <CimInstance> [-Usage <PhysicalDiskUsage>] [-Description <String>]
 [-Manufacturer <String>] [-Model <String>] [-CanPool <Boolean>] [-HealthStatus <PhysicalDiskHealthStatus>]
 [-CimSession <CimSession>] [<CommonParameters>]
```

#### ByVirtualDisk
```
Get-PhysicalDisk -VirtualDisk <CimInstance> [-VirtualRangeMin <UInt64>] [-VirtualRangeMax <UInt64>]
 [-HasAllocations <Boolean>] [-SelectedForUse <Boolean>] [-NoRedundancy] [-Usage <PhysicalDiskUsage>]
 [-Description <String>] [-Manufacturer <String>] [-Model <String>] [-CanPool <Boolean>]
 [-HealthStatus <PhysicalDiskHealthStatus>] [-CimSession <CimSession>] [<CommonParameters>]
```

### EXAMPLES

#### Example 1: Getting all physical disks
```
PS C:\> Get-PhysicalDisk
FriendlyName        CanPool            OperationalStatus   HealthStatus        Usage                              Size 
------------        --------            -----------------   ------------        -----                              ---- 
PhysicalDisk4       False               OK                  Healthy             Data Store                        25 GB
```

This example returns an array of all PhysicalDisk objects present in the computer. A storage management provider is required to manage physical disks.

#### Example 2: Getting all physical disks eligible for adding to a storage pool
```
PS C:\>Get-PhysicalDisk -CanPool $True
```

This example returns an array of PhysicalDisk objects that are available for adding to a storage pool (they are in a primordial pool).

### PARAMETERS

#### -CanPool
Gets physical disks that are available for use in a storage pool.

```yaml
Type: Boolean
Parameter Sets: ByUniqueId, ByObjectId, ByName, ByStorageSubsystem, ByStorageEnclosure, ByStorageNode, ByStoragePool, ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -CimSession
Runs the cmdlet in a remote session or on a remote computer.
Enter a computer name or a session object, such as the output of a [New-CimSession](https://docs.microsoft.com/powershell/module/cimcmdlets/new-cimsession) or [Get-CimSession](https://go.microsoft.com/fwlink/p/?LinkId=227966) cmdlet.
The default is the current session on the local computer.

```yaml
Type: CimSession
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -Description
Gets the physical disks that contain the specified description.
Enter a description or use wildcard characters to enter a description pattern.

```yaml
Type: String
Parameter Sets: ByUniqueId, ByObjectId, ByName, ByStorageSubsystem, ByStorageEnclosure, ByStorageNode, ByStoragePool, ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -FriendlyName
Gets the physical disk with the specified friendly name.
Enter a friendly name or use wildcard characters to enter a name pattern.

```yaml
Type: String
Parameter Sets: ByName
Aliases: 

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -HasAllocations
Indicates whether the cmdlet gets a list of physical disks that host the extents of the virtual disk that you specify by using the *VirtualDisk* parameter.

```yaml
Type: Boolean
Parameter Sets: ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -HealthStatus
Specifies the **health status** of physical disks.
The acceptable values for this parameter are:

- Healthy 
- Unhealthy 
- Unknown 
- Warning

```yaml
Type: PhysicalDiskHealthStatus
Parameter Sets: ByUniqueId, ByObjectId, ByName, ByStorageSubsystem, ByStorageEnclosure, ByStorageNode, ByStoragePool, ByVirtualDisk
Aliases: 
Accepted values: Healthy, Warning, Unhealthy, Unknown

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -InputObject
Specifies the input object that is used in a pipeline command.

```yaml
Type: CimInstance
Parameter Sets: ByInputObject
Aliases: 

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -Manufacturer
Gets the physical disks with the specified manufacturer.
Enter a manufacturer string or use wildcard characters to enter a pattern.

```yaml
Type: String
Parameter Sets: ByUniqueId, ByObjectId, ByName, ByStorageSubsystem, ByStorageEnclosure, ByStorageNode, ByStoragePool, ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -Model
Gets the physical disks of the specified model.
Enter a model string or use wildcard characters to enter a pattern.

```yaml
Type: String
Parameter Sets: ByUniqueId, ByObjectId, ByName, ByStorageSubsystem, ByStorageEnclosure, ByStorageNode, ByStoragePool, ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -NoRedundancy
Indicates that this cmdlet gets physical disks that contain the last remaining copy of the data of a virtual disk.

```yaml
Type: SwitchParameter
Parameter Sets: ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -ObjectId
Specifies the ID of the physical disk to get.

```yaml
Type: String
Parameter Sets: ByObjectId
Aliases: PhysicalDiskObjectId

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -PhysicallyConnected
Indicates that this cmdlet gets physical disks that are physically connected to the specified storage node.

```yaml
Type: SwitchParameter
Parameter Sets: ByStorageNode
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -SelectedForUse
Indicates whether the cmdlet gets a list of physical disks to host the extents that belong to the virtual disk specified by the *VirtualDisk* parameter.
Specify the physical disks to host the extents of a virtual disk by using the *PhysicalDisksToUse* parameter of the **New-VirtualDisk** cmdlet.

```yaml
Type: Boolean
Parameter Sets: ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -SerialNumber
Specifies the serial number of the physical disk to get.

```yaml
Type: String
Parameter Sets: ByName
Aliases: 

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -StorageEnclosure
Specifies a storage enclosure associated with the physical disk that this cmdlet gets.
To obtain a **StorageEnclosure** object, use the **Get-StorageEnclosure** cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByStorageEnclosure
Aliases: 

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -StorageNode
Specifies a storage node as a **CimInstance** object.
The cmdlet gets the physical disk connected to the node that you specify.
To obtain a storage node object, use the **Get-StorageNode** cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByStorageNode
Aliases: 

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -StoragePool
Accepts a StoragePool object as input and gets the physical disks that belong to the pool.
The Storage Pool CIM object is exposed by the **Get-StoragePool** cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByStoragePool
Aliases: 

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -StorageSubsystem
Specifies a storage subsystem.
This cmdlet gets physical disks attached to the storage subsystem that you specify.
To obtain a **StorageSubsystem** object, use the **Get-StorageSubSystem** cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByStorageSubsystem
Aliases: 

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -UniqueId
Gets only the physical disks with the specified IDs.
Type one or more IDs (separated by commas), or use wildcard characters to enter a pattern.

```yaml
Type: String
Parameter Sets: ByUniqueId
Aliases: Id

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -Usage
Specifies an allocation method or usage.
This cmdlet gets the physical disks that have the specified allocation method.
The acceptable values for this parameter are:

- AutoSelect 
- HotSpare 
- Journal 
- ManualSelect 
- Retired 
- Unknown

```yaml
Type: PhysicalDiskUsage
Parameter Sets: ByUniqueId, ByObjectId, ByName, ByStorageSubsystem, ByStorageEnclosure, ByStorageNode, ByStoragePool, ByVirtualDisk
Aliases: 
Accepted values: Unknown, AutoSelect, ManualSelect, HotSpare, Retired, Journal

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -VirtualDisk
Accepts a VirtualDisk object as input and gets the physical disks used by the virtual disk.
The VirtualDisk object is exposed by the **Get-VirtualDisk** cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByVirtualDisk
Aliases: 

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -VirtualRangeMax
This parameter is reserved for future use.

```yaml
Type: UInt64
Parameter Sets: ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -VirtualRangeMin
This parameter is reserved for future use.

```yaml
Type: UInt64
Parameter Sets: ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```


## Get-Disk
The **Get-Disk** cmdlet gets one or more Disk objects visible to the operating system, or optionally a filtered list.

Gets one or more disks visible to the operating system.

> [!NOTE]
> This cmdlet returns physical disk objects like basic disks and partitioned drive partitions.  Dynamic disks can span multiple pieces of physical media, so they will not be returned by Get-Disk. For more information, see [Basic and Dynamic Disks](https://docs.microsoft.com/windows/desktop/FileIO/basic-and-dynamic-disks).

### SYNTAX

#### ByNumber (Default)
```
Get-Disk [[-Number] <UInt32[]>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

#### ByUniqueId
```
Get-Disk [-UniqueId <String[]>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

#### ByName
```
Get-Disk [-FriendlyName <String[]>] [-SerialNumber <String[]>] [-CimSession <CimSession[]>]
 [-ThrottleLimit <Int32>] [-AsJob] [<CommonParameters>]
```

#### ByPath
```
Get-Disk [-Path <String[]>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob] [<CommonParameters>]
```

#### ByPartition
```
Get-Disk [-Partition <CimInstance>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

#### ByVirtualDisk
```
Get-Disk [-VirtualDisk <CimInstance>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

#### ByiSCSISession
```
Get-Disk [-iSCSISession <CimInstance>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

#### ByiSCSIConnection
```
Get-Disk [-iSCSIConnection <CimInstance>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

#### ByStorageSubSystem
```
Get-Disk [-StorageSubSystem <CimInstance>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

#### ByStorageNode
```
Get-Disk [-StorageNode <CimInstance>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

#### ByStorageJob
```
Get-Disk [-StorageJob <CimInstance>] [-CimSession <CimSession[]>] [-ThrottleLimit <Int32>] [-AsJob]
 [<CommonParameters>]
```

### EXAMPLES

#### Example 1: Get all disks
```
PS C:\>Get-Disk
```

This example gets all disks visible to the operating system.

#### Example 2: Get a disk by disk number
```
PS C:\>Get-Disk -Number 6
```

This example gets disk 6.

#### Example 3: Get all USB disks
```
PS C:\>Get-Disk | Where-Object -FilterScript {$_.Bustype -Eq "USB"}
```

This example gets all disks attached via the USB bus by piping the output of Get-Disk to the **Where-Object** cmdlet, and filtering by the USB value of the Bustype property.

#### Example 4: Get the iSCSI sessions for all iSCSI disks
```
PS C:\>Get-Disk | Where-Object -FilterScript {$_.BusType -Eq "iSCSI"} |
Get-IscsiSession | Format-Table
```

This example gets all disks attached via the iSCSI bus by piping the output of Get-Disk to the **Where-Object** cmdlet, and filtering by the iSCSI value of the Bustype property.
It then passes the Disk objects in the pipeline to the **Get-IscsiSession** cmdlet, which gets the associated iSCSI sessions, and then pipes the output to the **Format-Table** cmdlet for simplified display.

### PARAMETERS

#### -AsJob
Runs the cmdlet as a background job. Use this parameter to run commands that take a long time to complete.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -CimSession
Runs the cmdlet in a remote session or on a remote computer.
Enter a computer name or a session object, such as the output of a [New-CimSession](http://go.microsoft.com/fwlink/p/?LinkId=227967) or [Get-CimSession](http://go.microsoft.com/fwlink/p/?LinkId=227966) cmdlet.
The default is the current session on the local computer.

```yaml
Type: CimSession[]
Parameter Sets: (All)
Aliases: Session

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -FriendlyName
Gets the disk with the specified friendly name.
Enter a friendly name, or use wildcard characters to enter a name pattern.

```yaml
Type: String[]
Parameter Sets: ByName
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -Number
Specifies the disk number for which to get the associated Disk object.

```yaml
Type: UInt32[]
Parameter Sets: ByNumber
Aliases: DeviceId

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

#### -Partition
Accepts a Partition object as input.
The Partition CIM object is exposed by the [Get-Partition](http://technet.microsoft.com/library/85bb3c53-536e-408f-b159-28e91afeb1a1) cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByPartition
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -Path
Contains valid path information.

```yaml
Type: String[]
Parameter Sets: ByPath
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

#### -SerialNumber
Specifies an array of serial numbers associated with disks that this cmdlet gets.

```yaml
Type: String[]
Parameter Sets: ByName
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -StorageJob
Specifies a storage job object that is associated with disks that this cmdlet gets.
To obtain a storage job, use the Get-StorageJob cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByStorageJob
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -StorageNode


```yaml
Type: CimInstance
Parameter Sets: ByStorageNode
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -StorageSubSystem
Specifies the storage subsystem from which this cmdlet gets disks.
To obtain a **StorageSubsystem** object, use the Get-StorageSubSystem cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByStorageSubSystem
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -ThrottleLimit
Specifies the maximum number of concurrent operations that can be established to run the cmdlet.
If this parameter is omitted or a value of `0` is entered, then Windows PowerShell® calculates an optimum throttle limit for the cmdlet based on the number of CIM cmdlets that are running on the computer.
The throttle limit applies only to the current cmdlet, not to the session or to the computer.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -UniqueId
Gets only the disks with the specified IDs.
Type one or more IDs (separated by commas).

```yaml
Type: String[]
Parameter Sets: ByUniqueId
Aliases: Id

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### -VirtualDisk
Accepts a VirtualDisk object as input.
The Virtual Disk CIM object is exposed by the [Get-VirtualDisk](http://technet.microsoft.com/library/0eeba53f-6468-485f-a680-49260b4c83f0) cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByVirtualDisk
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -iSCSIConnection
Accepts an iSCSIConnection object as input.
The iSCSI Connection CIM object is exposed by the [Get-IscsiConnection](http://technet.microsoft.com/library/e566d297-76ad-48d0-b5af-11674f23b080) cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByiSCSIConnection
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

#### -iSCSISession
Accepts an iSCSISession object as input.
The iSCSI Session CIM object is exposed by the Get-IscsiSession cmdlet.

```yaml
Type: CimInstance
Parameter Sets: ByiSCSISession
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```






> [呼呼呼山](http://code4fun.me)
> 2020-06-24 18:44:39
