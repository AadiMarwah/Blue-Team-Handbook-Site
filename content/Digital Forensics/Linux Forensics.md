#Digital_Forensics 
[[LinuxForensicsCheatsheet.pdf]]

Windows is a widely used operating system that we see in commercial/enterprise environments but it is not the only one. We also have the Linux Operating System which plays major roles in many crucial spots,so knowing how to investigate a Linux environment is as important as investigating a windows one.

These notes are for a Ubuntu environment

---


## 1) OS and Account Information 

### A)OS Release Information
For getting the OS release information we look at the file placed at - <mark style="background: #FFB86CA6;">/etc/os-release</mark>

![[OS_release_Linux.png]]

### B)User Accounts
The user accounts that exist in a Linux system are stored in the <mark style="background: #FFB86CA6;">/etc/passwd</mark> . This file has 7 fields/parameters for easy bifurcation. To look the content of this file properly we can use the following command - <mark style="background: #FFB86CA6;">cat /etc/passwd | column -t -s :</mark>

![[User_Accounts_Linux.png]]

The image shows us the user accounts that exist and information related to them . Lets see what each column represents -

![[Linux Account Info.png]]

<mark style="background: #FFB86CA6;">Note - User created accounts will always have a UID greater than  or equal 1000 (>=1000)</mark>

### C)Group Information
Gives us information about the different groups that are present on the host . Also tells us about the different accounts belonging to different groups.

<mark style="background: #FFB86CA6;">Location - /etc/group</mark>

![[Group Information Linux.png]]
In the above image we can see that the "Ubuntu" belongs to the "adm" group which has a Group ID of 4 . The X signifies that the Ubuntu's password is stored in the <mark style="background: #FFB86CA6;">/etc/shadow</mark> location.

### D)Sudoer's List
User accounts that exist in the sudoer's list only those accounts have the capability to elevate their privileges using the sudo command. 

<mark style="background: #FFB86CA6;">Location - /etc/sudoers</mark> 

### E)Login Information
For logging information we have two sources using which we can perform analysis . These are <mark style="background: #FF5582A6;">wtmp</mark> and <mark style="background: #FF5582A6;">btmp</mark> respectively.

<mark style="background: #D2B3FFA6;">wtmp keeps historical data of logins</mark> while <mark style="background: #ABF7F7A6;">btmp keeps a record of all the failed login attempts made</mark> . 

The wtmp and btmp files are binary in nature making then not readable using the cat,less,vim commands . So in order to read these files we use the last utility.

Location - sudo last -f /var/log/btmp
Location - sudo last -f /var/log/wtmp

### F)Authentication Logs
Every user that authenticates on a Linux host is logged in the auth log. The auth log is a file placed in the location- <mark style="background: #FF5582A6;">/var/log/auth.log</mark>


## 2) System Configuration
Once we have determined the basic information about the OS and the user accounts we can deep dive into the systems configuration.


### A)Hostname
Name of the host.
<mark style="background: #FFB86CA6;">Location - /etc/hostname</mark>

### B)TimeZone
Timezone information is a significant piece of information that gives an indicator of the general location of the device or the time window it might be used in.
<mark style="background: #FFB86CA6;">Location - /etc/timezone</mark>

### C)Network Configuration 
To find about the network interfaces we can look at the <mark style="background: #FFB86CA6;">Location - /etc/network/interfaces</mark>

To find about the different IP and MAC address of the interfaces available we can use the ip command which is as follows - <mark style="background: #FFB86CA6;">ip address show</mark>

### D)Active Connections (helps in finding programs listed at specific addresses)
On a live system , knowing the active network connections is quite significant as it provides us with additional context for the investigation . We use the <mark style="background: #FFB86CA6;">netstat</mark> utility here.

Command - <mark style="background: #FFB86CA6;">netstat -natp</mark>

### E)Running Processes
If we are performing forensic analysis on a live system , it is great to know about the processes that are running . To find about the different processes we use the <mark style="background: #FFB86CA6;">ps</mark> utility

Command - <mark style="background: #FFB86CA6;">ps aux</mark> ( aux here is one of the options we are using . There are many different options as well which can be seen using the <mark style="background: #FFB86CA6;">man ps command</mark>)

### F)DNS Information 
To get DNS info we use the location - <mark style="background: #FFB86CA6;">/etc/hosts</mark> and to get information about the DNS Servers that a Linux host talks to for DNS resolution is in location - <mark style="background: #FFB86CA6;">/etc/resolve.conf</mark>

## 3) Persistence Mechanism
Persistence mechanism are ways using which programs are able to survive and stay alive even after system reboots. These mechanism are used by malware authors a lot to retain their access to a compromised machine even after many shutdowns. The different ways are - 

### A)CronJobs
These are commands that run periodically after a set amount of time . Location for getting the list of these commands as follows - 

<mark style="background: #FFB86CA6;">Location - /etc/crontab</mark>

![[Cronjobs.png]]

### B)Service Startup
Like Windows, services can be set up in Linux that will start and run in the background after every system boot. We can use the <mark style="background: #FFB86CA6;">ls</mark> utility to view the contents. 

<mark style="background: #FFB86CA6;">Location - /etc/init.d</mark>

### C).Bashrc
Whenever a bash shell is initiated/spawned the commands written in the .Bashrc file will get executed.This file can be considered as a startup list of actions to be performed. Hence it can prove to be a good place to look for persistence. 

<mark style="background: #FFB86CA6;">Location - ./.bashrc</mark>

System wide settings are stored in the following - 

1. <mark style="background: #FF5582A6;">/etc/bash.bashrc</mark>
2. <mark style="background: #FF5582A6;">/etc/profile</mark>


## 4) Evidence of Execution
One of the major reasons of why we perform forensic analysis is to determine if commands that were executed were either normal or malicious.Sources that can help us in this task -


### A)Sudo Execution History
All the commands that are run in a Linux environment using sudo is logged into the auth log.

<mark style="background: #FFB86CA6;">Location - /var/log/auth.log</mark>
<mark style="background: #FF5582A6;">Example_Command - /var/log/auth.log* | head</mark>
### B)Bash History
All the commands that are not run with sudo is logged into the bash_history file. Every user has their own bash_history file that is stored in their respective home directories . 

So when performing forensic analysis it is important to retrieve these files. 

<mark style="background: #FFB86CA6;">Location - ./.bash_history</mark>

### C)File accessed using Vim
Files opened using Vim are logged into the viminfo file. The location of this file is - <mark style="background: #FFB86CA6;">./.viminfo</mark>. 


## 5) LogFiles
One of the most important sources of information on the activity on a Linux host is the log files. These log files maintain a history of activity performed on the host and the amount of logging depends on the logging level defined on the system

### A)Syslog 
The Syslog contains messages that are recorded by the host about system activity.

<mark style="background: #FFB86CA6;">Location - /var/log/syslog</mark>
Example_Command - <mark style="background: #FF5582A6;">/var/log/syslog* | head</mark> 
### B)AuthLogs 
Same as the ones above

### C)Third-Party Logs
Contains logs for third-party applications such as webserver, database, or file share server logs

<mark style="background: #FFB86CA6;">Location -/var/log</mark>
