
#Digital_Forensics
Digital Forensics and Incident Response are two pillars in the cyber security defensive world which are very important. Incident Response refers to the blueprint that is followed by SOC analyst once an incident occurs and Digital Forensics is the process of investigating artifacts left by the attacker during an event/attack

---

Digital Forensics refers to the branch that is responsible for extracting artifacts left by an attacker after an event on devices like computers,mobiles,servers,etc. 

Incident Response refers to the blueprint or to a manual that has to be followed by the analyst after an event has occurred in order to get the affected system back to its normal working condition  


## Basic Concept's related to DFIR

### 1) Artifacts
Artifacts refer's to the activity that is performed on a system by the attacker. Ex - If an attacker uses window registry to ensure persistence ,the we can consider the window registry as artifacts
### 2) Evidence Preservation
When performing digital forensics , it is very important to ensure the integrity of the evidence . In order to do so , the best practice is to make the evidence write-protected first and then analysing/working on one of the evidence's write-protected copy . This make sure's that are original evidence does not get contaminated
### 3) Chain of Custody
Chain of Custody is the process of documenting the complete journey of an evidence. 
### 4) Order of Volatility
This refers to providing priority to digital devices over another for analysis/processing . Example - Lets say that we have a hard drive and a RAM stick for analysis. Now RAM is much more volatile in nature in comparison to the hard drive,because RAM only stores information in it until the system is getting power . Once the system shutdown's all the information in the RAM stick will be lost, but this is not the case with a hard drive . Data in the hard drive remains the same even after a system shutdowns . 

So in conclusion we should analyse the RAM stick first and then the hard drive. 
This is called as the order of volatility  
### 5) Timeline Creation
This refers to putting all the events that occurred in chronological order so that we can perform efficient and effective analysis.



## Incident Response Process
The Incident Response process has multiple phases in it . According to the SANS Institute , the phases of an incident response plan are -

1. Preparation 
2. Identification
3. Containment
4. Eradication
5. Recovery
6. Lesson Learnt

![[Incident Response Phase.png]]

