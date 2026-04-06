#Network_Security 
[[Snort_Cheatsheet.pdf]]
SNORT is an open-source, rule-based Network Intrusion Detection and Prevention System (NIDS/NIPS)

---

## 1)Intrusion Detection System VS Intrusion Prevention System

### Intrusion Detection System 
IDS is a passive monitoring solution whose main job is to detect any suspicious activities that happen. For each malicious event only an alert is raised and no other action is taken. 

<mark style="background: #FFB86CA6;">Example</mark> - Lets say that an IDS detects a SQL Injection Signature. The IDS will only raise an alert here but will not take any actions to stop the attack

IDS can be categorized into two parts based on the deployment model which as follows-  

1. <mark style="background: #FF5582A6;">Host Intrusion Detection Systems</mark> - Host-based IDS solutions are installed individually on the hosts and are responsible for only detecting potential security threats associated with that particular host. They provide detailed visibility of the host’s activities. However, host intrusion detection systems can be challenging to manage in large networks as they are resource-intensive and require management on each host.

2. <mark style="background: #FF5582A6;">Network Intrusion Detection System</mark> - Network-based IDS solutions are crucial in detecting potentially malicious activities within the whole network, regardless of any specific hosts. They monitor the network traffic of all the hosts involved to detect suspicious activities. It provides a centralized view of all the detection's inside the whole network.

IDS can be categorized into three parts based on the detection model which as follows -   

1. <mark style="background: #FF5582A6;">Signature Based IDS</mark> - Each attack that happens has its own unique pattern which is called as a <mark style="background: #FFB86CA6;">Signature</mark> . These signatures are stored in the IDS database for future reference for quick detection of an attack.However this type of IDS fails when there are zero-day attack as there are no prior signature/pattern based on which the detection can be done. 

2. <mark style="background: #FF5582A6;">Anomaly Based IDS</mark> - Anomaly Based IDS are systems where the IDS firstly learns about the normal behaviour of a host or a network and then performs detection if there is any deviation .These type of IDS can detect zero-day attacks as they do not rely on signatures.

3. <mark style="background: #FF5582A6;">Hybrid IDS</mark> - A hybrid IDS combines the detection methods of signature-based IDS and anomaly-based IDS to leverage the strengths of each approach. Some known threats may already have some signatures in the IDS database; in this case, the hybrid IDS would use the detection technique of the signature-based IDS. If it encounters a new threat, it can leverage the detection method of anomaly-based IDS.
 
  
### Intrusion Prevention System 
IPS is a active monitoring solution whose main job is to perform in depth packet analysis on the traffic to see for any malicious activity. If any suspicious packets/traffic is found then the IPS will stop the traffic by terminating the session,by dropping the specific packet,etc. 

<mark style="background: #FFB86CA6;">Example</mark> - Lets say that an IPS detects a SQL Injection Signature. The IPS here will stop the packet from reaching the destination by dropping it and may also reset the TCP session.

IPS Can be categorized into 4 main types which as follows - 

1. <mark style="background: #FF5582A6;">Network Intrusion Prevention System (NIPS)</mark> - NIPS monitors the traffic flow from various areas of the network. The aim is to protect the traffic on the entire subnet. If a signature is identified, the connection is terminated.

2. <mark style="background: #FF5582A6;">Behaviour-based Intrusion Prevention System (Network Behaviour Analysis - NBA)</mark> -  Behaviour-based systems monitor the traffic flow from various areas of the network. The aim is to protect the traffic on the entire subnet. If an anomaly is identified, the connection is terminated.

3. <mark style="background: #FF5582A6;">Wireless Intrusion Prevention System (WIPS)</mark> -  WIPS monitors the traffic flow from a wireless network. Its aim is to protect wireless traffic and stop possible attacks launched from there. If a signature is identified, the connection is terminated.

4. <mark style="background: #FF5582A6;">Host-based Intrusion prevention Systems (HIPS)</mark> -  HIPS actively protects the traffic flow from a single endpoint device. The aim is to investigate the traffic on a particular device. If a signature is identified, the connection is terminated.

## 2) Snort in Sniffer Mode 
Snort in sniffer mode just reads and displays network packets without performing any analysis on them.Some parameters/flags that will help us in sniffer mode as follows -

