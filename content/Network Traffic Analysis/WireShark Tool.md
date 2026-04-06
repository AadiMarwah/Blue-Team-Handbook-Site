#Network_Analysis 
#Network_Security 
Wireshark is one of the most widely used open source tools for packet capture and analysis.As Wireshark is a tool with a GUI or a Graphical User Interface,most of the things we do can be found out by simply tinkering with the tool itself. In this room our focus will be on how to detect the different types of attacks that take place on a network.



---

## A)NMAP Scans
Nmap is an industry standard tool that is used for mapping networks, identifying live hosts, finding out the different services that run on a system by sending packets and analyzing the responses . 

Nmap is a tool that is widely used by attackers/threat actors during the reconnaissance phase.

The most common Nmap Scans with their working and filters as follows



### 1)TCP Connect Scan
The TCP Connect Scan relies on the <mark style="background: #FFB86CA6;">Three Way Handshake Protocol</mark> to check if a port is open or not .This type of scan is used by <mark style="background: #FFB86CA6;">non-privileged users</mark> and generally has the window size <mark style="background: #FFB86CA6;">greater than 1024 bytes</mark>

The three way handshake protocol is a way using which a connection-oriented channel can be made between two devices(sender and receiver). Following is an image explaining the three way handshake protocol-
![[3-Way Handshake.png]]
The above image represents the three way handshake protocol that is used to establish a connection.Lets see what each packet does-

1. <mark style="background: #FF5582A6;">SYN Packet</mark> - The SYN packet sent is a way to tell the receiver that the sender wants to establish a connection

2. <mark style="background: #FF5582A6;">SYN+ACK Packet</mark> - The SYN+ACK Packet is sent by the receiver to the sender to tell that it acknowledged the attempt made by the sender to establish the connection and also itself is ready

3. <mark style="background: #FF5582A6;">ACK Packet</mark> - The sender acknowledged the receivers response and establish a connection.

Similar to this,there is also a connection termination process which as follows - 
![[Termination_of_Connection.png]]

The above image tells us about the procedure that is followed when a connection has to be terminated(after all the data to be sent is completed). Lets see what each packet does -

1. <mark style="background: #FF5582A6;">FIN Packet</mark> - This packet initializes the connection termination process. This packet tells the receiver that the sender has no more data to send and wants to end the connection

2. <mark style="background: #FF5582A6;">ACK Packet</mark> - The ACK packet sent back represents that the receiver acknowledges the fact that the sender want to end the connection.At this point of time the sender cannot send any data but the receiver still has the power.

3. <mark style="background: #FF5582A6;">FIN Packet</mark> - The FIN Packet sent by the receiver back to the sender says that the receiver also has no more data to send.

4. <mark style="background: #FF5582A6;">ACK Packet</mark> - The sender acknowledged that the receiver has no more data to send and want to terminate the connection.

| Open TCP Port      | Closed TCP Port    |
| ------------------ | ------------------ |
| SYN Packet -->     | SYN Packet -->     |
| SYN+ACK Packet <-- | RST+ACK Packet <-- |
| ACK -->            | No Packet Sent     |
| RST+ACK -->        | No Packet Sent     |

<mark style="background: #D2B3FFA6;">tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.window_size > 1024</mark>
This here is a basic filter that helps in finding TCP Connect Scan

### 2)SYN Scan
The SYN Scan doesn't rely on the three way handshake protocol to determine if a port is open or not . This type of scan is used by <mark style="background: #FFB86CA6;">privileged users</mark> and generally has the <mark style="background: #FFB86CA6;">windows size less that or equal to 1024 bytes</mark>

| Open TCP Port | Closed TCP Port |
| ------------- | --------------- |
| SYN-->        | SYN-->          |
| SYN+ACK <--   | RST+ACK <--     |
| RST-->        |                 |

<mark style="background: #D2B3FFA6;">tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.windows_size -le 1024 </mark>
This here is a basic filter that helps in finding SYN Scan

### 3)UDP Scan
The UDP Scan detects if a port is open or not by checking the response it gets when a packet is sent to a specific port . If the port is open then we will get no response but if it is closed then we will get an ICMP error message.

| Open UDP Port  | Closed UDP Port                           |
| -------------- | ----------------------------------------- |
| UDP Packet --> | UDP Packet -->                            |
|                | ICMP Error message (Port Unreachable) <-- |

