#Digital_Forensics 

Redline is a free tool that allows users to collect and analyze artifacts from an endpoint to determine if the endpoint/system is affected with malware. 

---

Redline is a DFIR tool that allows analysts to determine if a system is affected with malware or not . This is done by retrieving artifacts such as running processes,memory dumps,registry entries,etc from the system and comparing them against IOC's (Indicators of Compromise). 

## 1) Data Collection
The process of collecting data from a host has three different options/choices . The ways are as follows - 

- <mark style="background: #FF5582A6;">Standard Collector</mark> - Here we use a script which is configured in such a manner that it captures the minimum amount of data required for analysis . This method is extremely fast and only takes a few minutes for completion. 

- <mark style="background: #FF5582A6;">Comprehensive Collector</mark> - Here we use a script which is configured in such a manner that it captures the maximum amount of data possible for the analysis . This method is slow and can take upto few hours for completion. 

- <mark style="background: #FF5582A6;">IOC Search Collector</mark> - Here we will capture data that matches our Indicator's of Compromise that we have made/created using the IOC Editor.

<mark style="background: #FFB86CA6;">NOTE</mark> - Each of these methods have their own scripts to determine what has to be captured and what has to be left. These scripts can be edited by using the "Edit your Script" button under the Review Script Configuration Section where we have 5 different section and countless options.

![[Redline Script Config.png]]

The sections and their puposes as follows -

1. <mark style="background: #FFB86CA6;">Memory</mark> - Grabs artifacts related to the memory of the system

2. <mark style="background: #FFB86CA6;">Disk</mark> - Collects data on disk partitions and Volumes

3. <mark style="background: #FFB86CA6;">System</mark> - Will provide us information related to the machine like - Operating System,User Accounts,Groups,etc.

4. <mark style="background: #FFB86CA6;">Network</mark> - Collects network information and browser history . Network Options support Windows as well as Linux .

5. <mark style="background: #FFB86CA6;">Other</mark> - Responsible for getting miscellaneous information .

After changing the scripts as per our need ,We will give a folder location/base directory where everything will be saved.

## 2) RedLine Interface

Once the analysis file has been generated we will simply load that into RedLine. Example of how it may look - 

![[Redline Results.png]]

The above image shows us the different types of analysis data.Here we will conduct our information gathering and investigation.  

