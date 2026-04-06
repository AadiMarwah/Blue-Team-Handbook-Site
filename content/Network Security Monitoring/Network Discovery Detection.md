#Network_Security 
Whenever an attacker wants to compromise a network,the first step he/she takes is to understand how the network is operating. The attacker starts by gathering information about the different services available on the network,IP addresses,Open ports,etc. 

---

## What is Network Discovery?

Network Discovery is the first step an attacker takes when he/she wants to compromise a network . Network Discovery allows the attacker to gather crucial information like -

1. The assets that are publicly available on the internet(Attack Surface)

2. IP addresses and Open Ports of the asset,services that are running on the networks asset

3. Any vulnerability that can be exploited.
   <mark style="background: #FFB86CA6;">Example</mark> - If a older version of a service is being used with a vulnerability.

## External Scanning VS Internal Scanning 

<mark style="background: #FF5582A6;">External Scanning</mark> (Low Severity) - External Scanning is when the attacker  has an IP address outside the network as source address but the destination IP address(that is being scanned) is a part of the organization private IP pool. This type of scanning indicates that the attacker is still on the reconnaissance phase of the MITRE ATTACK Framework and has no foothold on the network yet. 

<mark style="background: #FF5582A6;">Internal Scanning</mark> (High Severity) - Internal scanning is when the attacker has both the source and destination IP address belonging to the organizations private IP pool. This type of scanning indicates that the attacker is on the Discovery phase of the MITRE ATTACK Framework and has foothold on the network which he/she can use to move laterally .

Once the attacker finds out which hosts are available on the network. He/She moves on to finding out the open ports on these hosts,This process is called as <mark style="background: #FFB86CA6;">Port Scanning</mark> and has two different approaches the attacker can follow.

### Horizontal Scanning VS Vertical Scanning 

<mark style="background: #FF5582A6;">Horizontal Scanning</mark> (Ports remain same but Host changes)- Horizontal scanning is when the attacker scans the same port across different hosts of the network. This type of scanning is done when the attackers want to compromise a specific port or a specif service that is running.

<mark style="background: #FF5582A6;">Vertical Scanning</mark> (Port Changes but Host remains same) - Vertical Scanning is where the attacker scans multiple ports of the same host. This type of scanning is performed when the attacker want to compromise the system(because of it being a high valued asset).
