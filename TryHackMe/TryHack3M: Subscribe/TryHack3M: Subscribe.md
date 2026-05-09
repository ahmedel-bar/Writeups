# TryHack3M: Subscribe Writeup
### Lab Link [TryHack3M: Subscribe](https://tryhackme.com/room/subscribe)
<img width="1200" height="1200" alt="image" src="https://github.com/user-attachments/assets/9b466c2d-043a-4e54-8764-ab0800bd85e7" />

## Incident Storyline
```
We have good news and bad news! The good news is that we are about to hit 3 million users on our platform, and the bad news is;

Well, last night, the UnderGround (UG) Hackers attacked our website, hackme.thm, and took complete control. They were able to turn off the signup page, so there won't be any new registrations. Given this, our user count is stuck at 2.99 Million.
Can you help us restore the registration panel on our site to reach our 3 million user milestone?
```
<img width="1125" height="562" alt="image" src="https://github.com/user-attachments/assets/bdf6e6fa-07da-4296-b6e4-58e99e470381" />

## Task 2 : Exploitation
`Sometimes, the attacker leaves footprints that allow you to regain access to the server.  Can you help HackM3 restore server access and get 3M subscribers?`

### Q1: What is the invite code for the hackme.thm website?

Once you access the webpage, the following page will appear.

![ah](Images/1.png)

To examine the website details, right-click on the page and choose Inspect from the browser menu.

![ah](Images/2.png)

When attempting to sign up, the following message appears: `Registration is currently disabled! Invite-only access`.

![ah](Images/3.png)

Further analysis within the browser’s Inspect panel revealed an invite.js file located in the Sources tab.

![ah](Images/4.png)

by analyzing invite.js it turned out that the browser must access the website using the specified hostname instead of the IP address.

![ah](Images/5.png)

so, open host file as admin `C:\Windows\System32\drivers\etc\hosts`
and add this line `10.114.158.247 capture3millionsubscribers.thm`
![ah](Images/6.png)

if you try to open the url direct from browser you will find this 
![ah](Images/7.png)


Therefore, the request must be sent using POST method by replacing sign-up.php with invite.php using the browser’s Inspect feature.
![ah](Images/8.png)

and add any value in `invite code` box to send request 
![ah](Images/9.png)

Answer: `VkXgo:Invited30MnUsers`

### Q2: What is the password for the user guest@hackme.thm?

Now, use the invite code obtained from the previous question and submit it in the invite code field.

![ah](Images/10.png)

Answer: `wedidit1010`


### Q3: What is the secure token for accessing the admin panel?
now, use the credential above to login 
![ah](Images/11.png)

![ah](Images/12.png)

the free room doesn't that useful, so open the VIP one an go to inspect 

change isVIP=false to true then refresh the site

![ah](Images/13.png)

you now can access the vip room 

![ah](Images/14.png)

but when you try to start the machine to access attack box will find this alert 

![ah](Images/15.png)

by analyze advanced_red_teaming.php you will find this soure

![ah](Images/16.png)

then try to access this path

![ah](Images/17.png)

by apply ls command you will find these files 
![ah](Images/18.png)

then print config.php content
![ah](Images/19.png)

Answer: `ACC#SS_TO_ADM1N_P@NEL`


### Q4: What is the flag value after enabling the registration feature and getting 3M subscribers on the platform?
remeber to add domain to host file 
![ah](Images/22.png)

after trying access the admin panel from prevoius question 
![ah](Images/20.png)

I can't access the site, so I used gobuster to bruteforece directories 

![ah](Images/21.png)

then access login endpoint 

![ah](Images/23.png)

use access token as authentic code 
![ah](Images/24.png)
now, you need user and password
you have to ways
using hydra and bruteforce but this will take much much time
the second is to use sqlmap to dump info 

try any user and password to understand the request 
![ah](Images/25.png)

By analyzing the login.js file, the API endpoint and JSON request format were identified, 
then SQLMap was used to test the login functionality for SQL injection vulnerabilities.

![ah](Images/26.png)

then use this command to retrive user and password

`sqlmap -u "http://admin1337special.hackme.thm:40009/api/login.php" --method POST --data='{"username":"admin","password":"test"}' --headers="Content-Type: application/json" --batch --dump`

![ah](Images/28.png)

use credential to login then set options as Sign up 
then click here
![ah](Images/29.png)

you will redirect to this page 

![ah](Images/30.png)

use this and you will find the answer 


![ah](Images/31.png)


Answer: `TryHack3M{3MSUBSCRIBERS}`




## Task 3 : Detection
### Q1: How many logs are ingested in the Splunk instance?
change time range to all time and search using all indexes

![ah](Images/27.png)

Answer: `10530`

### Q2: What is the web hacking tool used by the attacker to exploit the vulnerability on the website?

By reviewing the User-Agent field in the captured requests, the web hacking tool used by the attacker was identified.

![ah](Images/32.png)

Answer: `sqlmap`


### Q3: How many total events were observed related to the attack?
filter using user-agent above 

![ah](Images/33.png)

Answer: `158`

### Q4: What is the observed IP address of the attacker?
using the same filter and logs 
or the source ip used the malicious user-agent
![ah](Images/34.png)


Answer: `83.45.212.17`


### Q5: How many events were observed from the attacker's IP?

filter only by attacker source-ip 

![ah](Images/35.png)


Answer: `184`

### Q6: What is the table used by the attacker to execute the attack?

The primary SQL query syntax consists of the SELECT statement, which retrieves specific columns from a table based on a given condition.
SELECT <parameter, parameter> FROM <table> WHERE <condition>;

so, I appended FROM in my query

Answer: `TryHack3M_users`



## The End 
# I hope you find it useful.

