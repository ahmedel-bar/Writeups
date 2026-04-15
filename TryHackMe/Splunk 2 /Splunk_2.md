# Splunk 2 aka Boss of the SOC 
# 100 series questions
## scenario 
### Question 1: Identify the competitor website

The objective is to determine the competitor website visited by Amber by analyzing network traffic logs.

#### Step 1: Identify Amber's IP address

To begin the investigation, we search for Amber-related events:

- index="botsv2" amber 

Then, we focus on PAN traffic logs to extract her IP address:

index="botsv2" sourcetype="pan:traffic"

you will find the  `src_ip: 10.0.2.101`

#### Step 2: Analyze HTTP traffic

After identifying Amber's IP address, we analyze her web activity:

- index="botsv2" IPADDR sourcetype="stream:HTTP"

you can navigate to  `http_referrer` to see all visited websites 

#### Step 3: Extract visited websites

To narrow down the results and focus on websites:

`| dedup site`  
`| table site`
![Visited Websites](Images/1.png) 

#### Q1: Amber Turing was hoping for Frothly to be acquired by a potential competitor which fell through, but visited their website to find contact information for their executive team. What is the website domain that she visited?
Through a Google search, I found that Frothly is a brewing supplies company.
The domain `www.berkbeer.com` was identified as the competitor website, as it belongs to the same industry (beer/beverage) as Frothly.
Answer: `www.berkbeer.com`

### Question 2-7 
After identifying the competitor website, we focused on Amber’s HTTP traffic to that domain:

- index="botsv2" IPADDR sourcetype="stream:HTTP" COMPETITOR_WEBSITE

This helped identify the image accessed by Amber.


Next, we analyzed SMTP traffic to retrieve Amber’s email address and investigate communication with the competitor:

- index="botsv2" sourcetype="stream:smtp" AMBERS_EMAIL COMPETITOR_WEBSITE

This allowed us to identify email exchanges and extract additional details such as the CEO’s full name.

#### Q2: Amber found the executive contact information and sent him an email. What image file displayed the executive's contact information? Answer example: /path/image.ext
you can you this query 
- index="botsv2" 10.0.2.101 sourcetype="stream:HTTP" www.berkbeer.com 
| table uri_path 
| dedup uri_path

![CEO berk](Images/2.png) 

Answer: `/images/ceoberk.png`

#### Q3: What is the CEO's name? Provide the first and last name.

by using this query 
- index="botsv2" smtp
by the way, SMTP protocol is Simple Mail Transfer Protocol which is related to mails 
  you will find amber mail `aturing@froth.ly`
use this -index="botsv2" smtp  aturing@froth.ly ceo
then click `show as raw text` to show all info 
![ceo name and mail](Images/3.png)
Answer: `Martin Berk`

#### Q4: What is the CEO's email address?

with the same query above you can find sender_email: mberk@berkbeer.com

Answer: `mberk@berkbeer.com`

#### Q5: After the initial contact with the CEO, Amber contacted another employee at this competitor. What is that employee's email address?
- index="botsv2" smtp  aturing@froth.ly berkbeer
  you will find another email address from the same company
  
  ![another employee mail](Images/4.png)
  
  Answer : `hbernhard@berkbeer.com`

#### Q6: What is the name of the file attachment that Amber sent to a contact at the competitor?
- index="botsv2" smtp  aturing@froth.ly berkbeer

![attachment](Images/5.png)

Answer: `Saccharomyces_cerevisiae_patent.docx`

#### Q7: What is Amber's personal email address?
by invistigate the same event from above question, you will find the content-type is base64 decode 
![content-type](Images/6.png)

 use [cyberchef](https://cyberchef.org) to decode 
 
![decoded mail](Images/7.png)

Answer: `ambersthebest@yeastiebeastie.com`

  






