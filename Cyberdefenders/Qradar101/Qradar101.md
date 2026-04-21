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

The domain name can be found by filtering Event ID 4624
which related to sucessfull login to show account information.

but first you need to chance time range to include all events 

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







































    
