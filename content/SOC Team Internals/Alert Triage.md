#Team_Internals 
[[Alert Reporting]]
This is the first steps that are taken when dealing with attacks . Here we organize the alerts and filter them out properly.

---

Alert Triage is basically the process of prioritising the alerts based on the severity of the alerts that we see. As a SOC L1 analyst your job is  to organize and prioritise the alerts based on the damage they can cause.

This is done to  make sure that all the alerts are taken care off in a proper manner . Using triaging we can also send those alerts to the SOC L2 analyst that need in depth analysis . 

Now an alert has many things associated with it . The things that are as follows -

1.<mark style="background: #FF5582A6;">Alert Time</mark> - This is the time when the alert was created. An alert is generally created a little later after the event has already happened/occurred.

2.<mark style="background: #FF5582A6;">Alert Name</mark> - This gives us a very short summary telling which rule has been broken or violated <mark style="background: #FFB86CA6;">(Example - Brute Force Login attempt on company's networking portal)</mark>.


3.<mark style="background: #FF5582A6;">Alert Severity</mark> - This tells the analyst ,to which alert the priority has to be given and in what order . There are 4 levels of severity , which are -
		- Low
		- Medium
		- High
		- Critical 
The priority is given to the critical alerts first and then we move towards the low level alerts while going through the high level alerts and medium level alerts.

4.<mark style="background: #FF5582A6;">Alert Status</mark> - This tells us the current condition of the alert . Like <mark style="background: #FFB86CA6;">is it in progress, has been closed or is new and unassigned</mark> . So alert status tells us where is the alert currently in its life-cycle.

5.<mark style="background: #FF5582A6;">Alert Verdict</mark> - Many times the alert the analyst gets are not actually alerts but are false positive which can cause confusion . So alert verdict is a very important part of triaging as this <mark style="background: #FFB86CA6;">help to distinguish between which alert is a true positive and which alert is a false positive</mark> .

6.<mark style="background: #FF5582A6;">Alert Assignee</mark> - Tells to whom the alert has been assigned to get resolved/reviewed

7.<mark style="background: #FF5582A6;">Alert Description</mark> - Give a proper description about the alert that has occurred. Fields that can be under description section are as follows-
		- Host
		- Process name
		- Target File


After triaging has been done successfully we move on to make proper reports on the alerts and escalating these alerts to L2 analyst if needed for a deeper and thorough analysis .