| Flags | Meaning                                                                             |
| ----- | ---------------------------------------------------------------------------------- |
| -v    | Be Verbo                                                                            |
| -d    | Displays the packet data (Paylo                                                     |
| -X    | Displays full packet detail in HEX fo                                               |
| -e    | Display the limk layer headers (TCP/IP/UDP/                                         |
| - Is used to tell for which interface snort has to sniff the traffic (like ethernet) ernet  |

## 3) Snort in Packet Logger Mode
Snort in packet logger mode allows you to log the network traffic as a PCAP  files.Snort performs detection on the network traffic in real-time and displays the detection's as alerts on the console.Some parameters/flags that will help us in packet logger mode as follows -

| Flags                                                       | Meaning                                              |
| ----------------------------------------------------------- | ---------------------------------------------------- |
| -l                                                          | Logs the traffic at the specified location           |
| -K ASCII                                                    | Logs the packets in ASCII Format                     |
| -r (Does not work when logging is done using -K ASCII Flag) | Review the logged events. Basically reads the file   |
| -n                                                          | Specifies the number of packets to be read/displayed |

To use snort in Packet Logger Mode we  have to run the command - <mark style="background: #FF5582A6;">sudo snort -dev </mark>. Rest of the parameters can be mixed together .

We can use the -r flag to read a file while simultaneously filtering for specific strings. Example -
1. snort -r Task.log icmp
2. snort -r Task.log 'udp and port 53'
3. snort -r Task.log tcp
4. snort -r Task.log -n 5 -X

## 4) Snort in IPS and IDS 

| Flags | Meaning                                                                                                                                                                                                                                                                                                                                                                    |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -c    | Specifies the configuration file used                                                                                                                                                                                                                                                                                                                                      |
| -T    | Used to test the configuration file                                                                                                                                                                                                                                                                                                                                        |
| -N    | Disables logging                                                                                                                                                                                                                                                                                                                                                           |
| -D    | Background mode / Background activity                                                                                                                                                                                                                                                                                                                                      |
| -A    | Specifies the alert mode . There are 4 different mode <br><br>1) Full - Gives all possible information related to the alert . Like timestamp,source and destination IP <br><br>2) Console - Gives a fast and compact style alert on console<br><br>3) Cmg - Gives basic header details with payload information in hex and text format <br><br>4) None - Disables alerting |

To use snort mode in IPS we use the flags <mark style="background: #FFB86CA6;">-Q --daq afpacket</mark>.Also for the interface we will have to use the -i flag like <mark style="background: #FFB86CA6;">- i eth0:eth1</mark>

## 5) PCAP File analysis using Snort 

When using snort we are not just limited to live traffic but we can also perform analysis on captured traffic(PCAP Files).The following flags help us in this process - 

| Flags                               | Meaning                                                                       |
| ----------------------------------- | ----------------------------------------------------------------------------- |
| --pcap-single="File name" OR -r     | Specifies the packet capture file on which we have to perform analysis        |
| --pcap-list="File_Name1 File_Name2" | Specifies a list of packet capture files on which we have to perform analysis |
Example - 

<mark style="background: #FFB86CA6;">A)snort -c /etc/snort/snort.conf --pcap-single="mx-1.pcap" -A console -l . </mark>
   
This command will use the main configuration file to see if any traffic inside the pcap file is raising alerts. 

The alerts will be logged into the current directory. 

We will also be able to see the outputs/alerts in the terminal 

<mark style="background: #FFB86CA6;">B)snort -c /etc/snort/snort.conf --pcap-list="mx-1.pcap mx-2.pcap" -A console -l .</mark> 

This command will use the main configuration file to see if any traffic inside both of the pcap files is raising alerts.

The alerts will be logged into the current directory. 

We will also be able to see the outputs/alerts in the terminal 

## 6) Snort Rule Structure 

Snort rules are special queries using which we can filter specific traffic.The structure of a snort rule as follows -

![[Snort_Rule_Structure.png]]

Each rule that is made in snort has the above syntax . Let see what each parameter is and the different options in it -

#### A) Actions 
Actions specifies what has to be done when all the parameters of the rule meet . There are 4 different actions that can be performed -

1. <mark style="background: #FF5582A6;">Alert</mark> - Raises an alert and logs the packet
2. <mark style="background: #FF5582A6;">Log</mark> - Just logs the packet
3. <mark style="background: #FF5582A6;">Drop</mark> - Block and log the packet 
4. <mark style="background: #FF5582A6;">Reject</mark> - Block the packet,logs it and terminate the session
#### B) Protocol
Specifies for which protocol the rule is valid . Snort supports 3 to 4 protocols based on the version of it . The protocols are - TCP,UDP,ICMP,IP. 
#### C) Source IP
#### D) Source Port
#### E)Direction
There are only two direction when making snort rules the directions as follows -

