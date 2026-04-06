#Digital_Forensics 
#Windows 
[[Windows Registry]]

The Windows Registry is not the only place that record the users activity on the Operating System . The Windows File System also plays a vital role , there are dedicated location from where we can collect and analyze the artifacts.

---

## Why Do We Need a File System in the First Place?

Storage devices like Hard Disk's,USB Sticks,etc's are just a collection of bits . In order to make these bits have a meaning behind them , the bits have to be properly organized and structured. To fulfil this purpose we need a file system. 

Different File systems organize bits differently for easy interpretation.

## 1) FAT File System (File Allocation Tabel)

The FAT File System was used as the default file system for all the different versions of Microsoft Windows until the famous Windows XP. In the XP version the FAT File system was replaced by the NTFS file system and has been the default since then.

#### Working of FAT File System

The FAT File System has three major component in it , which are as follows- 

1. <mark style="background: #FF5582A6;">Clusters</mark> - Cluster is the basic storage unit of the FAT File System. Bits are grouped together to form a cluster and multiple clusters grouped together form a file . So we can say that file is a group of clusters.

2. <mark style="background: #FF5582A6;">Directory</mark> - The Directory is responsible for holding valuable information in it in relation to the different files. The information stored here include name of the file,starting cluster of the file,File length,etc.

3. <mark style="background: #FF5582A6;">File Allocation Table</mark> - The File allocation table has all the clusters indexed in it, in the form of a linked list. 
	
	A cluster being used points to the next cluster and this keeps on happening until a cluster points to the EOF Marker or the End of File Marker which tells us that no more clusters belong to the file .   

Example - Lets say that I have a file that has 10 clusters associated with it. Now when we open the file the starting cluster will be obtained from the directory and based on the starting clusters location the rest of the clusters will be retrieved from the File Allocation Table as it indexes the clusters in the form of a linked list .

#### Different Versions of FAT File System

|         Properties         |  FAT12   |  FAT16   |    FAT32    |
| :------------------------: | :------: | :------: | :---------: |
|   No of Addressable Bits   |    12    |    16    |     28      |
| Maximum Number of Clusters |  4,096   |  65,536  | 268,435,456 |
| Supported Size of Cluster  | 512B-8KB | 2KB-32KB |  3KB-32KB   |
|    Maximum Volume Size     |   32MB   |   2GB    |     2TB     |

Nowadays, FAT12 is rarely used due to its limited capacity. However, FAT16 and FAT32 are still used in some cases such as SD cards and USB drives. However ,the maximum file size supported by FAT32 is 4 GB, which makes it a limiting factor, as high-quality images and videos can exceed this limit. So to overcome this problem the exFAT File System was made

## 2) exFAT File System

The exFAt File system was made specifically for SD Cards because of 2 reasons - 

1. With increase in time and technology , the quality,size,resolution of images and videos have grown significantly making it impossible to use FAT32 in our SD Cards due to its limiting factor. 

2. Using the NTFS File System would also have been impractical as we did not require the security measures or the additional overhead in our SD Cards. 

Hence the exFAT File System was made . The exFAT file System supports a cluster size between 4KB and 32MB , making the maximum file size and volume size to be a whopping 128 Petabytes

## 3) NTFS File System(New Technology File System)

The new technology file system has many new features in it in comparison to the FAT File system . The features of the NTFS File System as follows -
### 1)Journaling
 In NTFS file system there is a $LOGFILE in the volume's root directory which record all changes related to the metadata of files .This include properties like file name, file size,file path,file access,etc. 
 <mark style="background: #FFB86CA6;">Example</mark> - Lets say that we have a file called "Hello.txt". Now the user changes the location of the file from one place to another and also adds content to the file , making the size of the file bigger. These all changes would be firstly written to the $LOGFILE and then to the disk.
 
 The purpose of the $LOGFILE is to record transactions before they are applied so that even if a crash happens everything works normally after.

### 2)Access Control 
 In NTFS file system there is a access control list which allows the administrators to set permission for each user on what they can access,edit and execute. <mark style="background: #FFB86CA6;">NOTE</mark> - This feature was not present in FAT,and hence makes NTFS more secure. 

### 3)Volume Shadow Copy
In NTFS, Volume Shadow Copy is a service using which snapshots of our system’s drive are taken at a specific point in time. These snapshots can be used to restore files and even the complete system if things are not working properly.
Volume Shadow Copy works on the principle of <mark style="background: #FFB86CA6;">Copy-On-Write</mark>.

<mark style="background: #FFB86CA6;">Example</mark> – Let’s say we have a file called "File.txt" which is currently empty. A snapshot of the drive is taken, but nothing is stored in the shadow copy at that moment.
Now when the user writes some content into the file, before the file is actually updated/saved, there is a very small delay. During this time, the original data of the file (i.e., how it was at the time of the snapshot) is saved into the Volume Shadow Copy. After that, the new content is written into the file.

This is how the Volume shadow copy works.

<mark style="background: #FFB86CA6;">NOTE</mark> - The volume shadow copy has been noticed to get deleted by ransomware attackers. This is done so that the users cannot simply recover all of his/her data.

### 4)Alternate data Streams 
In NTFS, a file is basically a stream of data (by stream of data we mean the content it holds). Each file has at least one data stream associated with it, which is called the default data stream. This is what we see when we open the file. However, in NTFS a file can have multiple data streams associated with it, which are not visible through normal file operations. These are called Alternate Data Streams (ADS). 

