# Splunk 3 aka Boss of the SOC writeup 
### lab Link [Splunk 3](https://tryhackme.com/room/splunk3zs)

# TASK 3 : AWS & other events

### Q1: List out the IAM users that accessed an AWS service (successfully or unsuccessfully) in Frothly's AWS environment? Answer guidance: Comma separated without spaces, in alphabetical order. (Example: ajackson,mjones,tmiller)

firstly, I used the command givent in the task 2 to list all sourcetype available
![cm](Images/0.png)

one of them are related to AWS
![cm](Images/1.png)

then I used this query 
  - index="botsv3" sourcetype = aws* | stats count by user

![cm](Images/2.png)

Answer: `bstoll,btun,splunk_access,web_admin`



### Q2: What field would you use to alert that AWS API activity has occurred without MFA (multi-factor authentication)? Answer guidance: Provide the full JSON path. (Example: iceCream.flavors.traditional)

in the link given in the Q1 [AWS](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-log-file-examples.html)

I found that 

![cm](Images/3.png)

by using this query 
   - index="botsv3" sourcetype = aws*  mfaAuthenticated

![cm](Images/4.png)

by searching on fields for mfa you can find that

![cm](Images/5.png)


Answer: `userIdentity.sessionContext.attributes.mfaAuthenticated`



### Q3: What is the processor number used on the web servers? Answer guidance: Include any special characters/punctuation. (Example: The processor number for Intel Core i7-8650U is i7-8650U.)

return back to the command from task 2 to list all sourcetypes
 - index="botsv3" hash | stats count by sourcetype | sort -count

![cm](Images/6.png)

the `dmesg` is related to `kernel logs` which can be related to `Hard Ware` component 
so, I used it as a source type and search for CPU
  - index="botsv3" sourcetype = dmesg CPU

and found that 

![cm](Images/7.png)


Answer: `E5-2676`


### Q4: Bud accidentally makes an S3 bucket publicly accessible. What is the event ID of the API call that enabled public access? Answer guidance: Include any special characters/punctuation.

`S3 : is a bucket in Amazon Web Services is a cloud storage container used to store and manage files like images, logs, and backups.`


by using this query be cause of the given link 
  - index="botsv3" sourcetype = aws*  PutBucketAcl

I found 2 events, by analyze both I found that 

![cm](Images/8.png)


Answer: `ab45689d-69cd-41e7-8705-5350402cf7ac`


### Q5: What is Bud's username?

from the same event in the previous question click show raw text

![cm](Images/10.png)


Answer: `bstoll`


### Q6: What is the name of the S3 bucket that was made publicly accessible?

also you can find the answer from the same event 

![cm](Images/11.png)

Answer: `frothlywebcode`



### Q7: What is the name of the text file that was successfully uploaded into the S3 bucket while it was publicly accessible?Answer guidance: Provide just the file name and extension, not the full path. (Example: filename.docx instead of /mylogs/web/filename.docx)

in source type I found that there was one related to `s3`

![cm](Images/12.png)

![cm](Images/9.png)

so, I filtered using this sourcetype and bucket name found in the previous question, 
(PUT OR POST ) because file is uploaded and stats by uri to show only uri
 - index="botsv3" sourcetype="aws:s3:accesslogs" bucket_name=frothlywebcode  (PUT OR POST) | stats count by request_uri

![cm](Images/13.png)


Answer: `OPEN_BUCKET_PLEASE_FIX.txt`


### Q8: What is the FQDN of the endpoint that is running a different Windows operating system edition than the others?

Firstly, I print all souretypes to see which of them are related to windows log and I found that

![cm](Images/14.png)

`winhostmon is a data source that collects monitoring information from Windows hosts, such as processes, services, and system activity.`

so I filtered using
  - index="botsv3" sourcetype="winhostmon" OS

then press all fields and search for OS 

![cm](Images/15.png)

as you can see the `Microsoft Windows 10 Enterprise` is less used

so I filtered using it to see more info
  - index="botsv3" sourcetype="winhostmon" "Microsoft Windows 10 Enterprise"

I found computer name 

![cm](Images/16.png)

```
FQDN = computername + domain
     = BSTOLL-L + froth.ly
we already know domain from Splunk 2 lab
```

or we can search by `BSTOLL-L` to see the FQDN

![cm](Images/17.png)

Answer: `BSTOLL-L.froth.ly`




# Task 4 : Cryptomining events

### Q1: A Frothly endpoint exhibits signs of coin mining activity. What is the name of the second process to reach 100 percent CPU processor utilization time from this activity on this endpoint? Answer guidance: Include any special characters/punctuation.

do any general query and search for CPU to see which field contains cpu percentage usage

![cm](Images/18.png)

then filter by it = 100 and list it and its name 
 - index="botsv3" process_cpu_used_percent = 100 | top process_name process_cpu_used_percent

![cm](Images/19.png)

Answer: `chrome#5`




### Q2: What is the short hostname of the only Frothly endpoint to actually mine Monero cryptocurrency? (Example: ahamilton instead of ahamilton.mycompany.com)

as you can see in the image above 
the process chrome#4 run about 129 which seemed suspicious so I filtered using it 
  - index="botsv3" process_cpu_used_percent = 100 chrome#4

![cm](Images/20.png)

Answer: `BSTOLL-L`


### Q3: Using Splunk's event order functions, what is the first seen signature ID of the coin miner threat according to Frothly's Symantec Endpoint Protection (SEP) data?