1. -> (Unidirectional)
2. <-> (Bidirectional)
#### F) Destination IP 
#### G) Destination Port
#### H) Message 
The message field is a basic prompt and quick identifier of the rule. Once the rule is triggered, the message field will appear in the console or log. Usually, the message part is a one-liner that summarises the event.
#### I) Reference
Each rule can have additional information or a reference to explain the purpose of the rule or threat pattern. That could be a Common Vulnerabilities and Exposures (CVE) ID or external data. Having references for the rules will always help analysts during the alert and incident investigation.
#### J) SID 
Helps in identification of each rule.These are unique numeric value associated with each rule.The three different slabs are-

1. <100 - Rules with SID less than 100 are reserved rules 
2. 100-999,999 - Rules came with the build
3. >=1000000 - Rules made by the user

#### K) REV 
Tells us about the number of times alterations/revisions has been done to a rule . With each revision we increment the counter of REV by 1.

Examples of different rules that can be used as follows - 

| Rules                                                                                                            | Functioning                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| alert icmp 192.168.1.56 any <> any any  (msg: "ICMP Packet From "; sid: 100001; rev:1;)                          | This rule will create an alert for each ICMP packet originating from the 192.168.1.56 IP address                 |
| alert icmp 192.168.1.0/24 any <> any any  (msg: "ICMP Packet Found"; sid: 100001; rev:1;)                        | This rule will create an alert for each ICMP packet originating from the 192.168.1.0/24 subnet.                  |
| alert icmp [192.168.1.0/24,  10.1.1.0/24] any <> any any  (msg: "ICMP Packet Found"; sid: 100001; rev:1;)        | This rule will create an alert for each ICMP packet originating from the 192.168.1.0/24 and 10.1.1.0/24 subnets. |
| alert icmp !192.168.1.0/24 any <> any any  (msg: "ICMP Packet Found"; sid: 100001; rev:1;)                       | This rule will create an alert for each ICMP packet not originating from the 192.168.1.0/24 subnet.              |
| alert tcp any any <> any 21  (msg: "FTP Port 21 Command Activity Detected"; sid: 100001; rev:1;)                 | This rule will create an alert for each TCP packet sent to port 21.                                              |
| alert tcp any any <> any !21  (msg: "Traffic Activity Without FTP Port 21 Command Channel"; sid: 100001; rev:1;) | This rule will create an alert for each TCP packet not sent to port 21.                                          |
| alert tcp any any <> any 1:1024   (msg: "TCP 1-1024 System Port Activity"; sid: 100001; rev:1;)                  | This rule will create an alert for each TCP packet sent to ports between 1 and 1024.                             |
| alert tcp any any <> any:1024   (msg: "TCP 0-1024 System Port Activity"; sid: 100001; rev:1;)                    | This rule will create an alert for each TCP packet coming to port below 1024 (1024 is included)                  |
| alert tcp any any <> any 1025: (msg: "TCP Non-System Port Activity"; sid: 100001; rev:1;)                        | This rule will create an alert for each TCP packet sent to a source port that is higher than or equal to 1025.   |
| alert tcp any any <> any [21,23] (msg: "FTP and Telnet Port 21-23 Activity Detected"; sid: 100001; rev:1;)       | This rule will create an alert for each TCP packet sent to ports 21 and 23.                                      |

Now lets looks at some flags that will help us in payload detection -

| Flags                          | Meaning                                                               | Example                                                                                                                                                                                                                           |
| ------------------------------ | --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| content<br>(Is case sensitive) | Payload data. It matches specific payload data by ASCII, HEX, or both | ASCII MODE - alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; sid: 100001; rev:1;)<br>OR<br>HEX mode - alert tcp any any <> any 80  (msg: "GET Request Found"; content:"\|47 45 54\|"; sid: 100001; rev:1;) |
| nocase                         | Disabling case sensitivity.                                           | alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; nocase; sid: 100001; rev:1;)                                                                                                                               |

Similar to the above case there are flags that will help us in non payload detection - 

| Flags     | Meaning                                                                           | Example                                                                             |
| --------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ID        | Filtering the IP ID Field                                                         | alert tcp any any <> any any (msg: "ID TEST"; id:123456; sid: 100001; rev:1;)       |
| TCP Flags | F - FIN<br>S - SYN<br>R - RST<br>P - PSH<br>A - ACK<br>U - URG                    | alert tcp any any <> any any (msg: "FLAG TEST"; flags:S;  sid: 100001; rev:1;)      |
| dsize     | Filters the packet payload size<br><br>dsize:<100<br>dsize>:100<br>dsize:min<>max | alert ip any any <> any any (msg: "SEQ TEST"; dsize:100<>300;  sid: 100001; rev:1;) |
| SameIP    | Filtering the source and destination IP addresses for duplication.                | alert ip any any <> any any (msg: "SAME-IP TEST";  sameip; sid: 100001; rev:1;)     |
