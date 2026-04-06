#Digital_Forensics 

Volatility is a open-source forensic tool that is made for analyzing memory dumps(snapshots of the RAM of a system) to gather information like processes running,network connections,open files,etc.

---


## 1) Memory Extraction
Memory extraction is the fist process, when beginning with a investigation . Now there are two types of system ,each having a different approach for extraction. These are as follows-

1. <mark style="background: #FF5582A6;">Bare-Metal System</mark> - These are those systems which are running the OS directly off the hardware(there is no virtualization layer).Tools that can be used are for the process- <mark style="background: #FFB86CA6;">FTK Imager, RedLine,Memoryze,FastDump</mark>.
   For most of the tools used we will get a <mark style="background: #FF5582A6;">.raw</mark> file as output.

2. <mark style="background: #FF5582A6;">Virtual Machines</mark> - Here we can extract the virtual memory file from the host machines's drive.Output depends on the hypervisor we are using. 
   VMWARE - .vmen
   Hyper-V - .bin
   Parallels - .mem
   VirtualBox - .sav

## 2) Plugins
In Volatility 2 the users were required to specify a profile, which was basically a way of telling volatility about the OS that was being used along with its version numbers. This was difficult to determine and using the wrong profile could lead to incorrect results . So to overcome this problem , profiles were completely removed from Volatility 3 and now the tool automatically detects the operating system and build information . 

Plugin naming has also changed significantly.Instead of using universal plugin names, plugins are now grouped by the operating system such as windows, Linux,mac because each OS has different memory structures.

## 3) Image Info
As we already know ,In Volatility 2 the analysts had to manually specify a profile that matched the operating system and its build number when analyzing a memory dump. However, Volatility 3 removed the concept of profiles.
Instead, it automatically detects the operating system and its corresponding build information from the memory image.

Even though this detection happens automatically, analysts may still want to manually view basic information about the memory dump, such as the operating system details and build information. Volatility 3 provides dedicated plugins for this purpose.The plugins for different operating systems as follows - 
- <mark style="background: #FFB86CA6;">windows.info</mark> – Displays information about Windows memory images
- <mark style="background: #FFB86CA6;">linux.info</mark> – Displays information about Linux memory images
- <mark style="background: #FFB86CA6;">mac.info</mark> – Displays information about macOS memory images

Command(Example)- <mark style="background: #FF5582A6;">python3 vol.py -f memory.raw windows.info</mark>
![[Windows.info.png]]

## 4) Listing Processes and Connections

### A)Processes
There are 3 different plugins that we can use for getting the Process information. Each have their own pros and cons which we will discuss, but first lets see about processes in RAM -

RAM is divided into two logical halves. One being the kernel space and the other being the user space.The kernel space is utilized by the Windows Operating system itself for core functionalities and the User space is used for the normal programs. 

Now when a program is launched/executed we get two different types of information for the same program in two different regions of RAM. One in the kernel space and other in the User space . 

In the kernel space a EPROCESS Structure is made for the program which is basically like a card holding critical information related to the program . This information can include -

- Process ID (PID)
- Parent Process
- Threads and many more 

 In the kernel space every program has its own EPROCESS Structure which are organized together by utilizing a doubly linked list

On the other hand in the User Space , the actual data of that program is being stored . Lets take an example for better understanding.

Lets say that i have Firefox.exe which has to be launched . Now when I launch/execute the program,two types of information will be made for it . One in the kernel space and One in the user space . The kernel space and user space can look like this- 
##### 1)Kernel Space 
![[EPROCESS_Volatility.png]]
The above image shows us the EPROCESS Structure made for Firefox.exe,holding critical information

##### 2)User Space
![[User Space_Volatility.png]]
The above image shows us about the actual data for the Firefox program.

#### A)pslist
This is the most basic way that we can use.Here the processes are listed by simply traversing the <mark style="background: #FFB86CA6;">Active Process List</mark> which is the doubly linked list of EPROCESS structures maintained by the windows kernel.

Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.raw windows.pslist</mark>