<mark style="background: #D2B3FFA6;">icmp.type == 3 and icmp.code == 3</mark>
Filter to see UDP Scan


## B)ARP Poisoning/Spoofing(Man-In-The-Middle)
ARP or Address Resolution Protocol is what allows communication between two devices that are connected on the same subnet/internal network by mapping their IP addresses with their MAC address.Lets take an example for better understand for both normal working and ARP Poisoning - Lets say that we have two computers connected on the same Network with the following MAC addresses and IP address-

| Computer A              | Computer B              |
| ----------------------- | ----------------------- |
| IP - 192.168.1.10       | IP - 192.168.1.20       |
| MAC - AA:AA:AA:AA:AA:AA | MAC - BB:BB:BB:BB:BB:BB |
and Computer A wants share some data with Computer B . As Computer A already knows the IP Address of B(Network Layer), the only thing remaining here is the MAC address.Lets see how the ARP Protocol will be used here -

1. <mark style="background: #FF5582A6;">ARP Request</mark> - Computer A will send a <mark style="background: #FFB86CA6;">broadcast message</mark> on the entire network (all devices connected to the same internal network will receive the request) asking ,the device with the IP address of 192.168.1.20 to tell its MAC address at 192.168.1.10.Ex - <mark style="background: #ADCCFFA6;">Who has 192.168.1.20 ? Tell 192.168.1.10</mark>

2. <mark style="background: #FF5582A6;">ARP Reply</mark> - Computer B will send a unicast message back to A telling its MAC address.Ex - <mark style="background: #ADCCFFA6;">192.168.1.20 is at BB:BB:BB:BB:BB:BB</mark>

Now,The ARP Cache for computer A and B  will look like - 

| ARP Cache - A                    | ARP Cache - B                    |
| -------------------------------- | -------------------------------- |
| 192.168.1.20 - BB:BB:BB:BB:BB:BB | 192.168.1.10 - AA:AA:AA:AA:AA:AA |

This is the general working of ARP Protocol but the problem with this protocol is that it has no authentication function, meaning that forged ARP Replies can be sent even if there is no ARP Request. 

In ARP Spoofing the attacker poisons the ARP Cache of devices connected to the network by sending fake/forged ARP Replies.Lets take the above example itself to understand how the ARP Cache will be manipulated -

| Attackers Computer      |
| ----------------------- |
| IP - 192.168.1.40       |
| MAC - CC:CC:CC:CC:CC:CC |
1. <mark style="background: #FF5582A6;">For Computer A</mark> - The attacker will send a forged ARP reply to Computer A,Ex - <mark style="background: #ADCCFFA6;">192.168.1.20 is at CC:CC:CC:CC:CC:CC</mark>,causing the ARP Cache of A to become- 

| ARP Cache - A                    |
| -------------------------------- |
| 192.168.1.20 - CC:CC:CC:CC:CC:CC |

2. <mark style="background: #FF5582A6;">For Computer B</mark> - The attacker will also send a forged ARP Reply like the case above to Computer B,Ex -  <mark style="background: #ADCCFFA6;">192.168.1.10 is at CC:CC:CC:CC:CC:CC </mark>,causing the ARP Cache of B to become -

| ARP Cache - B                    |
| -------------------------------- |
| 192.168.1.10 - CC:CC:CC:CC:CC:CC |

Now,all the communication that is done between A and B will go through the attacker.Hence a Man-In-The-Middle

Lets look at some wireshark filter that will help us -

| ARP           | WireShark Filters                                                     | Explanation                                                                                                         |
| ------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| ARP Request   | arp.opcode == 1                                                       | Explained Above                                                                                                     |
| ARP Reply     | arp.opcode == 2                                                       | Explained Above                                                                                                     |
| ARP Scanning  | arp.dst.hw_mac == 00:00:00:00:00:00                                   | In ARP the Destination Hardware's MAC address is unknown so it is set to 00:00:00:00:00:00 for the time being       |
| ARP Poisoning | arp.duplicate-address-detected or arp.duplicate-address-frame         | This filter signifies that two or more different devices with different MAC address own the same IP address         |
| ARP Flooding  | (arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address) | Checks for ARP Requests made by the specified MAC address , if number is extremely high then we can assume flooding |

