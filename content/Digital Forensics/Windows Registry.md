#Digital_Forensics 
#Blue_Teaming 
#Windows 
[[Windows_Forensics_CheatSheet.pdf]]

Window's is one of the most widely used operating system for both private as well as commercial use. Having knowledge on how to perform forensic analysis on a windows machine is a must for any person working in forensics.  

---

On a window's machine , a person's activity can be traced back easily by using the multiple artifacts the operating system makes for an event.

## Windows's Registry
The Windows Registry is a collection of databases that store system configuration data in it. This data can be related to hardware,software,user information etc. Windows Registry consists of keys and values 

Now in windows there are 5 registry's keys each storing different information in it . The registry keys as follows -

### 1) HKEY_CURRENT_USER(HKCU) 
HKCU shows information related to the account using which the user signed in . Example - Lets say I have 2 account's/user's A and B . When I will use account A to log in ,the HKCU will show config information in relation to account A , but if I use account B , then config information in relation to account B will be shown in HKCU.

The configuration info varies from user to user

### 2) HKEY_USERS(HKU)
HKU stores config info for multiple users in it . Example - Lets say I have three accounts/users - A,B,C . HKU will store config information for all of these users/accounts . Also
HKCU can be metaphorically considered as a subset of HKU.

### 3) HKEY_LOCAL_MACHINE(HKLM)
HKLM stores system-wide configuration in it . This information is universal , meaning that it is same for all the users/accounts unlike the HKCU. This includes information like Drivers,Hardware Configuration,System Policies,etc.

### 4) HKEY_CLASSES_ROOT(HKCR)
HKCR is a merged key consisting of HKLM and HKCU . HKCR ensures that our files are opened using the correct programs. We already know that HKLM store system-wide config information in it which remains the same for all the users , on the other hand HKCU stores information specific to a user. Lets take an example for better understanding. Example - We have a fie with the extension ".txt", now in HKLM the program specified to open a .txt file is Notepad but our user doesn't use Notepad,instead he uses Vim , now the txt file will always be opened using Vim for that specific account . 

In general what happens is that when we have to open a file,  based on the extension a program is selected . If there is one specified in the HKCU then that program is used to open the file otherwise the system fall backs on HKLM and uses the default program to open the file 
![[HKCR.png]]


### 5) HKEY_CURRENT_CONFIG(HKCC)
HKCC registry key stores information about the hardware profile that is currently being used at system startup. HKCC stores information like - Resolution,Monitor Type,Currently Installed Printers,etc. 


## Where are the registry keys located on a disk image

The majority of the hives are located in <mark style="background: #FF5582A6;">C:\Windows\System32\Config</mark>

1. DEFAULT - <mark style="background: #FFB86CA6;">HKEY_USERS\DEFAULT</mark>
2. SAM - <mark style="background: #FFB86CA6;">HKEY_LOCAL_MACHINE\SAM</mark>
3. SECURITY - <mark style="background: #FFB86CA6;">HKEY_LOCAL_MACHINE\Security</mark>
4. SOFTWARE - <mark style="background: #FFB86CA6;">HKEY_LOCAL_MACHINE\Software</mark>
5. SYSTEM - <mark style="background: #FFB86CA6;">HKEY_LOCAL_MACHINE\System</mark>

There are also two other sources holding a user information . These are -
1. NTUSER.DAT - <mark style="background: #FFB86CA6;">C:\Users\Username</mark>
2. USRCLASS.DAT - <mark style="background: #FFB86CA6;">C:\Users\Username\AppData\Local\Microsoft\Windows</mark>

Both of these files are hidden but are very important

## The AmCache Hive (C:\Windows\AppCompat\Programs\Amcache.hve)

The Amcache hive stores metadata related to the files that were recently opened/executed. This includes parameters like hash of the file,size of the file,path,timestamps(when file was created,modified,last executed),etc.

From a forensics point of view , the Amcache hive is very important 


## Transaction Logs and Backups
Transaction Logs and Registry Backups are two very important sources when performing forensic analysis

#### 1) Transaction Logs (Stored in the same directory as the hive itself with a .LOG extension)
Whenever changes have to be made to a registry hive, those changes are firstly recorded by the transaction logs . The transaction logs ensures that our system works perfectly fine after the updation and there are no problem with the registry values. This is ensured by following the simple rule of "<mark style="background: #FF5582A6;">No Partial Updation</mark>"

