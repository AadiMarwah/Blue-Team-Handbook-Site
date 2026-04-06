#Team_Internals 
[[Alert Triage]]
Workbooks are designed so that for specific cases or for specific events , triaging can be done efficiently and effectively without missing any key details.

---

In alert triaging we remove the false positives from the true positive which is generally easy but there are some cases on which extra thought has to be given . This is where Workbooks come in play. 

A SOC workbook is a blueprint that the analysts follow when triaging for specific events , following the workbooks make the entire process extremely fast and smooth which eventually leads to excellent alert triaging.

Example of how a workbook may look like-
![[WorkBooks.png]]


---


When it comes to triaging there are many record/assets that are looked at. The factors looked at are as follows-
1. <mark style="background: #FF5582A6;">Identity Inventory</mark> - Identity Inventory is a catalogue which hold information about the employees name, job profile,access level/privileges given,user account,contacts,etc. Here is a table to give a basic example of how the identity Inventory may look like -

|  *Full Name*  | *Username* |    *Email*    |         *Role*          | *Location* |            *Access*             |
|:-------------:|:----------:|:-------------:|:-----------------------:|:----------:|:-------------------------------:|
| Gregory Baker |  G.Baker   | G.baker@hello | Chief Financial Adviser | Europe,UK  | VPN,Admin Portal,Finance portal |
| Raymond Green |  R.Green   | R.Green@hello |    Financial Manager    |    USA     |        VPN,Amin Poratal         |

The table shows us that we have a employee Gregory Baker who is a Chief Financial Adviser working out in the UK office having access to VPN, Admin Portal and Finance Portal . We also have a employee with the name of Raymond green who is a Financial manager working out of USA having different access than Gregory.

2.<mark style="background: #FF5582A6;">Asset Inventory</mark> - Asset Inventory or asset lookup is a manifest of the companies computational resources like workstations,server etc.Here is a table for getting a basic idea of how the asset inventory may look like -

| *HostName* | *Location*     |  *IP Adress*  |        *OS*         |        *Owner*        |                  *Purpose*                  |
|:----------:| -------------- |:-------------:|:-------------------:|:---------------------:|:-------------------------------------------:|
|  HQ-1234   | USA Datacenter | 172.16.15.89  | Windows Server 2022 | Central IT Department |      File server for financial records      |
|   PC-245   | UK Office      | 192.168.5.100 |     Linux(Arch)     | Central IT Department | Corporate Laptop given to employee for work |




