#Phishing_Analysis 
Tools help us in extracting important information from an email for a efficient and effective analysis. 

---

## What Information should be collected?
Whenever we perform analysis on an email , there are some fields that have to be looked at . The fields are -

### In Email Header
1. Sender's Email Address 
2. Sender's IP Address
3. Reverse Lookup of Sender's IP Address 
4. Recipients Email Address (Information can be in CC/BCC)
5. Subject of the Email
6. Reply-To email address
7. Date and Time

### In Email's Body
1. Any URL Links(Check if a URL shortening service was used, then what is the original URL)
2. Name of the Attachment
3. Hash Value of the Attachment


### Tools Used For Email Header Analysis
1. MessageHeader - [Google MessageHeader](https://toolbox.googleapps.com/apps/messageheader/analyzeheader)
2. Message Header Analysis - [Header Analysis](https://mha.azurewebsites.net/)
3. MailHeader - [MailHeader.org](https://mailheader.org/)
4. IP Info - [IPinfo](https://ipinfo.io)
5. URL Scan - [URLScan.io](https://urlscan.io/)
6. Browser Simulate [WannaBrowser](https://www.wannabrowser.net/)
7. Talos Reputation Centre - [Talos](https://talosintelligence.com/reputation) 
 
### Tools Used for Email Body Analysis
1. URL Extractor - [URL](https://www.convertcsv.com/url-extractor.htm)
If we have a file as an attachment, then we can use the hash value to find if the file is malicious or not using many tools -
2. Virus Total - [Virus_Total](https://www.virustotal.com/gui/)
3. Talos File Reputation -[Talos](https://talosintelligence.com/talos_file_reputation)

### Malware SandBoxes
These are virtual environments where we can run malicious files to see their behaviour . Some Platforms which can be used -
1. Any.Run - [Any_Run](https://app.any.run/)
2. Hybrid Analysis - [Hybrid_Analysis](https://www.hybrid-analysis.com/)
3. Joe Security - [Joe's_Security](https://www.joesecurity.org/)

