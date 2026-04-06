#Digital_Forensics 

Kroll Artifact Parser and Extractor is a digital forensics and incident response tool which allows analyst's to quickly extract forensic artifacts from a windows machine and process them for suspicious activities using specially crafted modules. 

---

## Why we need KAPE in the first place ?

If we follow the traditional methods for digital forensics then there are quite some steps that are followed - 

1. Create a full disk image
2. Load the image into forensics tool 
3. Analyze the artifacts one by one to ensure everything is normal

Now this process will take a lot of time which we simply cannot afford when a malware/malicious program is on our network and host . Time is of the essence,hence KAPE comes into play.

KAPE only extracts the most critical artifacts that exist like prefetch files,amcache,shimcache,etc, allowing the analysts to start their investigation within minutes.

## 1)Working of KAPE
KAPE holds the responsibility for performing two core operation - 

1. <mark style="background: #FF5582A6;">Artifact Collection</mark> - Artifact collection refers to the gathering of critical files from the window machine . This is achieved by using <mark style="background: #FFB86CA6;">Target</mark> (Target refers to the forensic artifacts that need to be collected) 

2. <mark style="background: #FF5582A6;">Artifact Processing</mark> - Artifact Processing is done by using Modules . Modules are specially crafted programs that extract information from the artifacts collected during the collection phase.

These operations are orchestrated/controlled by the <mark style="background: #FF5582A6;">KAPE Engine</mark> which manages the artifact collection,module execution and output generation. 

#### How files are copied/collected by KAPE ?
The files that need to be collected are firstly added to a queue where KAPE follows a two passes approach to ensure all the files are collected.

In the first pass, KAPE attempts to copy all files that are not locked by the operating system and therefore are accessible through normal file system operations.  
Any files that cannot be copied during this phase are then added to a secondary queue.As These files are typically locked by the OS and cannot be accessed through standard methods,KAPE uses <mark style="background: #FF5582A6;">raw disk reads</mark>.

This is how KAPE copies all the files.

## 2)Target Options
A target file (has extension <mark style="background: #FFB86CA6;">.tkape</mark>) is basically a configuration file which tells KAPE two fundamental thing - 

1. Which artifact needs to be collected
2. What is the location/path to that artifact 

Now there are two ways using which we can specify our artifacts to KAPE. This is achieved by using - 

- <mark style="background: #FF5582A6;">Target Files</mark> - Specifies one single source

- <mark style="background: #FF5582A6;">Compound Targets</mark> - Using compound target we can specify multiple targets with just one command 

![[Target Folder KAPE.png]]
The above image shows us how <mark style="background: #FFB86CA6;">the target folder</mark> will look like. Each of these independent folders hold <mark style="background: #FF5582A6;">.tkape</mark> files.

Now lets take an examples to see and understand a <mark style="background: #FF5582A6;">.tkape</mark> file

### The Amcache.hve
![[Amcache.tkape.png]]
The above image is how a .tkape file will look for Amcache - 

1. <mark style="background: #FF5582A6;">Description</mark> - Simply tells us that this target collects Amcache registry hive  
2. <mark style="background: #FF5582A6;">Author</mark> - Specifies the creator of the file 
3. <mark style="background: #FF5582A6;">Version</mark> - Shows version of the template 
4. <mark style="background: #FF5582A6;">ID</mark> - This is a Unique identifier which helps in unique identifying  the target file . This ID helps us in preventing duplicate targets
5. <mark style="background: #FF5582A6;">Recreate Directories</mark> -  Specifies how files are stored in the output directory. 

6. <mark style="background: #FF5582A6;">Targets</mark> - This is the part of the file where we specify on what has to be collected or not . Within targets there are multiple parameters that need t given . These parameters are - 

   - <mark style="background: #FFB86CA6;">Name</mark> - This is the name of the artifact
   - <mark style="background: #FFB86CA6;">Category</mark> - This is used for the classification of target type 
   - <mark style="background: #FFB86CA6;">Path</mark> - Location of the artifact
   - <mark style="background: #FFB86CA6;">FileMask</mark> - Specifies the extension/pattern for which the file will be saved  . Ex - If the FileMask is "*.pf"* then only those files would we saved that has .pf in the ending .  
   -  <mark style="background: #FFB86CA6;">-</mark>  - Specifies the start of a new rule 

Now this file wad made specifically for Amcache but we can use different targets in the same file to basically make a compound target file. A compound target file can look like this - 
![[Compound.tkape.png]]



## 3)Module Options
As we already know modules are special programs that extract important information from the collected artifacts and store them for the analysts review.

The structure of the Modules folder will look like this -

![[Module Folder KAPE.png]]
Each of these independent folders will hold <mark style="background: #FF5582A6;">.mkape</mark> files which basically tells KAPE on how processing has to be done on the collected artifacts . 

Now lets take an example to see and understand a <mark style="background: #FF5582A6;">.mkape</mark> file - 

### ARP Cache
![[ARP_Cache.mkape.png]]

The above image is how a .mkape file will look for the ARP Cache processing - 

1. <mark style="background: #FF5582A6;">Description</mark> - Simply tells us that this module processes the ARP Cache artifacts  
2. <mark style="background: #FF5582A6;">Author</mark> - Specifies the creator of the file 
3. <mark style="background: #FF5582A6;">Version</mark> - Shows version of the template 
4. <mark style="background: #FF5582A6;">ID</mark> - This is a Unique identifier which helps in unique identifying the module file . This ID helps us in preventing duplicate modules
5. <mark style="background: #FF5582A6;">Export Format</mark> -  Specifies the format of the output file. 

6. <mark style="background: #FF5582A6;">Processors</mark> - This is the part of the file where we specify which tool or commands KAPE should use . Within processors there are multiple parameters that need to be given . These parameters are - 

   - <mark style="background: #FFB86CA6;">Executable</mark> - Specifies the program that has to be run
   - <mark style="background: #FFB86CA6;">Command Line</mark> - Specifies the argument that will be passed 
   - <mark style="background: #FFB86CA6;">Export Format</mark> - Specifies the format of the output
   - <mark style="background: #FFB86CA6;">Export File</mark> - Saves the output to the specified file 

  In the modules folder we also have a bin directory which has quite some importance. 
  The bin directory holds specialized tools/executables that are not part of the Windows OS as default. 
  ![[Bin_Executables KAPE.png]]
  The above image shows us a list of Eric Zimmerman's specialized tools that helps us in conducting forensics investigation on a windows machine. As these tools are not present on every Windows system by default,these executables are hence saved in the bin directory. 


As the tool has both GUI and CLI interface for the user to use . Most of the things can be learnt by simply tweaking with the tool itself.