## C)Identifying Hosts (DHCP,Kerberos,NetBIOS)
When investigating malware it is important to look beyond the obvious parameters like <mark style="background: #FFB86CA6;">IP Addresses</mark>(Because IP address can change) and <mark style="background: #FFB86CA6;">MAC Addresses</mark>(Only tells about the device,not about the person using the system).As a security analyst our main goal is to find the person/user who caused the malicious activity . This can only be achieved when we look at different parameters like which user was logged int to the machine,which users were involved in the event,how many of the systems were affected . This approach will allow us to start our investigation at an appropriate point . The three different ways using which we can identify hosts are as follows - 

### 1)DHCP(Dynamic Host Configuration Protocol)
DHCP or Dynamic Host configuration protocol is a protocol using which devices can get IP addresses assigned to them automatically for a specific time period.
The way using which devices get IP addresses assigned to them as follows -
![[DHCP Connection.png]]
The above image tells us the process of how a device gets an IP address alloted to it -

1. <mark style="background: #FF5582A6;">DHCP Discover</mark> - DHCP Discover is a broadcast message sent by the Device on the network to basically see if there is a DHCP Server on the network who can provide the device with an IP Configuration 

2. <mark style="background: #FF5582A6;">DHCP Offer</mark> - DHCP Offer is a response given by the DHCP Server telling that it exists on the network and can provide an IP address(It also includes information about the subnet of the IP address,default gateway,DNS,lease time,etc)to the device.

3. <mark style="background: #FF5582A6;">DHCP Request</mark> - Device accepts the offered IP address.This is a broadcast message sent by the device, so that if multiple DHCP Servers exist on the network then all of them know which configuration is being used.

4. <mark style="background: #FF5582A6;">DHCP ACK</mark> - The DHCP Server Acknowleges the fact that the device is using the offered IP address.

Hence the device gets an IP address on the network.

NOTE -When analyzing DHCP traffic , we must first identify the type of DHCP message, and then examine specific DHCP options to extract useful information about hosts and events.

Lets see some wireshark filters for DHCP messages and the respective option the message provide with  -

| DHCP Message | Filters               |
| ------------ | --------------------- |
| DHCP Request | dhcp.option.dhcp == 3 |
| DHCP ACK     | dhcp.option.dhcp == 5 |
| DHCP NAK     | dhcp.option.dhcp == 6 |
#### # FOR DHCP Request

| DHCP Option                         | Filter                            |
| ----------------------------------- | --------------------------------- |
| Option 12 - Hostname                | dhcp.option.hostname              |
| Option 50 - Requested IP Address    | dhcp.option.requested_ip_address  |
| Option 51 - Requested IP Lease Time | dhcp.option.ip_address_lease_time |
| Option 61 - Clients MAC Address     | dhcp.option.client_id             |

#### # FOR DHCP ACK 

| DHCP Options                       | Filter                  |
| ---------------------------------- | ----------------------- |
| Option 15 - Domain Name            | dhcp.option.domain_name |
| Option 51 - Assigned IP Lease Time |                         |

#### # FOR DHCP NAK

| DHCP Options                  |
| ----------------------------- |
| Option 56 - Rejection Details |


### 2)NetBIOS Analysis
NetBIOS(Network Basic Input/Output System) is a way using which computers on the same network are able identify other devices by their names and the services they provide. The NetBIOS works mainly using the computer's name rather than IP Addresses.

| NBNS                                                                 | Filters for Grabbing Low Hanging Fruit |
| -------------------------------------------------------------------- | -------------------------------------- |
| Global Filter/Search                                                 | nbns                                   |
| For Specialized(Could contain parameters like name,TTL,IP addresses) | nbns.name contains "Keyword"           |

### 3)Kerberos Analysis
Kerberos is an authentication method where devices on a network are able to authenticate themselves to other systems without sharing there usernames or passwords over the environment. Kerberos is default authentication service for Windows.
The core working of kerberos depends upon KDC or a Key Distribution Centre , which is split into two halves(<mark style="background: #FFB86CA6;">Authentication Server and Ticket Granting Server</mark>).

