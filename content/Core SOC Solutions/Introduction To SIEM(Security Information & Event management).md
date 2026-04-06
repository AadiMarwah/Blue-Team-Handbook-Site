#SOC_Solution 
SIEM is a centralised solution for looking at logs collected from different sources/endpoints. 

---

When it comes to being a SOC analyst , our job is to make sure that the organisations stay safe from attacks. A big part of doing this involves investigating and analyzing logs from multiple sources.

There are two types of logs sources that exist which are -

1. <mark style="background: #FF5582A6;">Host-Centric Log Source</mark> - These log's are related to an endpoint(Windows,Linux,etc) and it's activities. These logs provides information for the events that occur on a system like user accessing a file,powershell execution,user logging-in etc.

2. <mark style="background: #FF5582A6;">Network-Centric Log Source</mark> - These log sources tell about the events that occur or are related to the network.Devices that generate such logs are firewalls,IDS,routers,etc. These log sources record activities like - User sharing file over the network, ssh connection , file being accesses via FTP protocol.


# Need of SIEM?
SIEM solutions are extremely essential for the efficient working of a SOC team because without this there is no chance that the analyst find's anything. 

SIEM solutions solve a lot of problems which are as follows -

- <mark style="background: #FF5582A6;">No Centralization</mark> - Without an SIEM solution there will be no centralisation of logs , making it extremely difficult for the analyst to analyze the logs , as he/she would have to go from device to device.

- <mark style="background: #FF5582A6;">No Correlations</mark> - If we look at activities individually that occur on a system then most of them will look normal. However if we correlate them and analyze them as a whole then there is a chance of finding a malicious activity .  

- <mark style="background: #FF5582A6;">No Standardization</mark> - Different log sources provide logs with different formats . If an analyst has to study these logs then he/she would have to know about all the different formats. An SIEM solves this problem by standardizing the logs into one consistent format,making the job of the analyst easy. Standardization of logs into one consistent format is know as the process of <mark style="background: #FFB86CA6;">Normalization</mark>.

