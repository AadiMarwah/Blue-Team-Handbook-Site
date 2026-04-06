#Phishing_Analysis
Phishing is one of the most used social engineering attack tactics today.In this attack,the attacker relies on the user to click on a malicious link or download a malicious attachment,allowing the threat actor to get a hold of the system or the network.

---

Phishing is a widely used tactic today used by attackers to get a hold of a system or a network . Phishing is generally conducted through emails but now there are many other mediums as well using which phishing can be performed.

## Introduction to Email
Email was made in the 1970's for ARPNET by Ray Tomlinson. 
Now there are 3 things that make an email -
1. User_MailBox(aadimarwah316)
2. @
3. Domain(gmail.com)

## Protocols Used Email Delivery 
A lot of different protocols are used when it comes to sending and receiving emails. The protocols and their working as follows -

1. <mark style="background: #FF5582A6;">SMTP(Simple Mail Transfer Protocol)</mark> - This protocol is used for sending email's. The email that we write is taken from our email client to our mail server and from there it send to the receiver's mail server . After email has been delivered to the receiver's mail server , job of SMTP protocol is over. So in short <mark style="background: #FFB86CA6;">SMTP is used to send the emails to the receiver's mail server</mark>. Uses the <mark style="background: #FFB86CA6;">587 port for Secure Transport</mark>.

2. <mark style="background: #FF5582A6;">POP3(Post Office Protocol)</mark> - The Post office protocol allows the user to access the email's received . Here the emails that are on the server are downloaded onto the system(using which the mails were accessed) and the copies of these emails are removed from the mail server. In short <mark style="background: #FFB86CA6;">POP 3 allows the user to access and read the emails only on one device</mark>.Uses the <mark style="background: #FFB86CA6;">995 port for Secure Transport</mark>.

3. <mark style="background: #FF5582A6;">IMAP(Internet Message Access Protocol)</mark> - The functioning of the IMAP protocol is same as the POP3 protocol but here the emails are not downloaded onto the device. The emails are kept on the server and the user can access these emails on the server itself . In short <mark style="background: #FFB86CA6;">IMAP allows the user to access and read the emails on multiple devices</mark>. Uses the <mark style="background: #FFB86CA6;">993 port for Secure Transport</mark>.

## Email Headers
Email headers include important information regarding an email. The fields of an email header are -
1. <mark style="background: #FF5582A6;">Sender</mark> - Address that sends the email
2. <mark style="background: #FF5582A6;">To</mark> - Recipients email address
3. <mark style="background: #FF5582A6;">Subject</mark> - Tells what the email is about
4. <mark style="background: #FF5582A6;">Date</mark> - Date and Time the email was sent
5. <mark style="background: #FF5582A6;">X-Originating-IP</mark> - The IP address from which the email was originally sent(IP address before the email was relayed through multiple servers,Senders's Original IP address)
6. <mark style="background: #FF5582A6;">smtp.mailfrom/header.from</mark> - The domain from where the email was sent from.
7. <mark style="background: #FF5582A6;">Reply-To</mark> - The email address to which a reply will be sent to . 

## Email Body
Email Body is the text,attachments,hyperlinks that we send in an email. Here also we can find important information.

## Different Types Of Phishing
There are many mediums now using which phishing can be performed. Also there are many different types of targets on which phishing is attempted . Based on these two parameter, the various types of phishing as follows -

1. <mark style="background: #FF5582A6;">Spear Phishing</mark> - In spear phishing instead of sending email's to masses ,email's are send to specific users where the email is surrounded around the users interest, responsibilities,etc.

2. <mark style="background: #FF5582A6;">Whaling</mark> - Whaling is a type of phishing which is targeted toward high ranking officials of a company like CFO's,CEO's,etc.

3. <mark style="background: #FF5582A6;">Smishing</mark> - Smishing is where specially crafted messages are send to the user on mobile devices.

4. <mark style="background: #FF5582A6;">Vishing</mark> - Type of Phishing where the attack medium is phone calls.


<mark style="background: #FF5582A6;">IMPORTANT_NOTE</mark> - When dealing with hyperlinks,attachments,IP address,Defanging is an essential task that has to be performed . Defanging ensure's that malicious hyperlinks,IP address become unclickable,preventing any accidents from happening. 
