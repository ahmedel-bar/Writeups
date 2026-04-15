# Splunk 2 aka Boss of the SOC 
# 100 series questions
## scenario 
The first objective is to find out what competitor website she visited. What is a good starting point?

When it comes to HTTP traffic, the source and destination IP addresses should be recorded in logs. You need Amber's IP address.

You can start with the following command, index="botsv2" amber, and see what events are returned. Look at the events on the first page. 

Amber's IP address is visible in the events related to PAN traffic, but it's not straightforward. 

To get her IP address, we can hone in on the PAN traffic source type specifically. 

Command: index="botsv2" sourcetype="pan:traffic"

From here, you should have Amber's IP address. You can build a new search query using this information.

It would be best if you used the HTTP stream source type in your new search query. 

Using Amber's IP address, construct the following search query. 

Command: index="botsv2" IPADDR sourcetype="stream:HTTP"

You must substitute IPADDR with Amber's IP address.

After this query executes, there are many events to sift through for the answer. How else can you narrow this down?

Look at the additional fields.

Another field you can add to the search query to further shrink the returned events list is the site field.

Think about it; you're investigating what competitor website Amber visited.

Expand the search query only to return the site field. Additionally, you can remove duplicate entries and display the results nicely in a table. 

Command: index="botsv2" IPADDR sourcetype="stream:HTTP" | KEYWORD site | KEYWORD site

You must substitute KEYWORD with the Splunk commands to remove the duplicate entries and display the output in a table format.

Note: The first KEYWORD is to remove the duplicate entries, and the second is to display the output in a table format. 

The results returned to show the URIs that Amber visited, but which website is the one that you're looking for?

To help answer these questions: Who does Amber work for, and what industry are they in? 

The competitor is in the same industry. The competitor website now should clearly be visible in the table output.

Extra: You can also use the industry as a search phrase to narrow down the results to a handful of events (1 result to be exact). 

Command: index="botsv2" IPADDR sourcetype="stream:HTTP" *INDUSTRY* | KEYWORD site | KEYWORD site

Note: Use asterisks to surround the search term.

Questions 2-7

Amber found the executive contact information and sent him an email. Based on question 2, you know it's an image.

Since you now know the competitor website, you can construct a more specific search query isolating the results to focus on Amber's HTTP traffic to the competitor website. 

Command: index="botsv2" IPADDR sourcetype="stream:HTTP" COMPETITOR_WEBSITE

Replace COMPETITOR_WEBSITE with the actual URI of the competitor website. 

You can expand on the search query to output the specific field you want in a table format for an easy-to-read format, as we did for the previous objective. 

Based on the image, you have the CEO's last name but not his first name. Maybe you can get the name in the email communication.

You can now draw your attention to email traffic, SMTP, but you need Amber's email address. You should be able to get this on your own. :)

Once you have Amber's email address, you can build a search query to focus on her email address and the competitor's website to find events that show email communication between Amber and the competitor. 

Command: index="botsv2" sourcetype="stream:smtp" AMBERS_EMAIL COMPETITOR_WEBSITE

Replace AMBERS_EMAIL with her actual email address. 




