#Team_Internals  
A SOC team should perform at the highest possible efficiency level it can , now there are different metrics using which efficiency can be determined .

---

The SOC team is responsible for upholding the CIA triad (Maintaining Confidentiality,integrity and availability) for an organisation ,this is done by managing and triaging alerts . As a SOC L1 analyst your job is to filter out the false positives from the true positives and report the true positives to the upper management or SOC L2 analyst for in-depth analysis. This gives us 4 metrics to work with-


1. <mark style="background: #FF5582A6;">Alert Count</mark> - This is the total number of alerts received by the SOC analyst. Example - SOC analyst see 10 alerts on the portal .


2. <mark style="background: #FF5582A6;">False Positive Rate</mark> - This tells us about the level of noise in the alerts . To calculate we use (<mark style="background: #FFB86CA6;">False Positive / Total Number of Alerts</mark>). Example - out of the total 10 alerts ,7 were false positives making the noise about 70 percent which is quite high . Higher the noise , the greater the probability of missing true positives.


3. <mark style="background: #FF5582A6;">Alert Escalation Rate</mark> - This is the metric to calculate the number of alerts escalated to L2 analysts. To calculate (<mark style="background: #FFB86CA6;">Escalated Alerts/Total Number of Alerts</mark>). Example - From the 3 true positives , if only one is escalated to the L2 analyst then this is good because this indicates the level of experience of the L1 analyst 


4. <mark style="background: #FF5582A6;">Threat Detection Rate</mark> - Refers to the percentage of the threats that were successfully detected and prevented . To calculate (<mark style="background: #FFB86CA6;">Detected Threats/Total Number of Alerts</mark>). Example - If a company faced 6 attacks but only was able to detect 4 , then this is concerning because every attack missed is an opportunity for the attackers to cause harm to the organisations's infrastructure
 

---


Now we will move on to discuss about <mark style="background: #FF5582A6;">SLA or Service Level Agreement</mark>. This is a document that is signed between the internal SOC team and organisation which specifies the level of service the SOC team will be providing to the organisation at all times . Now there are many things that come in an Service Level Agreement ,this includes -

1. <mark style="background: #FF5582A6;">Mean Time to Detect(MTTD)</mark> - This tells us the average time in between the attack and the detection by the SOC analyst.Example - <mark style="background: #FFB86CA6;">5 Minutes</mark>

2. <mark style="background: #FF5582A6;">Mean Time to Acknowledge(MTTA)</mark> - This tells us the average time to it takes for the L1 analyst to start triaging for the new alert. Example - <mark style="background: #FFB86CA6;">10 Minutes</mark>

3. <mark style="background: #FF5582A6;">Mean Time to Respond (MTTR)</mark> - This tells the average time it takes for the SOC to stop the breach from spreading.Example - <mark style="background: #FFB86CA6;">60 Minutes</mark> 

![[SLA(Service Legal Agreement).png]]


