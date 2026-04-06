#Network_Security
In today's time we use pretty complicated networks involving computers,servers,security devices,applications,etc which are not in isolation but interconnected. Ensuring a network's security always has been important because if an attackers gets access to one component of the network then he/she can get access to all the other components with time.
Here we will look at the fundamental concept that are needed  for ensuring a network's security. 

---

## What is a Network and what are the components?

A network is a collection of devices like servers,workstation,security devices,etc that are connected to each other to ensure proper communication and data flow between all the devices. The different components and their working as follows 

1. <mark style="background: #FF5582A6;">Workstation</mark> - Workstations are basically laptops or computers on which the employee performs day to day tasks. These are the most common entry points for attackers because if they are able to access a workstation,then they have an opportunity to move laterally throughout the network to gather information.

2. <mark style="background: #FF5582A6;">File Server</mark> - This server provides centralized access to shared documents.

3. <mark style="background: #FF5582A6;">Database Server</mark> - Holds critical records like Financial data,Customer List,Employee Information,etc.

4. <mark style="background: #FF5582A6;">Application Servers</mark> - These servers provide services to both the customers and employees. Example of such servers are - <mark style="background: #FFB86CA6;">Web Server</mark>(Hosts the company website),<mark style="background: #FFB86CA6;">Email Server</mark> (Manages company's email communication),<mark style="background: #FFB86CA6;">VPN Server</mark>,etc. 

5. <mark style="background: #FF5582A6;">Active Directory</mark> - This server ensures the authenticity of users on the network .It manages users, groups, computers, and their access rights. Employees use their AD credentials to log into their computers, access email, file servers, and internal applications.

6. <mark style="background: #FF5582A6;">Routers</mark> - Devices responsible for connecting different networks with each other,most importantly connecting LAN Networks to the Internet .  

7. <mark style="background: #FF5582A6;">Switches</mark> - Connects devices within the same network,ensuring seamless communication between servers,workstation,printers,etc.

8. <mark style="background: #FF5582A6;">Firewalls</mark> - It is a security gateway between the trusted internal network and the untrusted Internet. The job of a firewall is to filter out the packets coming into the network based on the rules and policies.

In a network there are two types of logs that are generated. The classification of these logs can be done as follows -

1.  <mark style="background: #FF5582A6;">Host-Centric Log Source</mark> - These log's are related to an endpoint(Windows,Linux,etc) and it's activities. These logs provides information for the events that occur on a system like user accessing a file,powershell execution,user logging-in etc.

2. <mark style="background: #FF5582A6;">Network-Centric Log Source</mark> - These log sources tell about the events that occur or are related to the network.Devices that generate such logs are firewalls,IDS,routers,etc. These log sources record activities like - User sharing file over the network, ssh connection , file being accesses via FTP protocol. 

