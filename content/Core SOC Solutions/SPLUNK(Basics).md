#SOC_Solution 
[[Introduction To SIEM(Security Information & Event management)]]
Splunk is one of the leading SIEM solution that exists in the market. Almost all organizations use SPLUNK in their SOC teams for log analysis.

---

# What is SPLUNK and its components?

SPLUNK is a SIEM software that most organizations use nowadays to perform the task of log analysis . SPLUNK consists of 3 components , these components are -

1. <mark style="background: #FF5582A6;">SPLUNK Forwarder</mark> - SPLUNK Forwarder is a lightweight agent ,whose job is to record activities and events that occur on an endpoint and send it to the SPLUNK instance. The logs that are collected by the forwarder are sent to the <mark style="background: #FFB86CA6;">SPLUNK Indexer</mark>

2. <mark style="background: #FF5582A6;">SPLUNK Indexer</mark> - The role of the SPLUNK indexer is to process the logs it has received from the forwarder. The indexer parses and normalizes the logs,storing the data into field-value pairs . The indexer also categorizes the data so that easy search and analysis can be performed by the analyst.

3. <mark style="background: #FF5582A6;">Search Head</mark> - Using the search head the analyst can query the indexed logs and retrieve the desired data. Searching is done using <mark style="background: #FFB86CA6;">SPL(Search Processing Language)</mark> .