|               Advantages               |                                                                                       Disadvantages                                                                                        |
|:--------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|          1) Fast and Reliable          |                                                                   1) Unreliable if the Active Process List is corrupted                                                                    |
|    2) Shows metadata of the process    | 2) pslist cannot detect hidden programs . If a malware is successfully able to remove itself from the linked list then pslist will not be able to detect it even though it has a presence. |
| 3) Shows recently terminated processes |                                                                                                                                                                                            |

#### B)psscan
psscan doesn't rely on the Active Process List , it list processes based on the EPROCESS Structure.

Command -<mark style="background: #FF5582A6;"> python3 vol.py -f memory.ray windows.psscan</mark>

|                                                Advantages                                                |           Disadvantages           |
| :------------------------------------------------------------------------------------------------------: | :-------------------------------: |
| 1) Detects hidden processes . Even if a program is not in the linked list , using psscan we can find it. | 1) Slower in comparison to pslist |
|                             2) Can find processes that have been terminated                              |  2) Can produce false positives   |


#### C)pstree
In pstree the programs are listed in a hierarchical manner based on the parent child relation between the processes.

Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.raw windows.pstree</mark>


|                                                      Advantages                                                       |                                      Disadvantages                                       |
| :-------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------: |
| 1) Show process relationships, so can help us in finding suspicious process and suspicious child parent relationships | 1) Relies on Active Process list, making this unreliable if the list is corrupted itself |
|                                   2) Can be used to reconstruct the attack timeline                                   |                          2) Cannot find the orphaned processes                           |

### B)Network Connections
There are 2 plugins here using which we can get information about the network connections . These plugins are as follows 

#### A)Netstat
By using netstat we can identify the network connections that existed in the system at the time of the snapshot . The netstat plugin looks for network-related kernel objects and extracts them . 

These network-related kernel object can hold a lot of helpful information like -

1. Local IP Address
2. Local Port
3. Remote IP Address
4. Remote Port 

Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.raw windows.netstat</mark>

| Advantages                             | Disadvantages                |
| -------------------------------------- | ---------------------------- |
| 1) Shows active network connections    | 1) Unstable in Volatility 3  |
| 2) Links network activity to processes | 2) May miss some connections |

#### B)dlllist
DLL are files which contain reusable code in them which is used by programs. So the dlllist will give us the DLL's related to programs 

Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.py windows.dlllist</mark>


## 5) Volatility Hunting and Detection Capabilities

In malware hunting we have two plugins which are extremely important . The plugins and their respective responsibilities as follows - 

1. <mark style="background: #FF5582A6;">YARA Scan</mark> - Volatility allows the user to run memory files against YARA rules so that strings,patterns,etc can be extracted.
   
   Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.raw windows.yarascan</mark>

1. <mark style="background: #FF5582A6;">malfind</mark> - This is a plugin that is used to find malicious code injections into processes. Usually attacker inject malicious codes into processes so that a malicious programs run under legitimate name.
   
   ##### How Code Injection Works?
   
   Each process has its own memory associated with it in the user space which is divided into multiple segments. Example -
   ![[Code Injection Basics.png]]
   In the above image the segments are Code Section , Heap Stack, DLL'S. Now each of these sections have permissions.
   
   R -> Flag that specifies reading
   W -> Flag that specifies writing
   X -> Flag that specifies execution 
   
   Now when code injection is done by malicious actors . A new segment/section is firstly made into the memory which has generally RWX permission associated with it(By RWX we mean having the ability to READ+WRITE+EXECUTE). Within these new segments , malicious codes are inserted which are then executed .
   
   Another way to find out if there is malicious injection is to see if there is no file backing of a segment. Generally Files in segment belongs to specific folders but in injected code there is no file that exist, only code. Hence making the segment with no file backing. 
   
   So finally if a segment of a processes memory has no file backing and RWX permission , then we can say that malicious code has been injected into the process.
   
   Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.raw windows.malfind</mark>


## 6) Advanced Memory Forensics
In the previous sections we discussed about basic memory forensics techniques,such as:

- listing processes
- identifying code injection
- viewing command history

These techniques work well for detecting simple malware activity. However, advanced attackers do not rely on such basic methods. Instead, they often use techniques that allow them to hide their presence from the operating system itself.

This is typically done by manipulating kernel-level objects. Some of the common kernel components that attackers target include:

- System Call Tables
- Device Drivers
- Interrupt Functions
- Callback Functions
- Process Scheduler

By modifying these structures, malware can control how the operating system behaves, allowing it to hide processes, files, network connections, or other malicious activity.

#### A)Memory Layout in Windows
As we already know, system memory is logically divided into two main regions:User Space and Kernel Space
##### <mark style="background: #FFB86CA6;">User Space</mark>
User space is where normal programs run. Examples include:

- browsers
- games
- text editors
- command shells

These programs do not have direct access to the systems hardware. This restriction exists to prevent rogue or malicious programs.
##### <mark style="background: #FFB86CA6;">Kernel Space</mark>
Kernel space contains the core components of the operating system, including:

- the Windows kernel
- memory manager
- process scheduler
- device drivers

The kernel has full control over the system,including direct access to hardware.

##### <mark style="background: #FFB86CA6;">Role of the Kernel</mark>
Because user programs cannot directly interact with the hardware of the system, the kernel acts as a middleman between the programs and system resources.

Whenever a program needs to perform a privileged operation, it must request permission from the kernel.

These requests are made using system calls.

##### <mark style="background: #FFB86CA6;">System Calls</mark>
A system call is a way that allows programs running in user space to request services from the operating system kernel.

Programs use system calls to perform tasks such as:

- creating files
- allocating memory
- creating new processes
- accessing hardware
- sending network data

For example, if a program wants to create a file, it cannot directly write to the disk. Instead, it sends a system call to the kernel requesting the file to be created.

The kernel then processes the request and performs the operation safely.

##### <mark style="background: #FFB86CA6;">Role of Drivers</mark>
When a request involves hardware interaction, device drivers come into play.

A driver is a special piece of software that allows the operating system to communicate with a specific hardware device.

Hardware devices are built differently by different manufacturers. Because of this, the operating system cannot directly communicate with every piece of hardware. Drivers solve this problem by acting as translators between the operating system and the hardware.

In simple terms:

<mark style="background: #FF5582A6;">Operating System ↔ Driver ↔ Hardware</mark>

Lets take an example for better understanding - 

Consider a game that requires heavy graphical processing.

1. The game runs in user space.
2. When the game needs to render graphics, it requests GPU access through a system call.
3. The request is sent to the kernel.
4. The kernel then interacts with the GPU driver.
5. The GPU driver translates the operating system's instructions into commands that the **GPU hardware** can execute.

This process allows the program to use the GPU without directly interacting with the hardware itself.

### System Service Descriptor Table (SSDT) and Hooking 
The system service descriptor table can be considered as a database which maps system call numbers along with kernel level functions . Like whenever a system call is received by the kernel , it uses the SSDT table to run the correct code . Lets take an example - 

We have a program who want to make a file . Now this action cannot be performed by the program itself so a system call will be send to the kernel . In this system call there will be a number which will tell the function that has to be executed. This will be done by looking at the mapping in the SSDT Table. In our case the system call will be 0x12 which is linked to NTCreate File .

| System Calls | Kernel Fuction      |
| ------------ | ------------------- |
| 0x01         | NTTerminate Process |
| 0x11         | NTRead File         |
| 0x12         | NTCreate File       |

Hooking is a technique where the normal path of execution is changed in such a manner that the malicious code runs before the original code .

Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.raw windows.ssdt</mark>

Adversaries not only use these techniques for stealth but also uses malicious driver files . There are two plugins that will help us in finding these driver files -

1. <mark style="background: #FFB86CA6;">driverscan</mark> - This plugin will scan for drivers that were present on the system during extraction. driver-scan also helps us in finding information that the module plugin has missed.
   Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.raw windows.driverscan</mark> 

2. <mark style="background: #FFB86CA6;">modules</mark> - This is a extremely helpful plugin when the malware is active as it will dump all of the loaded kernel modules but if the malware is staying idle then this plugin may miss some stuff. 
   
   Command - <mark style="background: #FF5582A6;">python3 vol.py -f memory.ray windows.modules</mark>

Other plugins that can help in our investigation are - 
- modscan
- driverirp
- callbacks
- idt
- apihooks
- moddump
- handles 