Lets see what happens under the hood when updation is done to a registry value-

Lets say that a registry value is being updated and during that time the system crashes or loses it power.This causes an immediate shutdown ,resulting in a partial updation of the registry value.This partial updation can cause our operating system to break, so in order to protect this from happening transaction logs are used.

The next time our system boots the operating system will look at the transaction log and will determine that a partial updation was made . If the updation was not committed then a rollback occurs causing the registry value to be come back to its original value before the updation .If already a commit has been made then replay occurs ensuring that complete change occurs.

<mark style="background: #FF5582A6;">This is what the transaction log do. It makes sure that either complete updation occurs or there is no updation at all</mark>

#### 2) Registry Backup (C:\Windows\System32\Config\RegBack)
The registry backup is responsible for holding a snapshot of the registry hives.

Registry backup is really essential because this allows our system to come back to its previous registry configuration after wrong/incorrect modification were made, resulting in our operating system to break or to not work properly.

From a forensics point of view we can use the registry backup to see the updation that have been made by comparing different snapshots with each other 


## Data Acquisition
Extracting registry hives from a disk image or a live system is part of data acquisition. While registry hives can be directly copied from an offline disk image, live systems require specialized tools because active registry hives are locked by the operating system. Some of these tools which we can use are as follows - 

1. KAPE
2. Autopsy
3. FTK Imager

We will use RegRipper and Registry Explorer to perform analysis on the acquired data.

## System Information and System Accounts

##### 1) OS Version - <mark style="background: #FFB86CA6;">SOFTWARE\Microsoft\Windows NT\CurrentVersion</mark>
##### 2) Current Control Set - <mark style="background: #FFB86CA6;">SYSTEM\Select\Current</mark> 
A control set can be considered as the blueprint our operating system follows when it boots-up. There are many different control sets that our operating system can choose from, each having a different configuration from the other . 

Basically a control set holds a snapshot of the system configuration in relation to hardware,drivers,services,etc.

Current control Set refers to the control set that was used by the operating system to boot up.

Commonly we see two control sets, these are -

1. <mark style="background: #FF5582A6;">ControlSet001</mark>- Control set used by the OS to boot up(<mark style="background: #FFB86CA6;">SYSTEM\ControlSet001</mark>)

2. <mark style="background: #FF5582A6;">ControlSet002</mark> - Last known good configuration that was used by the OS to bootup(<mark style="background: #FFB86CA6;">SYSTEM\ControlSet002</mark>)

##### 3) Computer Name - <mark style="background: #FFB86CA6;">SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName</mark>

##### 4) Time-zone Information - <mark style="background: #FFB86CA6;">SYSTEM\CurrentControlSet\Control\TimeZoneInformation</mark>

##### 5) Network Interfaces and Past Networks - <mark style="background: #FFB86CA6;">SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces
</mark>
For seeing the past networks that a system was connected to are -
1. <mark style="background: #FFB86CA6;">SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged</mark>

2.  <mark style="background: #FFB86CA6;">SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed</mark>
##### 6) Autostart Programs

1. NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run
2. NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce
3. SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
4. SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run
5. SOFTWARE\Microsoft\Windows\CurrentVersion\Run

##### 7) SAM Hive and User Information
Stores data related to the user like account information,login information,group information,last login , no of times the user logged in,last failed login,last password change,expiry of password,etc . Location is at - <mark style="background: #FFB86CA6;">SAM\Domains\Account\Users</mark>


## Usage of Different Files and Folders

#### 1) Recent Files
Windows keep a track of the files that were recently opened/executed for each user. The recently used files list we see in the windows explorer is this. Location where this information is stored is - <mark style="background: #FFB86CA6;">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs</mark>
The Registry Explorer displays information to us in such manner that the recently used files are on the top and the older ones at the bottom. 

If we want to look for files with specific extension then we can simply go to the location . Ex - <mark style="background: #FFB86CA6;">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.pdf</mark>
#### 2) Office Recent Files
Similarly to windows explorer, Microsoft Office also keeps a record of the recently opened files .Location for this is-
<mark style="background: #FFB86CA6;">NTUSER.DAT\Software\Microsoft\Office\VERSION</mark>

