# Qradar101
lab Link [Qradar101](https://cyberdefenders.org/blueteam-ctf-challenges/qradar101/)
## Category : Threat Hunting
## Tools : 
  - IBM Qradar

## How to setup the lab? 
follow the instraction in the lab and watch the video

![instractions](Images/0.png)

## Scenario
```
A financial company was compromised, and they are looking for a security analyst to help them investigate the incident. The company suspects that an insider helped the attacker get into the network, but they have no evidence.
 

The initial analysis performed by the company's team showed that many systems were compromised. Also, alerts indicate the use of well known malicious tools in the network. As a SOC analyst, you are assigned to investigate the incident using QRadar SIEM and reconstruct the events carried out by the attacker.
```

## Dataset: 
  - Sysmon - swift on security configuration
  - Powershell logging
  - Windows Eventlog
  - Suricata IDS
  - Zeek logs (conn, HTTP)


### Q1: How many log sources available?
when you open the qradar in the first time, you will see this screen 

![instractions](Images/1.png)

To know number of log sources, you need to navigate to admin panel

![instractions](Images/2.png)

under data sources you can see Log sources, so press it

![instractions](Images/3.png)

you can count them or as you can see below the screen

Answer: `15` 



### Q2: What is the IDS software used to monitor the network?

as mentioned in the challenge details 

![instractions](Images/4.png)

Answer: `suricata`


### Q3: What is the domain name used in the network?

What is the event ID that will be triggered if the user from the domain successfully logs in?

EventID: 4624 *sucessful login*

but first you need to chance time range to include all events 
choose any old time because the logs start in 2020  

![instractions](Images/5.png)

then 

![instractions](Images/6.png)

after that add the customize search  then click add Filter
then click search

![instractions](Images/7.png)

you will show the next

![instractions](Images/8.png)

double click on any log to show payload information 

![instractions](Images/9.png)

Answer: `hackdefend.local`



### Q4: Multiple IPs were communicating with the malicious server. One of them ends with "20". Provide the full IP.

as in the previous question, specify the time range and group the result by source ip 

![instractions](Images/10.png)

you will find that src ip ends with `20`

Answer: `192.168.20.20`


### Q5: What is the SID of the most frequent alert rule in the dataset?
every signature rule in IPS/IDS has SID

we need to group by `RULE SID` as below

![instractions](Images/11.png)

you will find that 
![instractions](Images/12.png)


Answer: `2027865`




### Q6: What is the attacker's IP address?

offense a high-level security alert that groups related suspicious events or flows together. 
so, I navigate to offense to investigate in 

first, you need to clear default filters to show offenses

I started with this because it is the only public IP address in the list.

![instractions](Images/13.png)

double click on it to show more information
The offense indicates excessive firewall deny events originating from a single public source IP targeting multiple internal hosts. 
This behavior is consistent with a potential scanning or reconnaissance activity, where the attacker attempts to probe multiple systems within the network.

![instractions](Images/14.png)


Answer: `192.20.80.25`





### Q7: The attacker was searching for data belonging to one of the company's projects, can you find the name of the project?

search for payload contain project as below 

![instractions](Images/15.png)

get deeper in any event to see payload 
I choosed this one 

you will find that 

![instractions](Images/16.png)



Answer: `project48`



### Q8: What is the IP address of the first infected machine?

set a filter within attacker ip as a source address, then sort by time 

![instractions](Images/17.png)




Answer: `192.168.10.15`




### Q9: What is the username of the infected employee using 192.168.10.15?





Answer: ``



### Q10: Hackers do not like logging, what logging was the attacker checking to see if enabled?




Answer: ``


### Q11: Name of the second system the attacker targeted to cover up the employee?



Answer: ``


### Q12: When was the first malicious connection to the domain controller (log start time - hh:mm:ss)?




Answer: ``

### Q13: What is the md5 hash of the malicious file?





Answer: ``


### Q14: What is the MITRE persistence technique ID used by the attacker?




Answer: ``


### Q15: What protocol is used to perform host discovery?




Answer: ``


### Q16: What is the email service used by the company?(one word)



Answer: ``

### Q17: What is the name of the malicious file used for the initial infection?



Answer: ``


### Q18: What is the name of the new account added by the attacker?


Answer: ``


### Q19: What is the PID of the process that performed injection?


Answer: ``

### Q20: What is the name of the tool used for lateral movement?


Answer: ``


### Q21: Attacker exfiltrated one file, what is the name of the tool used for exfiltration?


Answer: ``


### Q22: Who is the other legitimate domain admin other than the administrator?


Answer: ``

### Q23: The attacker used the host discovery technique to know how many hosts available in a certain network, what is the network the hacker scanned from the host IP 1 to 30?

Answer: ``


### Q24: What is the name of the employee who hired the attacker?



Answer: ``





































    
