# Splunk 2 aka Boss of the SOC 
# 100 series questions
## scenario 
### Question 1: Identify the competitor website

The objective is to determine the competitor website visited by Amber by analyzing network traffic logs.

#### Step 1: Identify Amber's IP address

To begin the investigation, we search for Amber-related events:

index="botsv2" amber 

Then, we focus on PAN traffic logs to extract her IP address:

index="botsv2" sourcetype="pan:traffic"

you will find the  `src_ip: 10.0.1.200`

#### Step 2: Analyze HTTP traffic

After identifying Amber's IP address, we analyze her web activity:

index="botsv2" IPADDR sourcetype="stream:HTTP" 

you can navigate to  `http_referrer` to see all visited websites 

#### Step 3: Extract visited websites

To narrow down the results and focus on websites:

| dedup site  ` to remove duplicated ` 
| table site
![Visited Websites] (Images/1.png)