#### 3) ShellBags
In windows, opening folder can be customized/changed based on the user preferences. These changes can be brought to specific folders of the system . So ShellBags hold information in them which explain how different folders have to be displayed(Ex- View,Position,Size of folder,etc) . 

Shellbags also records the recently used folders,making it a great source from a forensic point of view.   

Location  
1. USRCLASS.DAT\Local Setting\Software\Microsoft\Windows\Shell\Bags

2. USRCLASS.DAT\Local Setting\Software\Microsoft\Windows\Shell\BagsMRU

3. NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags

4. NTUSER.DAT\Software\Microsoft\Windows\Shell\BagsMRU

To get information related to ShellBags use <mark style="background: #FF5582A6;">ShellBag Explorer</mark>

#### 4) Open/Save and LastVisited Dialog MRU's
When we open or save a file, a dialog box appears asking us where to save or open that file from. It might be noticed that once we open/save a file at a specific location, Windows remembers that location. This implies that we can find out recently used files if we get our hands on this information. We can do so by examining the following registry keys 

1. NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU

2. NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPiDlMRU 

#### 5) Search Bars
Whenever we search for a file in the windows explorer , the address bar where we type our filename/path is also logged .

Location - 

1. NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths

2. NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery

## Evidence of Execution

#### 1) UserAssist
Windows keep track of the programs/application that were launched using the Windows Explorer. Here information related to the program is stored for statistical purposes . This information can include parameters like number of times the program was executed,time of the launch,etc.

UserAssist doesn't include those programs in it that were launched using the command line 

Location - <mark style="background: #FFB86CA6;">NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count</mark>

#### 2) ShimCache (Also Called as AppCompatCache)(SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache)

Tools that has to be used to parse ShimCache is <mark style="background: #FF5582A6;">AppCompatCache Parser</mark>

Firstly, let us understand what a **shim** is and why it is important.

As operating systems evolve from one version to another, many internal changes take place. These changes can sometimes break older applications. **Shims exist to ensure backward compatibility**, allowing older ".exe" and ".dll" files to run correctly on newer versions of Windows.

Example - Suppose there is an ".exe" file present on the system that needs to be executed. Before execution, Windows must ensure that the application is compatible with the current operating system. This responsibility lies with the **Application Compatibility Engine**.

- The Application Compatibility Engine evaluates the executable to determine whether it may face compatibility issues.

- If a compatibility issue is identified, a **shim** is applied **at process creation time**.

- A shim acts as a **middleman or translator** between the application and the operating system,telling the OS on how to take inputs from the application.   

This mechanism allows the OS to execute older applications without modifying the application’s original code.

If Windows had to perform this compatibility analysis every time an executable was encountered or executed, it would result in wasted computing resources.

To avoid this:

- Windows stores **metadata about the evaluated executables** in the **Shimcache** (Application Compatibility Cache).

- Shimcache helps the OS remember whether an executable requires special handling or not.
  
- This prevents repeated compatibility checks and improves system efficiency.

This is the reason why Shimcache is so important . 


#### 3) BAM and DAM

<mark style="background: #FFB86CA6;">BAM(Background Activity Monitor)</mark> -  Responsibility of BAM is to keep tabs on the background activity of the applications. Location - <mark style="background: #FF5582A6;">SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}</mark>

<mark style="background: #FFB86CA6;">DAM(Desktop Activity Moderator)</mark> - Responsible for power optimization . Location - <mark style="background: #FF5582A6;">SYSTEM\CurrentControlSet\Services\dam\UserSettings\{SID}</mark>


## External Devices or USB Forensics

#### 1) For Device Identification
For device identification we can look at two locations. These locations store Vendor ID, Product ID and Version of the USB plugged in.The location as follows -

1. SYSTEM\CurrentControlSet\Enum\USB
2. SYSTEM\CurrentControlSet\Enum\USBSTOR

#### 2) First and Last Time
The windows registry is so good at recording the user's activity that it keeps a track of when a USB was plugged into the system for the first time . It also records the last time the USB was plugged in.There are different codes associated with each event . These codes are as follows -

1. 0064 - First Connection Time
2. 0066 - Last Connection Time 
3. 0067 - Last Removal Time

#### 3) USB Device Volume Name
The Device name of the connected drive can be looked at the following location - <mark style="background: #FFB86CA6;">SOFTWARE\Microsoft\Windows Portable Devices\Device</mark>