| Kerberos                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Filters                                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Global Search                                                                                                                                                                                                                                                                                                                                                                                                                                              | kerberos                                                                                                                  |
| **CNameString**: The Username (It is the name of the user who is requesting for authentication).<br><br>This filter is important because machines also authenticate using Kerberos, which can cause confusion between user and machine authentication. By filtering for names that contain a "$", we can reliably identify and exclude machine accounts, since in Active Directory, computer accounts follow a naming convention where names ends with a $ | 1)kerberos.CNameString contains "keyword" <br><br><br><br>2)kerberos.CNameString and !(kerberos.CNameString contains "$") |
| **pvno**:protocol version(Tells which version of kerberos is being used)                                                                                                                                                                                                                                                                                                                                                                                   | Example - kerberos.pvno == 5                                                                                              |
| **realm**:This contains the domain of the organization who is authenticating the users systems by using KDC's . Ex - Domain of the organization providing us with the authentication service is ANON.ORG,who owns 10 KDC's. So in realm ANON.ORG will be present                                                                                                                                                                                           | <br><br><br><br>Example - kerberos.realm contains ".org"                                                                  |
| **sname**:Contains information about the service being used by a specific system on a specific domain.Example -<br>HTTP/HOST-1@ANON[.]COM<br>In the above example - HTTP is the service,HOST-1 is the machines name and ANON.COM is the domain                                                                                                                                                                                                             | <br><br><br>Example - kerberos.snameString == "krbtg"                                                                     |

## D)Tunnelling Traffic:DNS and ICMP
Traffic tunnelling also know as Port Forwarding is a mechanism using which devices on different networks can communicate with each other . Lets take an example for better understanding-
Following is the data of the devices of each network and the intermediary devices-

| Network A                        | Network B                        |
| -------------------------------- | -------------------------------- |
| Computer-A1 - 192.168.1.10       | Computer-B1- 192.168.2.10        |
| Computer-A2 - 192.168.1.20       | Computer-B2- 192.168.2.20        |
| Router-A (Public) - 1.1.1.1      | Router-B (Public) - 2.2.2.2      |
| Router-A (Private) - 10.10.10.10 | Router-B (Private) - 20.20.20.20 |
1)If Computer-A1 want to send data to Computer-B2 then the packet holding the data will have the destination address as 2.2.2.2 as Computer-A1 doesn't know the internal structure of Network B . 

2)The data will first travel to Router A , where using routing protocol data will be forwarded to Router B . 

3)Once Router B receives this data it will look for NAT Rules or Network Address Translation Rule . This rule can look something like this - **If data received at 2.2.2.2:35(35 is the port here) then send the data over to 192.168.2.20:45(45 is the port here)**.

4)As the NAT rules match here,Router B updates the packet's destination address,port address to 192.168.2.20 and 45 respectively.

5)After updation has been made,the packet is send to Computer B2

This entire process is called as Port Forwarding 

### 1)ICMP Analysis
Internet Control Message Protocol (ICMP) is designed for diagnosing and reporting network communication issues. It is highly used in error reporting and testing. As it is a trusted network layer protocol, sometimes it is used for denial of service (DoS) attacks; also, adversaries use it in data exfiltration and C2 tunnelling activities.

**ICMP analysis in a nutshell:**

Usually, ICMP tunnelling attacks are anomalies appearing/starting after a malware execution or vulnerability exploitation. As the ICMP packets can transfer an additional data payload, adversaries use this section to exfiltrate data.However its disadvantage is that most enterprise networks block custom packets or require administrator privileges to create custom ICMP packets.

If there is a large volume of ICMP traffic or if there are anomalous packet sizes then we can consider a possibility of an ICMP Tunnelling attack. Lets look at a filter to detect this type of attack - 

| ICMP            | Filter                 |
| --------------- | ---------------------- |
| Global Search   | icmp                   |
| ICMP Tunnelling | icmp and data.len > 64 |

### 2)DNS Analysis
DNS or Domain Name System is a system that is used to translate/convert domain names into IP addresses.Example - It easier to remember YouTube.com instead of its actual IP address like "176.89.97.56", but machines cannot interpret this,they can only communicate using IP addresses.Hence DNS acts like a mapping system that connects these easy-to-remember domain names with their corresponding IP addresses, which allows users to access websites without remembering complex numerical addresses

#### Normal Workflow of DNS 
When we type youtube.com in a browser, the browser first asks the Operating System whether the IP address for youtube.com is already stored in its cache. If the IP address is found, the browser directly connects to that address.

If the IP address is not present in the cache, then the  Operating System sends a DNS query requesting the **A-type record (IP address)** for youtube.com. The DNS server responds with the IP address, and the browser then establishes a connection to the website.

