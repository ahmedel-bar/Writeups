# Hunter lab wrtieup 
Lab link [Hunter](https://cyberdefenders.org/blueteam-ctf-challenges/hunter/)
## Category : Endpoint Forensics
## Tools : 
- Useful cheat sheet ![Windows FOR heat sheet](Images/windowsFOR.pdf) 
- AccessData_FTK_Imager
- reistery explorer
- timeline explorer
- PECmd.exe
- SysTools Outlook PST Viewer
- skyperious
- ShellBags explorer
- Exiftool 
- JLECmd.exe


## scenario 
 ```
The SOC team got an alert regarding some illegal port scanning activity coming from an employee's system. The employee was not authorized to do any port scanning or any offensive hacking activity within the network. The employee claimed that he had no idea about that, and it is probably a malware acting on his behalf. The IR team managed to respond immediately and take a full forensic image of the user's system to perform some investigations.

There is a theory that the user intentionally installed illegal applications to do port scanning and maybe other things. He was probably planning for something bigger, far beyond a port scanning!

It all began when the user asked for a salary raise that was rejected. After that, his behavior was abnormal and different. The suspect is believed to have weak technical skills, and there might be an outsider helping him!

Your objective as a soc analyst is to analyze the image and to either confirm or deny this theory.
```


### Firstly, unzip the lab file then open FTK_Imager then select image file, after that select the image itself 

![](Images/1.png) 

### Q1: What is the computer name of the suspect machine? 

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

### Q2: What is the computer IP?

 network information can found in this hive path `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`
 
 or directely from available bookmarks 
 
 ![](Images/8.png)

Answer: `10.0.2.15`

### Q3: What was the DHCP LeaseObtainedTime?

 from the image above you can see the answer 
 
 Answer: `21/06/2016 02:24:12 UTC`

### Q4: What is the computer SID?
  SID is security identifier you can found it in  `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList`
  export SOFTWARE hive from FTK_imager and open it in RegisteryExplorer
 
  ![](Images/9.png)
 
  Answer: `S-1-5-21-2489440558-2754304563-710705792`

 ### Q5:What is the Operating System(OS) version?

 in the SOFTWARE hive and from bookmarks you can find `windows version information`
 
  ![](Images/10.png)
 
  Answer: `8.1`
 
  ### Q6: What was the computer timezone?
 
 the hive path that contain time zone info is `SYSTEM\CurrentControlSet\Control\TimeZoneInformation`
 or you can find it directely from bookmarks 
 
 ![](Images/11.png)
 
 I used info in this photo and then asked ai tool  
 
 ![](Images/12.png)
 
 Answer: `UTC-07:00`


### Q7: How many times did this user log on to the computer?

 you can find info related to users from `SAM` hive export it from FTK_Imager as you did in SYSTEM hive,
 then add it to registery explorer
 
 ![](Images/13.png) 
 
 Answer: `3`


### Q8: When was the last login time for the discovered account? Format: one-space between date and time

the answer in the photo above 

Answer: `2016-06-21 01:42`


### Q9: There was a “Network Scanner” running on this computer, what was it? And when was the last time the suspect used it? Format: program.exe,YYYY-MM-DD HH:MM:SS UTC 

 by get deeper in image on FTK_imager i found this 
 
 ![](Images/14.png) 
 
 as you already know zenmap is a network scanner tool 
 now we need to know the last time it's used
 the files provide it is `prefetchfiles` 
 you can found this folder in this path `root\windows\prefetch`
 we need now to export the whole folder 
 
 ![](Images/15.png) 
 
 after that we need to analyze this folder 
 we can analyze it using `PECmd.exe` from EZtools 
 use this command `PECmd.exe -d Prefetch --csv <folder to save in> --csvf <file name>`
 
 ![](Images/16.png) 
 
 open the CSV file using `timelineexplorer` from EZtools and search for zenmap 
 
 ![](Images/17.png) 
 
 Answer: `zenmap.exe,2016-06-21 12:08:13 UTC`


### Q10: When did the port scan end? (Example: Sat Jan 23 hh:mm:ss 2016)

 export this file to know info about last scan 
 
 ![](Images/18.png) 
 
 ![](Images/19.png) 
 
 go to this path and export this file 
 
 ![](Images/20.png) 
 
 I tried to open it with browser but it didin't open, so I opened it with vscode you can open it with any text editor 
 
 ![](Images/21.png) 
 
 Answer: `Tue Jun 21 05:12:09 2016`


### Q11: How many ports were scanned?

 ![](Images/22.png) 
 
 Answer: `1000`


### Q12: What ports were found "open"?(comma-separated, ascending)

 ![](Images/23.png) 
 
 Answer: `22,80,9929,31337` 


### Q13: What was the version of the network scanner running on this computer?

 ![](Images/24.png) 
 
 Answer: `7.12`


### Q14: The employee engaged in a Skype conversation with someone. What is the skype username of the other party? 

 SKYPE data are in this path `C:\Users\<Username>\AppData\Roaming\Skype\` 
 open this path and export the `main.db` file
 
 ![](Images/25.png) 
 
 then open the main.db using `skyperious`
 
 ![](Images/26.png) 
 
 Answer: `linux-rul3z`


### Q15: What is the name of the application both parties agreed to use to exfiltrate data and provide remote access for the external attacker in their Skype conversation?
 
 in the photo above you can find the answer 
 
 Answer: `Teamviewer`


### Q16: What is the Gmail email address of the suspect employee?

 emails are sended by outlook and you can find outlook data in this path
 `C:\Users\<Username>\AppData\Local\Microsoft\Outlook\`
 
 I searched in this path and found nothing, so I tried to search in other palces and found this under the document folder 
 
 ![](Images/27.png) 
 
 go ahead and open this file using `SysTools Outlook PST Viewer`
 
 ![](Images/28.png)  
 
 Answer: `ehptmsgs@gmail.com`


### Q17: It looks like the suspect user deleted an important diagram after his conversation with the external attacker. What is the file name of the deleted diagram?

 ![](Images/29.png)  
 
 Answer: `home-network-design-networking-for-a-single-family-home-case-house-arkko-1433-x-792.jpg`


### Q18: The user Documents' directory contained a PDF file discussing data exfiltration techniques. What is the name of the file?

 in the documnet folder you will see more than .pdf file, so you need to export all of them and see the content 
 
 ![](Images/30.png)  
 
 If you open `Ryan_VanAntwerp_thesis.pdf`, you will find that 
 
 ![](Images/31.png)  
 
 Answer: `Ryan_VanAntwerp_thesis.pdf`



### Q19: What was the name of the Disk Encryption application Installed on the victim system? (two words space separated)
 
 installed programmers are found in these to paths: `SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`
                                                    `SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths`
 I searched in both but found nothing related to Disk Encryption 
 
 so, I tried to search in Programmer Files (x86) and i found this file
 
 ![](Images/32.png)  
 
  by searching in this file I found that he deleted some files and apps by bcwipe to remove evidence 
 
  ![](Images/33.png)  
 
 Answer: `Crypto Swap`



### Q20: What are the serial numbers of the two identified USB storage?

 the hive that contains USB identification `SYSTEM\CurrentControlSet\Enum\USBSTOR`
 
  ![](Images/34.png)
 
  Answer: `07B20C03C80830A9,AAI6UXDKZDV8E9OU`
 


### Q21: One of the installed applications is a file shredder. What is the name of the application? (two words space separated)
  shredder is used to damage and delete files
 
  you can find it in the `Uninstall.log` or from the same photo you can see the `wipeList.txt`
 
  ![](Images/35.png)
  ![](Images/36.png)
 
  Answer: `Jetico BCWipe`



### Q22: How many prefetch files were discovered on the system?

 Like what we did in question 9, I opened the PE.CSV in timeline explorer and see how many .pf file
 
 ![](Images/37.png)
 
 
 Answer: `174`



### Q23: How many times was the file shredder application executed?

 you can also find the answer by searching for bcwipe.exe in pe.csv using timeline explorer
 
 ![](Images/38.png)
 
 
 Answer: `5`


### Q24: Using prefetch, determine when was the last time ZENMAP.EXE-56B17C4C.pf was executed?

 search for the keyword above in pe.csv, too 
 
 ![](Images/39.png)
 
 Answer: `06/21/2016 12:08:13 PM`



### Q25: A JAR file for an offensive traffic manipulation tool was executed. What is the absolute path of the file?

 I didn't find anyhting in the pe.csv, so I turn back and tried to find any .jar and I found Burpsuite in download folder
 
 
 ![](Images/40.png)
 
 Answer: `C:\Users\Hunter\Downloads\Burpsuite_free_v1.7.03.jar` 


### Q26: The suspect employee tried to exfiltrate data by sending it as an email attachment. What is the name of the suspected attachment?

 turn back to the outlook data and systool 
 
 ![](Images/41.png)
 
 Answer: `Pictures.7z`



### Q27: Shellbags shows that the employee created a folder to include all the data he will exfiltrate. What is the full path of that folder?

 shellbags info are found in these two hives `C:\Users\<---->\AppData\Local\Microsoft\Windows\userclass.dat` 
 and `C:\Users\<username>\ NTUSER.DAT` but I didn't found NTUSER.DAT in the image 
 
 so i Exported USRCLASS.DAT 
 
 ![](Images/42.png)
 
 then open ShellBags explorer from EZtools
 
 ![](Images/43.png)
 
 Answer: `C:\Users\Hunter\Pictures\Exfil`



### Q28: The user deleted two JPG files from the system and moved them to $Recycle-Bin. What is the file name that has the resolution of 1920x1200?

 the $Recycle-Bin include 2 photos but as you can see there were corupted, so I searched in the image for an original photo
 
 ![](Images/44.png)
 
 
 after searching I found that with the same resolution and for the same cat 
 
 ![](Images/45.png) 
 
 to provide double check, use `exiftool` to see  metadata of the photo
 
 ![](Images/46.png) 
 
 Answer: `ws_Small_cute_kitty_1920x1200.jpg`


### Q29: Provide the name of the directory where information about jump lists items (created automatically by the system) is stored?
 
 Windows jump lists: show us the recently opened files in that application.
 
 it can be found in two places
 `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\ AutomaticDestinations OR customdestination`
 
 the one is created automatically by the system is AutomaticDestinations
 
 Answer: `AutomaticDestinations`


### Q30: Using JUMP LIST analysis, provide the full path of the application with the AppID of "aa28770954eaeaaa" used to bypass network security monitoring controls. 

 export  AutomaticDestinations and analyze it using `JLECmd.exe` from EZtools
 
 ![](Images/47.png) 
 
 ![](Images/48.png) 
 
 open the csv file using timeline Explorer and search for aa28770954eaeaaa 
 
 after searching I found nothing, so I turned back and exported customdestination 
 and used the same command `JLECmd.exe -d CustomDestinations --csv <folder to save in> --csvf <file name>`,
 then open the out file using timeline explorer and searched for aa28770954eaeaaa 
 
 ![](Images/49.png) 
 
 Answer: `C:\Users\Hunter\Desktop\Tor Browser\Browser\firefox.exe`



# The end. I hope this has been helpful to you.
