<mark style="background: #FFB86CA6;">Example</mark>– Lets say that we have a file "File.txt" which has the word "Hello" in it. This is the only thing that we will see normally, but if we access the alternate data streams of File.txt, we can find additional data/content attached to the same file which is simply not visible. 

| File.txt        | "Hello"       | Default Data Stream/Content   |
| --------------- | ------------- | ----------------------------- |
| File.txt:secret | "Secret Data" | Alternate Data Stream/Content |

### 5)Master File Table 
The master file table in  NTFS is similar to the File Allocation Table . The MFT or Master File Table is like a centralized database that stores information about all the files and folders that are present on the disk . The Master File Table is also is much more detailed than the File Allocation Table of FAT. <mark style="background: #FFB86CA6;">Tool used for parsing files is MFTECmd.exe(Master File Table Explorer in Command Line) and for viewing the output we have EZViewer</mark>.

Important Files in MFT from a forensics point of view -

1. <mark style="background: #FF5582A6;">$MFT</mark> - The $MFT stores information about the mapping of clusters for different files along with their metadata. When an OS boots up for the first time , it uses <mark style="background: #FFB86CA6;">VBR or Volume Boot Record</mark> to find the location of the $MFT . Once the location is known , anytime a file is opened the OS refers to the $MFT to retrieve the clusters belonging to the specific file so that reconstruction can happen.

2. <mark style="background: #FF5582A6;">$LOGFILE</mark> -  File used for Journaling.

3. <mark style="background: #FF5582A6;">$UsrJrnl</mark> - This stands for Update Sequence Number (USN)  Journal. This basically keeps track of the changes that   have been made to the metadata of a file and why , for a long period of time. 
   Now this can be considered very similar to the $LOGFILE but both of these files have completely different purposes.The $LOGFILE is used to record changes made to the metadata of a file so that if a crash happens then the system can recover easily , while on the other hand $UsrJrnl is used to record changes made to the metadata of a file so that proper tracking can be done. 

## 4) Important Locations that show evidence of execution 


### 1)Windows Prefetch Files
When a program is executed in Windows, information related to that program is saved to the prefetch files(which have an extension of <mark style="background: #FF5582A6;">".pf"</mark>). This stored information is used by the OS to load programs more quickly in case of frequent use.

<mark style="background: #FFB86CA6;">Location - C:\Windows\Prefetch</mark>

The prefetch files are great for analysis from a forensics point of view because this hold information like -

1. Number of times the program was executed
2. Files used by the program if any
3. Last run time of application

<mark style="background: #FFB86CA6;">Tool used - PECmd.exe</mark>

### 2)Windows 10 Timeline
Windows keep track of the recently used application in an SQLite Database which is called the Windows 10 Timeline.The information that can be stored here is - 

1. Last run time of application
2. Time the application was in focus.By focus we mean the time during which the user was using the application in foreground and not in the background.

Location - <mark style="background: #FFB86CA6;">C:\Users\{username}\AppData\Local\ConnectedDevicesPlatform\{folder}\ActivitiesCache.db</mark>

<mark style="background: #FFB86CA6;">Tool Used - WxTCmd.exe</mark>

### 3)Windows Jump List 
In Windows, Jump Lists allow users to quickly access recently used applications, files, and folders. This feature was introduced to improve usability by providing quick access to items directly from the taskbar itself. When a user right-clicks on an application's icon on the taskbar, a Jump List appears displaying recently opened files, folders, and frequently performed tasks associated with that application. This allows users to reopen files or perform common actions without navigating through the application manually.

Jump List can include information which as follows - 

1. First time the application was executed
2. Last Time the application was executed
3. Application used for opening a file

Location - <mark style="background: #FFB86CA6;">C:\Users\{username}\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations</mark>
<mark style="background: #FFB86CA6;">Tool used - JLECmd.exe</mark>

## 5) File/Folder Knowledge 

### 1)Windows Shortcut Files 
Windows creates a shortcut file for each file opened either locally or remotely.These shortcut files contain information like - 

1. First Time the file was opened 
2. Last Time the file was opened 
3. Path of the opened file 

Location -

1. <mark style="background: #FFB86CA6;">C:\Users\{username}\AppData\Roaming\Microsoft\Windows\Recent</mark> 

2. <mark style="background: #FFB86CA6;">C:\Users\{username}\AppData\Roaming\Microsoft\Office\Recent</mark>

<mark style="background: #FFB86CA6;">Tool used - LECmd.exe</mark>

The creation date of the shortcut file points to the date/time when the file was first opened. 

The date/time of modification of the shortcut file points to the last time the file was accessed.

### 2)IE/Edge History
Internet Explorer's/Older versions of Edge are great sources when it comes to forensics . These basically record a users browser activity . Information that is gathered and stored as follows - 

1. Websites Visited
2. Cached URL's
3. Files Downloaded 
4. Files Opened locally,etc.

IE/Edge history also sometimes record the files that were opened locally on the system, which we can tell by looking at the prefix(<mark style="background: #FFB86CA6;">prefix - "file:///"</mark>).

<mark style="background: #FFB86CA6;">Location - C:\Users\{username}\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat</mark>
<mark style="background: #FFB86CA6;">Tool - Autopsy</mark>

### 3)JumpList's
Same as the one above 


## 6)External Devices / USB Forensics 

### 1)Setupapi.dev.log 
When any new device is attached to the system , the information related to its setup is logged and stored . 

<mark style="background: #FFB86CA6;">Location - C:\Windows\inf\setupapi.dev.log</mark>

The logged information include - 

1. Serial number and Device ID 
2. First time the device was connected
3. Last time the device was connected 

### 2)Shortcut Files
Same as above 

