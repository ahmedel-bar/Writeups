# Fixit Writeup
### Lab Link [Fixit](https://tryhackme.com/room/fixit)

## Scenario
```
 you've just completed your third screening interview for a SOC Level 2 role at MSSP Cybertees Ltd,
and you're now faced with the final assessment to test your knowledge.
You'll be given access to a Splunk instance receiving network logs from an unknown source.
The data isn't arriving in a usable state, so before you can analyze what's happening on the network, you must Fixit!
```

### Objectives
This challenge is divided into three phases
- Fix event boundaries for the incoming logs
- Extract custom fields from the available events
- Analyze event data to uncover network activity

## Phase 1: Fixing Event Boundaries
The first phase of your challenge is to fix the event boundaries for the incoming logs. As seen in the screenshot below and in your Splunk instance, the raw data is being ingested and Splunk cannot determine where one event ends and the next begins, making the data impossible to analyze. Go ahead and jump into the Fixit app's configuration files to get started!

<img width="524" height="221" alt="image" src="https://github.com/user-attachments/assets/bc13106d-e125-4e39-9e21-ecf452fc8979" />

## Phase 2: Extracting Custom Fields
The next phase of your challenge requires the extraction of meaningful fields from your client's event data. You can accomplish this by updating the Fixit app’s configuration files or by creating field extractions directly through the Splunk UI.

Use the sample logs below to help extract the following fields

- Username
- Department
- Domain
- URI
- SourceIP
- Country


```
[Network-log]: User named Emily Clark from Finance department accessed the resource Cybertees.THM/contact.html from the source IP 192.168.1.4 and country 
Japan at: Mon Dec  1 10:13:38 2025
[Network-log]: User named Robert Wilson from HR department accessed the resource Cybertees.THM/signup.html from the source IP 10.0.0.2 and country 
Germany at: Mon Dec  1 10:13:42 2025
[Network-log]: User named Patricia Allen from Finance department accessed the resource Cybertees.THM/checkout.html from the source IP 172.16.0.1 and country 
Mexico at: Mon Dec  1 10:13:48 2025
```

## Phase 3: Analyzing Event Data
Once the log data is flowing in correctly and the fields have been extracted, it's time to begin your analysis. Using the available data, apply your skills to uncover what's happening on the network!




### Q1: What is the full path to the Fixit app directory in your instance?

to find the answer navigate to /opt/splunk 

![app](Images/0.png) 

![app](Images/3.png) 

then search for the fixit usingthis command
- find . -iname fixit 2>/dev/null

![app](Images/4.png) 

Answer: `/opt/splunk/etc/apps/fixit`


### Q2: Investigate the inputs.conf configuration file of the Fixit app.What is the full path of the network-logs script?








