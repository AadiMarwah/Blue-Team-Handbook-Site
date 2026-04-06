#SOC_Solution
EDR is a security solution designed to monitor,detect and counter/respond to threats at the endpoint level.By endpoint we can mean many things like servers,workstation,computer.


---

In today's time data holds the utmost importance in one's life,making it more valuable than currency itself.So data security is essential for any organisation. Before COVID , employees used to go to offices making data privacy and security easy because all or most of the employees used to work on the company's secure network . Now that is not the case,most of the corporate houses allow people to work hybridly (from home as well as from office),making the task of securing data tough.


To overcome this problem we use EDR. EDR or Endpoint Detection and response is a security solution that has the capability of fighting advanced threats on an endpoint level.
There are many names that come up when discussing about EDR-

1. Cloudstrike Falcon
2. SentinelOne ActiveEDR
3. Microsoft Defender for Endpoint


<mark style="background: #D2B3FFA6;">Now let's see what are the three main features of EDR</mark> -

1. <mark style="background: #FF5582A6;">Visibility</mark> - The level of analysis that can be achieved depends upon the availability of the data. EDR provides us with in-depth information of the endpoint. This information can include - process modification,registry modification,file and folder modification , user actions etc. The data that is provided to the analyst using EDR is also in a very structured format making it easy for the analyst to do his/her job properly and efficiently.

2. <mark style="background: #FF5582A6;">Detection</mark> - The detection feature is where we can see why organisation prefer EDR over traditional antivirus solution's . EDR uses both signature-based detection and behaviour-based detection which means it detects anomalies based on the deviation seen from the normal behaviour.

3. <mark style="background: #FF5582A6;">Response</mark> - This is an integral part of the EDR. Using the EDR central console we can provide response to the threats detected at any endpoint.


# Now how does an EDR work under the hood ?


The data we see in an EDR's centralised console is because of Agents that are installed or configured on the endpoint device's. Let's look at what an agent is and what does it do -

<mark style="background: #FF5582A6;">Agent</mark> - An agent is the eyes and ears of an EDR . It's job is to sit at the endpoint and monitor stuff . The data that is collected using these agents is then send to the EDR central console.  

![[EDR agents.png]]

At the console, correlation between the data is done by using complex logic and machine learning algorithms. 
EDR agents are also known as <mark style="background: #FFB86CA6;">sensors</mark>.


# EDR Telemetry

Telemetry refers to the data that is collected by the agents from the endpoints and is sent to the EDR console . There is a lot of detailed telemetry collected by the EDR which are as follows -

1. <mark style="background: #FF5582A6;">Process Execution & Terminations</mark> - This keeps track of all the running or idle processes which help's in finding out suspicious child-parent processes , malicious executable scripts spawning suspicious processes,etc 

2. <mark style="background: #FF5582A6;">Network Connections</mark> - This keeps track of all the network connections of an endpoint , which helps in finding out about unusual port usage , connections if made to a C2 (Command & Control) server , data exfilitration etc . 

3. <mark style="background: #FF5582A6;">Command Line Activity</mark> -  This records all the commands that are executed on the powershell or the CMD . This helps's in finding if any malicious script , suspicious commands are run on the endpoint.

4. <mark style="background: #FF5582A6;">Files and Folder Modification</mark> -  Keeps track of the changes made to a file or a folder..

5. <mark style="background: #FF5582A6;">Registry Modification</mark> - Registry is a very good source when it comes to finding out about the configuration of the endpoint system . Any changes made to this is logged/recorded by the EDR  


# Detection & Response

Based on the telemetry received,we have 2 things that can be done.Detection and Response  
Lets see how these work in detail 


## Detection
There are many ways using which detection can be done . The many ways are -

- <mark style="background: #FF5582A6;">Behavioural Detection</mark> -  In this type of detection the behaviour of the file is monitored for any suspicious or malicious activity . This is better because we do not make our judgement solely based on the signature of the file like a traditional Antivirus solution do.Lets take an example for better understanding - The attacker uses winword.exe process to spawn a powerlshell.exe process . Now if we would have been using a Antivirus solution then this event would not have been marked as malicious/suspicious but the EDR will mark this as suspicious activity because winword.exe spawning powershell.exe is a unusual parent-child process relation .

- <mark style="background: #FF5582A6;">Anomaly Detection</mark> - Over time,agents learn about the baseline behaviour of the endpoints . In this type of detection ,whenever there is deviation from the baseline behaviour the activity will be marked as suspicious . 

- <mark style="background: #FF5582A6;">IOC's Matching</mark> -  EDR's don't work alone, they have strong integration with threat intelligence tools that helps in detection . Lets take an example for better understanding - We download a file  with which a executable file also gets downloaded , the executable file will have a hash value associated with it . If that hash value is flagged as malicious or suspicious in the threat intelligence tool then the EDR will instantly mark the executable file . 

- <mark style="background: #FF5582A6;">MITRE ATT&CK mapping</mark> - The process or events that are flagged by the EDR are not just marked but are also mapped to the MITRE tactic's and techniques . Example - A suspicious event occurs then this will be the mapping
  <mark style="background: #FFB86CA6;">Tactic</mark> - Persistence
  <mark style="background: #FFB86CA6;">Technique</mark> - Scheduled Task

- <mark style="background: #FF5582A6;">Machine Learning Algorithms</mark> - Threat actors keep on evolving , making their techniques as sophisticated as possible so that they can evade the defences at all cost . Nowadays however EDR's come with machine learning models that are trained on very huge datasets's making the job of the attacker tough .


## Response
The next step after detection is responding . Now EDR offers both automated and manual responses that can be given. For automatic responses we can make policies that block malicious activity but manual response allows us to have a wide range of capabilities.

- <mark style="background: #FF5582A6;">Isolate the Host</mark> - If at a host any malicious activity is detected then that endpoint can be isolated from the network using an EDR. This limits/contains the affect of the malicious activity .

- <mark style="background: #FF5582A6;">Terminate Process</mark> -  Not every activity requires host isolation . Some endpoints run core services which cannot be isolated because if we isolate these endpoints, then the organisation can face more loss from this than the malicious activity itself. In these cases ,the best way to deal with the problem is by terminating the process itself .EDR provides the ability to the analyst to terminate any process at any given time.

- <mark style="background: #FF5582A6;">Quarantine</mark> - When we get a malicious file on an endpoint then  that specific file can be quarantined(moved to a location where it cannot be executed) , making it easy for the analyst to either permanently delete the file or restore it after analysing it thoroughly.

- <mark style="background: #FF5582A6;">Remote Access</mark> -  An EDR provides the analyst to get remote access to any endpoint on the network and see what is happening on that specific system . The ability of having remote access is great because this allows the analyst to collect specific data from specific devices and also allows the analyst to have a closer look .






 