use sourcetype = symantec:ep:security:file which related to antivirus, EDR, security 
 -index="botsv3" sourcetype="symantec:ep:security:file" | table CIDS_Signature_ID _time

![cm](Images/21.png)

Answer: `30358`



### Q4: What is the name of the attack?

using the same filter

![cm](Images/22.png)

Answer: `JSCoinminer Download 8`



### Q5: According to Symantec's website, what is the severity of this specific coin miner threat?

search for `JSCoinminer Download 8 severity levels`

![cm](Images/23.png)

Answer: `Medium`



### Q6: What is the short hostname of the only Frothly endpoint to show evidence of defeating the cryptocurrency threat? (Example: ahamilton instead of ahamilton.mycompany.com)

using the same filter above which related to AV & EDR & Security 

you will find only one hostname

![cm](Images/24.png)

Answer: `BTUN-L`



# Task 5 : More AWS events

### Q1: What IAM user access key generates the most distinct errors when attempting to access IAM resources?
first, I query all events under `sourcetype = aws*` then search in fileds for accesskey

![cm](Images/25.png)

then search for errors

![cm](Images/26.png)

then from this resource [AWS](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html)

I found that 
![cm](Images/27.png)

 - index="botsv3"  sourcetype = aws* errorCode=accessdenied | stats count by userIdentity.accessKeyId

![cm](Images/28.png)

Answer: `AKIAJOGCDXJ5NW5PXUPA`



### Q2: Bud accidentally commits AWS access keys to an external code repository. Shortly after, he receives a notification from AWS that the account had been compromised. What is the support case ID that Amazon opens on his behalf?

using the hint 
![cm](Images/29.png)

using this query 
  - index="botsv3" sourcetype="stream:smtp" "access keys"

![cm](Images/30.png)


### Q3: AWS access keys consist of two parts: an access key ID (e.g., AKIAIOSFODNN7EXAMPLE) and a secret access key (e.g., wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY). What is the secret access key of the key that was leaked to the external code repository?

becuase the `Bud accidentally commits AWS access keys to an external code repository` I searched for this repo in the event from previous question 
to find the commited access keys 

![cm](Images/31.png)

![cm](Images/32.png)

Answer: `Bx8/gTsYC98T0oWiFhpmdROqh*ELPtXJSR9vFPNGk`




### Q4: Using the leaked key, the adversary makes an unauthorized attempt to create a key for a specific resource. What is the name of that resource? Answer guidance: One word.

searching by `aws_acccess_key_id` in the repo 

![cm](Images/33.png)

then expand on the query using eventname= create access key 
  - index="botsv3" AKIAJOGCDXJ5NW5PXUPA eventName=CreateAccessKey

![cm](Images/34.png)

Answer: `nullweb_admin`



### Q5: Using the leaked key, the adversary makes an unauthorized attempt to describe an account. What is the full user agent string of the application that originated the request?

as we did in the previous question 

![cm](Images/35.png)

- index="botsv3" AKIAJOGCDXJ5NW5PXUPA eventName=DescribeAccountAttributes

![cm](Images/36.png)


Answer: `	ElasticWolf/5.1.6`



# Task 6 : Pivoting back to endpoint events

### Q1: What is the full user agent string that uploaded the malicious link file to OneDrive?

I started free search with `OneDrive` 
then, I expanded on the query using sourcetype related to office365 which related to OneDrive 
 - index="botsv3" sourcetype="ms:o365:management" OR sourcetype="o365:management:activity" | stats count by UserAgent

![cm](Images/37.png)

by searching for `NaenaraBrowser` it turned out that it's a browser related to `north korean`

![cm](Images/38.png)

Answer: `Mozilla/5.0 (X11; U; Linux i686; ko-KP; rv: 19.1br) Gecko/20130508 Fedora/1.9.1-2.5.rs3.0 NaenaraBrowser/3.5b4`


### Q2: What was the name of the macro-enabled attachment identified as malware?
as we know file extensions are associated with macro-enabled are `xlsm, docm, pptm`
so, I tried to seacrh for them 

  - index="botsv3" (docm OR pptm OR xlsm) | stats count by TargetFilename

![cm](Images/40.png)

investiagte more in both

![cm](Images/39.png)

The process `HxTsr.exe` is part of the Windows Mail app, which means the malicious macro file was downloaded from an email.

Answer: `Frothly-Brewery-Financial-Planning-FY2019-Draft.xlsm`




### Q3: What is the name of the executable that was embedded in the malware? Answer guidance: Include the file extension. (Example: explorer.exe)

from the previous question you can find the answer

Answer: `HxTsr.exe`





### Q4: What is the password for the user that was successfully created by the user "root" on the on-premises Linux system?

to add a user in linux you need to use this command `useradd`

so, I searched by it and also search using root 
  - index="botsv3" useradd root

![cm](Images/41.png)


Answer: `ilovedavidverve`



### Q5: What is the name of the user that was created after the endpoint was compromised?

Answer: `svcvnc`






### Q6: Based on the previous question, what groups was this user assigned to after the endpoint was compromised? Answer guidance: Comma separated without spaces, in alphabetical order.

Answer: ``





### Q7: What is the process ID of the process listening on a "leet" port?


Answer: ``





### Q8: What is the MD5 value of the file downloaded to Fyodor's endpoint system and used to scan Frothly's network?

Answer: ``


..




























































































## The End 
# I hope you find it useful.



