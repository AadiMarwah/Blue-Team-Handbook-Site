#Cyber_Frameworks
Pyramid of pain can be considered as a frameworks where we aim to move toward the top of the pyramid. The more we get to the top the more it is painful for the attacker

---


# 1) Hash Value
The lowest level of the pyramid is the Hash value . Whenever we make a file, that file has a hash value associated with it.The hash value basically help us to uniquely identify between different file  since each file has a unique hash generated because of a specific formula or a hashing algorithm . Some examples of hashing algorithms are - MD5,SHA256,SHA512 etc. Hash values are at the bottom of the pyramid because these values are just used by the analyst to gain insight or mark a malicious artifact . Hash values are not great IOC's(Indicator of Compromise) because even if we change a single byte ,the file gets a completely new hash value. Lets say we have a malware and we have blocked its execution because of the specific hash value , Now the attacker add a '#' to the file and sends it again , this time this file will bypass our security measures because of the new hash value . Hence the hash values when considering are at the bottom on the pyramid 


# 2) IP Addresses
Above the hash value we have IP addresses. An IP address is basically the address of a device when connected over the internet . This address is used to send requests and get feedback/answers for that request. Now when IP addresses are found by analyst , they are not problematic for the attacker because he can just use a new IP address to establish/re-establish the connection . In today's time attackers use the technique of <mark style="background: #FFB86CA6;">Fast Flux </mark>. This technique is used to hide malicious and phishing websites , In this technique there is a pool of infected computes/laptops the attacker has access to . Now whenever a person comes to the website he will get connected to an IP from that pool , when the next person comes he will get connected to a different IP from that pool. This will keep on happening making it very difficult for the analyst to block the connection to the website , hence making IP block-list not effective.


# 3) Domain Name 
Above the IP address we have domain names. Domain name is basically a string mapped to a specific IP address . Domain names simplifies it for us to remember complex IP address . When we find a domain name an attacker is using , then this starts to hurt because now the attacker will have to purchase a new domain name , register it and start again on what he wants to do with the domain . So by finding a domain name we can cause some problems for the attacker. Attackers exploit domain names by using the <mark style="background: #FFB86CA6;">Shortened URL technique and the Punycode technique</mark>. In the shortened URL technique the attackers hide the location of the malicious website by making the URL's short and making it look as if the URL will be taking the user to the correct location . In Punycode technique the attackers make similar domains to the legitimate one making the user feel as if he is clicking on the correct domain name . Example as follow -
Legitimate Domain name - Adidas.com
Punycode - Ad1das.com


# 4) Host Artifacts
This is where it starts to get annoying for the attacker . Host artifacts are basically the changes that are made on the system or can be considered as the clues that are left by the attacker on the host/endpoint device (changes to the registry values, new hidden files left etc). If an analyst is able to understand these clues on what the attacker is planning then this becomes really problematic for the threat actor  because now he will have to go back and change his methodologies and craft a new plan to cause havoc to the system as the analyst would now be prepared on how to prevent that attack 


# 5) Network Artifact
Above the host artifacts we have the network artifacts . Network artifacts basically refers to the clues/information of the attacker left on the network . These can include information about the C2(Command & Control Server),user-agent string(Browser), URI patterns followed by POST methods etc. When these are detected the attacker has to go back and craft a new plan giving the analyst time to be prepared for new attacks 

# 6) Tools
If an analyst is able to find out what tools the attacker will be using or has used then countermeasures can be prepared easily . At this point the attacker will most probably stop because now he will have to invest both time and money either in finding new techniques and tools or by making a new tool itself 


# 7) Tactics,Techniques and Procedures(TTP's)
This is the topmost of the pyramid . This is basically the blueprint attacker will follow when conducting out the attack.If this blueprint is found out by the analyst before the attack then countermeasures can be made very easily . Here the attacker only has two choices which as follows-
	- Go back and start research again 
	- Give up and find another target (which is much more easy than going back and doing research work)




---



Graphical representation of Pyramid of Pain
![[Pyramid of Pain.png]]

