#SOC_Solution 
[[Introduction To SIEM(Security Information & Event management)]]
ELK Stack is collection of open-source tools that help in collecting,storing,visualizing and analyzing the data.This is not a SIEM solution itself but almost works as one ,making it widely used by SOC teams.

---

Elastic Stack was originally made to store,search and analyze large amounts of data. This was used by organizations for monitoring their applications performances,  but with time and integration of new features , ELK stack became a good candidate to be used as a SIEM solution. 

There are many components to the ELK stack . These components are as follows -

1. <mark style="background: #FF5582A6;">Beats</mark> - They are host based agents also called as <mark style="background: #FFB86CA6;">Data Shippers</mark>, their job is to collect data from the endpoints and send it to either <mark style="background: #FFB86CA6;">LogStash for filtering or to ElasticSearch Directly</mark>

2. <mark style="background: #FF5582A6;">LogStash</mark> -  LogStash is a data processing engine that ingests data from different sources, applies filters on it which are made by the analyst in the configuration file and sends the filtered/processed data to ElasticSearch . There are three different part of the LogStash configuration file  -
	- <mark style="background: #FFB8EBA6;">Input</mark> - This part tells from which location the data will be ingested
	- <mark style="background: #FFB8EBA6;">Filter</mark> - Rules made by the analyst to normalize the data and filter out the unnecessary junk.
	- <mark style="background: #FFB8EBA6;">Output</mark> - Tells where the processed data has to be sent 

3. <mark style="background: #FF5582A6;">ElasticSearch</mark> - This a full text based engine that stores JSON formatted documents in it . ElasticSearch is very essential for the working of the ELK Stack because without this there will be no searching,correlation or analysis of the acquired data .

4. <mark style="background: #FF5582A6;">Kibana</mark> -  It is a web-based visualization tool that works with ElasticSearch to analyze and investigate the data. It allows user to make graphical representation based on the logs for easy and better understanding .

![[ELK_Stack.png]]

This is how the ELK stack and its component's work under the hood.
