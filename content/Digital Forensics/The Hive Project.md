#Digital_Forensics 

The Hive Project is an open-source,scalable and freely available security incident response platform which allows security analyst's and practitioners to investigate,track and act upon a identified incident in a swift and a collaborative manner.

---

## 1) Why Hive is soo good?
The hive project is one of the best security incident response solution in the market because of its following capabilities - 

1. <mark style="background: #FF5582A6;">Collaboration</mark> - Allows multiple analyst's of the same organization to work on one case simultaneously. The hive's live streaming capability also helps everyone to keep an eye on the cases.

2. <mark style="background: #FF5582A6;">Elaborate</mark> - Investigations correspond to cases. The details of each case can be broken down into multiple associated tasks, which can be created from scratch or through a template engine. Additionally, analysts can record their progress, attach artifacts of evidence and assign tasks effortlessly. 

3. <mark style="background: #FF5582A6;">Act</mark> - The hive project support quick triaging for the analysts as they have the ability to add observable s to their case,leveraging tags,flagged IOC's,etc.


## 2) Hive Features and Integrations
We already know that using hive, analysts of one organization can work on the same case simultaneously. This is because the hive project has a very rich feature set along with strong integration.The features include - 

- <mark style="background: #FF5582A6;">Case/Task management</mark> - All the investigations that are done are related to a case . Now each case can be broken down into multiple tasks for granularity. Analyst can also attack evidences,artifacts related to the cases.  

- <mark style="background: #FF5582A6;">Alert Triage</mark> - Cases can be imported from SIEM alerts,Email reports and security events directly allowing the analyst to quickly triage the events and determine whether it need escalation or not 

- <mark style="background: #FF5582A6;">Enrichment with Cortex</mark> - Cortex is an analysis and active response search engine which helps the analysts to extract more information from threat indicators by creating correlations and patterns in cases 

- <mark style="background: #FF5582A6;">Active Responses</mark> - TheHive allows analysts to use Responders and run active actions to communicate, share information about incidents and prevent or contain a threat.

- <mark style="background: #FF5582A6;">Custom Dashboards</mark> - Analyst's can have their own customized dashboard which can consist of statistics,tasks,different metrics and observable for cases.

- <mark style="background: #FF5582A6;">Built-in MISP integration</mark> -  MISP is a threat intelligence platform that allows analysts for sharing, storing and correlating Indicators of Compromise of targeted attacks and other threats

## 3) User profiles & Permissions
An administrator using The hive project has the ability to create a group for an organization and assign different roles to different users . A group comes with 4 pre-defined user profiles which are -

- <mark style="background: #FF5582A6;">Admin</mark> - Full administrative permissions for the platform; However cannot manage cases or data related to the cases 

- <mark style="background: #FF5582A6;">Org-Admin</mark> - Manages users and all organization-level configurations.Can create and edit Cases, Tasks, Observables and even run Analysers and Responders

- <mark style="background: #FF5582A6;">Analyst</mark> - Can create and edit cases,tasks,observable's and even run analysers and responders

- <mark style="background: #FF5582A6;">Read-Only</mark> - Can only read, Cases, Tasks and Observables details;

Each profile has its own set of pre-defined permissions . All of the permissions are - 

|        Permissions        |                       Meanings                       |
| :-----------------------: | :--------------------------------------------------: |
|   manageOrganisation(1)   |           Creat and Udpate organisation's            |
|      manageConfig(1)      |                Update Configurations                 |
|     manageProfile(1)      |           Create, update & delete Profiles           |
|       manageTag(1)        |             Create, update & delete Tags             |
|   manageCustomFields(1)   |         Create, update & delete CustomFields         |
|        manageCase         |            Create, update & delete Cases             |
|     manageObservable      |         Create, update & delete Observables          |
|       manageAlerts        |            Create, update & import Alerts            |
|        manageUser         |            Create, update & delete User's            |
|    manageCasetemplate     |        Create, update & delete Case Template         |
|        manageTask         |            Create, update & delete Tasks             |
|        manageShare        | Share Case,task-observable s with other organization |
|     manageAnalyse(2)      |                   Execute Analyse                    |
|     manageActions(2)      |                   Execute Actions                    |
| manageAnalyseTemplates(2) |      Create, update & delete Analyse Templates       |

TheHive project is a GUI tool which makes things easy to understand and implement by simply interacting with the tool. 
