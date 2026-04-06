#Network_Analysis
Network Traffic Analysis is the process of capturing a networks traffic,inspecting it and finally doing analysis on it to draw conclusion(whether an incident is normal or suspicious). 

---

## Why Network Traffic Analysis is important ?
There are two ways we can look at why network traffic analysis is important . 

### From Network Administrative View
1. To see the performance of a network 
2. Check for any abnormalities in the network . Ex- Sudden performance Peaks, slow network

### From a SOC Analyst Point of View
1. To see any suspicious activity caused by a threat actor on the network if he/she is on the network
2. To recreate events during incident response phase 


## Network Flow in OSI Model as well as the TCP/IP Model

### 1)OSI MODEL
The OSI Model is not actually a framework that used in our day to day life.Rather it is more of a concept which explains how the flow of data occurs through different layers.

The OSI Model is made of 7 layers where each layer has its own purpose. .The layers are as follows-

1. <mark style="background: #FF5582A6;">Physical Layer</mark> - This is the first layer , its responsibility is the transmission of the bits through the physical medium . This layer includes hardware that is used in our day to day networking like LAN Cables,Connectors etc.

2. <mark style="background: #FF5582A6;">Data Link Layer</mark> - The Data Link Layer's is the second layer which is responsible for <mark style="background: #FFB86CA6;">Flow control</mark>(Making sure that a fast sender is not overwhelming a slow receiver),<mark style="background: #FFB86CA6;">Error control</mark>(If Frames are dropped or damaged during transmission then to re-transmit the frames to the receiver) as well as <mark style="background: #FFB86CA6;">Framing</mark>(Process where bits are grouped together to form frames) . The Data Link Layer  has also the job of <mark style="background: #FFB86CA6;">Physical Addressing</mark>(Mapping the IP Address with the MAC address)

3. <mark style="background: #FF5582A6;">Network Layer</mark> - The Network Layer is the third layer in the OSI Model whose main job is to decide the optimal route for sending the information from the sender to the receiver . The Networking layer also is responsible for <mark style="background: #FFB86CA6;">Logical Addressing(i.e IP Addressing) and Fragmentation</mark>(When the size of the data to be sent is greater than the MTU or Maximum Transmission unit.Then data is broken down into multiple fragments.These fragments are then send to the destination, where reassembling happens based on the fragment offset value). 

4. <mark style="background: #FF5582A6;">Transport Layer</mark> - The Transport Layer is one of the most crucial layers in the OSI Model who performs <mark style="background: #FFB86CA6;">Port Addressing</mark>. Two protocols belong to this layer using which delivery takes place . The two protocols are - <mark style="background: #FFB86CA6;">TCP(Connection Oriented) and UDP(Connection Less)</mark>. The Transport layer also has the responsibility of <mark style="background: #FFB86CA6;">segmentation and reassembly of the different segments at the destination</mark>.

5. <mark style="background: #FF5582A6;">Session Layer</mark> - The Session Layer is the fifth layer of the OSI Model whose responsibility is to maintain a proper <mark style="background: #FFB86CA6;">connection between the sender and the receiver while data is being transmitted</mark> . If for any reason the connection is broken during transmission, then the session layer has checkpoints in it which ensure that data transmission begins from where it had stopped. The Session layer also manages the <mark style="background: #FFB86CA6;">termination of connection  between the sender and the receiver</mark> after full data transfer has occurred.

6. <mark style="background: #FF5582A6;">Presentation Layer</mark> - This layer is responsible for standardizing the data that is received. Like converting text written in one format to ASCII.

7. <mark style="background: #FF5582A6;">Application Layer</mark> - This is the topmost layer of the OSI Model . This is the layer with which the user interacts with. <mark style="background: #FFB86CA6;">Example - HTTP,DNS</mark>

### 2)TCP/IP Model
The TCP/IP model is what we actually use in our day-to-day life for data transmission. It has only four layers because some layers of the OSI model are combined together. The diagram below shows the TCP/IP model in comparison with the OSI model.
![[OSI VS TCP.png]]

On our left we have the OSI Model and on the right we have the TCP/IP Model. From the image we can see that the data link layer and the physical layer when combined makes the Link Layer and similarly the top 3 layers of the OSI model make the Application layer .


## Network Traffic Sources and Flows
A corporate network typically has some predetermined network flows and sources about whom we should have good knowledge when it comes to traffic analysis.
### 1)Sources
These are the devices that are responsible for making traffic on a network. There are two categories under which we can classify different devices-

1. <mark style="background: #FF5582A6;">Intermediary Sources</mark> - These are devices through which traffic mostly passes. While they generate some traffic, it is significantly lower than what endpoint devices generate. Under this category, we can find firewalls, switches, web proxies, IDS, IPS, routers, access points, wireless LAN controllers, and many more. Infrastructure of the Internet Service Provider we use is also included in this category. The traffic that originates from these devices comes from services like <mark style="background: #FFB86CA6;">routing protocols</mark> (EIGRP, OSPF, BGP), <mark style="background: #FFB86CA6;">management protocols</mark> (SNMP, PING), <mark style="background: #FFB86CA6;">logging protocols</mark> (SYSLOG), and other supporting protocols (ARP, STP, DHCP).

2. <mark style="background: #FF5582A6;">Endpoint Sources</mark> - These are devices where traffic originates and ends. Endpoint devices take the bulk of the network bandwidth. Devices that fall under this category are servers, hosts, IoT devices, printers, virtual machines, cloud resources, mobile phones, tablets, and many more.

### 2)Flows
Flows represent how data travels through our network. The flow of a network is generally determined by the services that are available on the network.Two types of flows exist . They are as follows -

1. <mark style="background: #FF5582A6;">North-South Traffic</mark> - North–South traffic refers to data that crosses the perimeter of an internal network and interacts with elements outside the network. The network perimeter is usually defined by components such as a firewall, gateway, or edge router. A North–South connection has two associated directions: <mark style="background: #BBFABBA6;">egress</mark>, <mark style="background: #FFB86CA6;">which refers to traffic leaving the network</mark>, and <mark style="background: #BBFABBA6;">ingress</mark>, <mark style="background: #FFB86CA6;">which refers to traffic entering the network</mark>. This type of traffic is monitored closely because data leaving a secure internal network interacts with the internet or external networks, where it may be exposed to malicious activity. For this reason, firewalls are placed at the network perimeter to inspect and filter suspicious traffic.

2. <mark style="background: #FF5582A6;">East-West Traffic</mark> - East–West traffic refers to data that flows within an internal network and does not cross the network perimeter. This type of traffic occurs between systems, servers, and services inside the same network. Since East–West traffic remains internal, it is often monitored less compared to North–South traffic. However, monitoring this traffic is also important because if an attacker compromises one system, they can use East–West traffic to move laterally within the network and access other resources.Hence,tracking East–West traffic helps in detecting internal threats and suspicious activity within the network.


**<mark style="background: #FF5582A6;">NOTE</mark> We can record a networks traffic by using many different ways such as <mark style="background: #FFB86CA6;">logging</mark> or <mark style="background: #FFB86CA6;">port forwarding</mark>(Copying the traffic received on a port of a device and sending it for analysis to another port to which our machine is connected) or by getting a <mark style="background: #FFB86CA6;">Physical Network Tap</mark>( A Physical Device which record the activity of the entire network without causing the network to slow down)**  
 
