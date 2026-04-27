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
























































































## The End 
# I hope you find it useful.



