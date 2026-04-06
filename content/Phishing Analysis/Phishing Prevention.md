#Phishing_Analysis 
As Phishing is one of the most used social engineering tactics today,preventive measures are of extreme importance. There are different frameworks and workflows that can be used by organizations in order to stop the phishing attempts.

---

# 1)Sender Policy Framework(SPF)
Sender Policy Framework or SPF ,is used by the recipient mail server to verify that the mail server sending the email is authorized by the domain to send emails on its behalf . The SPF rules are stored in the DNS as a form of TXT records containing a list of IP address  .

 <mark style="background: #FF5582A6;">Lets Take an example for better understanding</mark> 
I own a domain called Anon.xyz , Now the only servers I am allowing to send emails on behalf of my domain are with the IP address of (10.10.10.10),(67.67.67.67),(20.20.20.20).
Considering an email is sent on the behalf of our domain. The receiving mail server will verify the sending server using the SPF records. It will check  if the server's IP address is included in the authorized list of IP address which can send the emails. 
Based on the result it is decided if the mail is accepted or rejected . <mark style="background: #FFB86CA6;">So in short when an email is sent,the receiving mail server verifies the sending mail server by looking at the domain's SPF record</mark> 

The delivery of the email depends upon the result obtained by using the SPF record.For different cases ,the delivery of email as follows -
1. <mark style="background: #BBFABBA6;">Pass,Neutral,None</mark> - Allow (Accept and Process the Email)
2. <mark style="background: #FFF3A3A6;">SoftFail,PermError</mark> - Flag (Mark as suspicious but allow)
3. <mark style="background: #FF5582A6;">Fail,TempError</mark> - Reject (Email is Discarded)

<mark style="background: #FF5582A6;">How an SPF record looks like?</mark>
![[SPF Record Syntax.png]]


<mark style="background: #FF5582A6;">General WorkFlow of SPF</mark>
![[WorkFlow_Of_SPF.png]]

Tool to look up SPF Records for different domains - [SPF](https://mxtoolbox.com/spf.aspx) 

---


# 2)Domain Key Identified Mail(DKIM)
This is used to check the integrity of the sent email and also implicitly proves if the sender is authorized . The DKIM record are stored on the DNS .Lets see how DKIM works-

When an email is sent , the sending mail server adds a digital signature to the message using a private key . The receiver's mail server verifies this message by retrieving the public key from the DKIM records . If the signature matches then we know that the mail has not been altered/tampered with during transmission. However if the signature is not the same then the email will get flagger or rejected 

<mark style="background: #FF5582A6;">How a DKIM Record look like?</mark>
![[DKIM Record Syntax.png]]

<mark style="background: #FF5582A6;">General Workflow of DKIM</mark>
![[Workflow_Of_DKIM.png]]


---

# 3) Domain-Based Message Authentication,Reporting ,and Conformance (DMARC)

DMARC sits on top of the DKIM and SPF. It uses a concept of alignment and tells the receiving server what to do if the alignment fails . <mark style="background: #FFB86CA6;">Basically keeps in mind the output of SPF and DKIM and then based on outputs ,see if there is alignment</mark>


<mark style="background: #FF5582A6;">How DMARC records look like</mark> 
![[DMARC Record Syntax.png]]

Too to lookup DMARC records for different domains - [DMARC](https://mxtoolbox.com/dmarc.aspx)

---


# 4) Secure/Multipurpose Internet Mail Extensions (S/MIME)

S/MIME is a standard protocol that is used to send messages which are digitally signed and are encrypted . Digital Signatures and Encryption helps us to achieve many things , which as follows 

#### Digital Signature 
1. <mark style="background: #FF5582A6;">Authentication</mark> - Confirm Senders Identity using their Digital Certificate 
2. <mark style="background: #FF5582A6;">Non-Repudiation</mark> - Ensures the sender cannot deny sending the message later.
3. <mark style="background: #FF5582A6;">Data Integrity</mark>  - Detects any changes to the message after it's signed

#### Encryption
1. <mark style="background: #FF5582A6;">Confidentiality</mark> - Keeps the content private and readable only by the intended recipient
2. <mark style="background: #FF5582A6;">Integrity</mark> - Detects any changes during message transmission

How S/MIME will work when two people have to communicate . For our case lets take Alice and Bob 

Both Alice and Bob will require certificates which will have their public key's in it.These certificates will be exchanged between Alice and Bob when communication is done between the two for the first time. When Alice will send an email to Bob then that email will be signed first using Alice's Private Key and then will be encrypted using Bob's Public Key . Bob  will receive that message and will verify the signature by using Alice's Public Key(retrieved from the digital certificate) and then will decrypt the message using his own private key. This entire process ensures that the the sender is authenticated and the email is not tampered with during transit.

<mark style="background: #FF5582A6;">General WorkFlow of S/MIME</mark> -
![[WorkFlow_Of_SMIME.png]]


  