#### Working of DNS Tunnelling
DNS Tunneling is a technique through which attackers can exfiltrate data out of a network and run arbitrary commands remotely without raising suspicion.This is done by abusing the DNS protocol.Lets take an example -

1) Lets say that a system gets infected with a malware called STUXNET. Now This malware will work silently behind the scenes collecting data of the system.

2) In order to receive the collected data and to run the commands remotely the attacker would need to configure a communication channel . Here the attacker will basically use a domain he/she own's (Ex-Malicious.org) that will be configured on a DNS Server to which the attacker has full access and full control.

3) The collected data from STUXNET will then be encoded into a different format(like base64) which will then be attached along the domain name so it looks like a subdomain.
   Example-
   <mark style="background: #FF5582A6;">Host_Name</mark>:Anon_Computer_System
   <mark style="background: #FF5582A6;">base64_encoded_form</mark>: QW5vbl9Db21wdXRlcl9TeXN0ZW0=(Host Name Encoded)
   <mark style="background: #FF5582A6;">DNS_Query</mark>: QW5vbl9Db21wdXRlcl9TeXN0ZW0=.Malicious.org

4) The DNS Query will be sent to Malicious.org acting as a query to the subdomain and once the data is received,decoding will take place allowing the attacker to successfully exfiltrate data.

5) Commands are run remotely by the attacker on the infected system because of the malware's characteristic to look for DNS Responses periodically/after a certain amount of time. 
   
   If an attacker want to execute a command he can simply send a response back to a DNS query in the form of a TXT record which when parsed by the malware will cause the execution of the command.

This is how DNS Tunneling works

NOTE - A simple tell to DNS Tunneling is the fact that the query length is way to long than expected.The filters as follows -

| DNS                   | Filters               |
| --------------------- | --------------------- |
| Global Search         | dns                   |
| To detect dnscat      | dns contains "dnscat" |
| Based on Query Length | frame.len > 15        |


## E)ClearText Protocol Analysis:FTP and HTTP

### 1)FTP(File Transfer Protocol)
FTP or File Transfer Protocol is a system that is designed to transfer files with ease, so it focuses on simplicity rather than security. As a result of this, using this protocol in an unsecured environments could create security issues like:

- Man in the Middle attacks
- Credential stealing and unauthorised access
- Phishing
- Malware planting
- Data exfiltration

Lets look at different filters for grabbing different parameters - 

#### # For Information exchanged using Requests and Responses

| FTP                    | Filters                  |
| ---------------------- | ------------------------ |
| 211 - System Status    | ftp.response.code == 211 |
| 212 - Directory Status | ftp.response.code == 212 |
| 213 - File Status      | ftp.response.code == 213 |

#### # For Connecting Messages

| FTP                         | Filters                  |
| --------------------------- | ------------------------ |
| 220 - Service Ready         | ftp.response.code == 220 |
| 227 - Entering Passive Mode | ftp.response.code == 227 |
| 228 - Long Passive Mode     | ftp.response.code == 228 |
| 229 - Extended passive Mode | ftp.response.code == 229 |

#### # For Authentication Messages

| FTP                                                | Filters                  |
| -------------------------------------------------- | ------------------------ |
| 230 - User login                                   | ftp.response.code == 230 |
| 231 - User logout                                  | ftp.response.code == 231 |
| 331 - Valid username                               | ftp.response.code == 331 |
| 430 - Invalid username or password                 | ftp.response.code == 430 |
| 530 - No login, invalid password (Login Incorrect) | ftp.response.code == 530 |

#### # Addition Filters for grabbing low hanging fruit

| FTP                             | Filters                       |
| ------------------------------- | ----------------------------- |
| USER - Username                 | ftp.request.command == "USER" |
| PASS - Password                 | ftp.request.command == "PASS" |
| LIST - List                     |                               |
| CWD - Current Working Directory |                               |

#### # Advanced Filters

| FTP                                                                   | Filters                                                               |
| --------------------------------------------------------------------- | --------------------------------------------------------------------- |
| BruteForce Attempt(To List Failed Login Attempts)                     | ftp.response.code == 530                                              |
| BruteForce-Signal(To list failed login attempts and target usernames) | (ftp.response.code == 530) and (ftp.response.arg contains "username") |
| Password Spray signal (List Target for a static password)             | (ftp.request.command == "PASS") and (ftp.request.arg == "Password")   |

