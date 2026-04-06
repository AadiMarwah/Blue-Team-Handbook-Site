#SOC_Solution 
As the volume and complexity of alerts increase, A SOC team faces many challenges like alert fatigue,too many disconnected tools, inadequate communication with other departments,etc.Making the job of the SOC difficult. To overcome these problems we use the SOAR Tool 

---

The Security Orchestration,Automation & Response Tool is a tool which helps the SOC team by making its's workflow smooth and efficient. The SOAR tool unifies all the other disconnected tool an analyst use into one interface,meaning that now the analyst does have switch between SIEM,EDR,Firewall or other security solution's.

This tool also provides the analyst's with the feature's of case management and ticketing, allowing for proper documentation and resolution of the threat's.

The core capabilities of SOAR as follows -

1. <mark style="background: #FF5582A6;">Orchestration</mark> - Unification of all tools into one interface. In orchestration there are fixed steps defined to investigate different types of threats . These blueprints which are followed are called as <mark style="background: #FFB86CA6;">Playbook's</mark>. 

2. <mark style="background: #FF5582A6;">Automation</mark> - Coordination between multiple tools can be automated,allowing the SOC Analyst's to save time and to go through hundreds of alerts without getting exhausted/fatigued. The SOAR will itself follow the playbook without any human intervention.

3. <mark style="background: #FF5582A6;">Response</mark> - SOAR gives the ability to take actions involving multiple tools/security solutions using only one unified interface .


# SOAR Playbooks

SOAR Playbook's as previously discussed, are different  workflow's that are followed by the SOAR Tool for specific alerts/cases.These playbooks are made for alerts that happen regularly(Ex-Phishing). 
Lets take a look on how a phishing playbook may look like - 

![[Phishing Playbook (SOAR).png]]
