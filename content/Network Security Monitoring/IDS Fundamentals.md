#Network_Security 
Intrusion Detection Systems are specialized software's that help us in finding out attackers who have bypassed through our firewalls to get access to a endpoint of a network.

---

Intrusion Detection Systems can be categorized based on two parameters which as follows -

## A) Deployment Modes

1. <mark style="background: #FF5582A6;">Host Intrusion Detection Systems</mark> - Host-based IDS solutions are installed individually on the hosts and are responsible for only detecting potential security threats associated with that particular host. They provide detailed visibility of the host’s activities. However, host intrusion detection systems can be challenging to manage in large networks as they are resource-intensive and require management on each host.

2. <mark style="background: #FF5582A6;">Network Intrusion Detection System</mark> - Network-based IDS solutions are crucial in detecting potentially malicious activities within the whole network, regardless of any specific hosts. They monitor the network traffic of all the hosts involved to detect suspicious activities. It provides a centralized view of all the detection's inside the whole network.

## B) Detection Modes

1. <mark style="background: #FF5582A6;">Signature Based IDS</mark> - Each attack that happens has its own unique pattern which is called as a <mark style="background: #FFB86CA6;">Signature</mark> . These signatures are stored in the IDS database for future reference for quick detection of an attack.However this type of IDS fails when there are zero-day attack as there are no prior signature/pattern based on which the detection can be done. 

2. <mark style="background: #FF5582A6;">Anomaly Based IDS</mark> - Anomaly Based IDS are systems where the IDS firstly learns about the normal behaviour of a host or a network and then performs detection if there is any deviation .These type of IDS can detect zero-day attacks as they do not rely on signatures.

3. <mark style="background: #FF5582A6;">Hybrid IDS</mark> - A hybrid IDS combines the detection methods of signature-based IDS and anomaly-based IDS to leverage the strengths of each approach. Some known threats may already have some signatures in the IDS database; in this case, the hybrid IDS would use the detection technique of the signature-based IDS. If it encounters a new threat, it can leverage the detection method of anomaly-based IDS.
 