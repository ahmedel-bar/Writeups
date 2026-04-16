# Hunter lab wrtieup 
## Category : Endpoint Forensics 
## Tools : 
- AccessData_FTK_Imager


## scenario 
The SOC team got an alert regarding some illegal port scanning activity coming from an employee's system. The employee was not authorized to do any port scanning or any offensive hacking activity within the network. The employee claimed that he had no idea about that, and it is probably a malware acting on his behalf. The IR team managed to respond immediately and take a full forensic image of the user's system to perform some investigations.

There is a theory that the user intentionally installed illegal applications to do port scanning and maybe other things. He was probably planning for something bigger, far beyond a port scanning!

It all began when the user asked for a salary raise that was rejected. After that, his behavior was abnormal and different. The suspect is believed to have weak technical skills, and there might be an outsider helping him!

Your objective as a soc analyst is to analyze the image and to either confirm or deny this theory.


### Firstly, open FTK_Imager then select image file, after that select the image itself 

![](Images/1.png) 

#### Q1: What is the computer name of the suspect machine? 

computer name can found in this hive `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`

so, we need to export software hive you will found it in this path `root\windows\system32\config` 

remeber, you need to extract all SYSTEM files like SYSTEM, SYSTEM.LOG 

![](Images/2.png)

open the SYSTEM and SYSTEM.LOG file using `RegisteryExplorer` form EZtools 

![](Images/3.png)
![](Images/4.png)
![](Images/5.png)

now we need to go to the path hive above 
![](Images/6.png)

OR you can found it in available bookmarks in RegisteryExplorer

![](Images/7.png)

Answer: `4ORENSICS`

#### Q2: What is the computer IP?

network information can found in this hive path `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`

or directely from available bookmarks 

![](Images/8.png)

Answer: `10.0.2.15`

#### Q3: What was the DHCP LeaseObtainedTime?

from the image above you can see the answer 

Answer: `21/06/2016 02:24:12 UTC`

#### Q4: What is the computer SID?
 SID is security identifier you can found it in  `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList`
 export SOFTWARE hive from FTK_imager and open it in RegisteryExplorer

 ![](Images/9.png)

 Answer: `S-1-5-21-2489440558-2754304563-710705792`

 #### Q5:What is the Operating System(OS) version?

in the SOFTWARE hive and from bookmarks you can find `windows version information`

 ![](Images/10.png)

 Answer: `8.1`

 #### Q6: What was the computer timezone?

the hive path that contain time zone info is `SYSTEM\CurrentControlSet\Control\TimeZoneInformation`
or you can find it directely from bookmarks 

![](Images/11.png)

I used info in this photo and then asked ai tool  

![](Images/12.png)

Answer: `UTC-07:00`


#### Q7: How many times did this user log on to the computer?

you can find info related to users from `SAM` hive export it from FTK_Imager as you did in SYSTEM hive,
then add it to registery explorer

![](Images/13.png) 

Answer: `3`