### 2)HTTP(Hypertext Transfer Protocol)
Hypertext Transfer Protocol (HTTP) is a cleartext-based, request-response and client-server protocol. It is the standard type of network activity to request/serve web pages, and by default, it is not blocked by any network perimeter. As a result of being unencrypted and the backbone of web traffic, HTTP is one of the must-to-know protocols in traffic analysis. Following attacks could be detected with the help of HTTP analysis:

- Phishing pages
- Web attacks
- Data exfiltration
- Command and control traffic (C2)

Lets look at some values that are essential when it comes to HTTP Responses(To query them we use the filter of <mark style="background: #FF5582A6;">http.response.code ==</mark> ) (We already know HTTP Requests like GET and POST)-

- **200 OK:** Request successful.
- **301 Moved Permanently:** Resource is moved to a new URL/path (permanently).
- **302 Moved Temporarily:** Resource is moved to a new URL/path (temporarily).
- **400 Bad Request:** Server didn't understand the request.
- **401 Unauthorised:** URL needs authorisation (login, etc.).
- **403 Forbidden:** No access to the requested URL. 
- **404 Not Found:** Server can't find the requested URL.
- **405 Method Not Allowed:** Used method is not suitable or blocked.
- **408 Request Timeout:**  Request look longer than server wait time.
- **500 Internal Server Error:** Request not completed, unexpected error.
- **503 Service Unavailable:** Request not completed, server or service is down.

Now lets look at some more parameters for analyzing the HTTP Traffic -

- **User agent:** Browser and operating system identification to a web server application.
- **Request URI:** Points the requested resource from the server.
- **Full URI**: Complete URI information.
- **Server:** Server service name.
- **Host:** Hostname of the server
- **Connection:** Connection status.
- **Line-based text data:** Cleartext data provided by the server.
- **HTML Form URL Encoded:** Web form information.

#### UserAgent Analysis in HTTP(Browser+Anything that makes HTTP request)
UserAgent is a very crucial field that can help us in finding out the different services that are making an HTTP Request . 
UserAgents are generally confused with Browser's but it also includes HTTP Request generated because of CURL or WGET Example - 
<mark style="background: #FFB86CA6;">(http.user_agent contains "sqlmap") or (http.user_agent contains "Nmap") or (http.user_agent contains "Wfuzz") or (http.user_agent contains "Nikto")</mark>

## F)Encrypted Protocol Analysis: HTTPS
HTTPS or Hypertext Transfer Protocol Secure is basically the extended version of HTTP ,which has been made with security in mind . HTTPS uses <mark style="background: #FFB86CA6;">TLS(Transport Layer Security)</mark> protocol to encrypt communications, so it is impossible to decrypt the traffic and view the transferred data without having the encryption/decryption key pairs. As this protocol provides a good level of security for transmitting sensitive data, attackers and malicious websites also use HTTPS. Therefore, a security analyst should know how to use key files to decrypt encrypted traffic and investigate the traffic activity.  


| TLS             | Filters                 |
| --------------- | ----------------------- |
| Global Search   | tls                     |
| Client Request  | tls.handshake.type == 1 |
| Server Response | tls.handshake.type == 2 |

Similar to the TCP three-way handshake process, the TLS protocol has its handshake process. The first two steps contain "Client Hello" and "Server Hello" messages. The given filters show the initial hello packets in a capture file. These filters are helpful to spot which IP addresses are involved in the TLS handshake.

- Filter for finding Client Hello: http.request or tls.handshake.type == 1) and !(ssdp) 
- Filter for finding Server Hello: (http.request or tls.handshake.type == 2) and !(ssdp)

Whenever we want to do analysis on a encrypted traffic session ,the first step is to decrypt the traffic. This is done by using Key Log Files . A Key Log file is a text file that holds unique key pairs to decrypt the encrypted traffic.These key pairs are automatically created (per session) when a connection is established with an SSL/TLS-enabled webpage. As these processes are all accomplished in the browser, we need to configure our system and use a suitable browser (Chrome and Firefox support this) to save these values as a key log file.


NOTE 1 - A general practice that we should follow when doing traffic analysis is to firstly take an overview of the pcap file using Network Miner and then doing in-depth analysis using Wireshark.

**NOTE 2- Notes essential for Data Exfiltration.**
