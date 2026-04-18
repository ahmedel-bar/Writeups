# Silent Breach Lab 
lab link [Silent Breach](https://cyberdefenders.org/blueteam-ctf-challenges/silent-breach/)

## Scenario
    The IMF is hit by a cyber attack compromising sensitive data. Luther sends Ethan to retrieve crucial information from a compromised server.
    Despite warnings, Ethan downloads the intel, which later becomes unreadable.
    To recover it, he creates a forensic image and asks Benji for help in decoding the files.

## Tools
 - FTK Imager 
 - text editor
 - DB browser (SQLite)
 - HxD


first you need to extract lab file using any uncompression tool like winrar using password `cyberdefenders.org`

after that, open the image file using FTK_image 

 ![FTK](Images/1.png)
 
#### Q1: What is the MD5 hash of the potentially malicious EXE file the user downloaded? 
  open the image on FTK_Imager and go to the Download folder, you will find only one .exe file
  export hash list 

 ![](Images/2.png)

 open the hash list file using any text editor 

 ![](Images/3.png)

 Answer: `336A7CF476EBC7548C93507339196ABB`
  


#### Q2: What is the URL from which the file was downloaded?
 
 open the file .exe itself, you will find Zone.Identifier file export it and see its content 

 ![](Images/4.png)

 ![](Images/5.png)
 
 Answer: `http://192.168.16.128:8000/IMF-Info.pdf.exe`


#### Q3: What application did the user use to download this file?

 you need to search in all browsers history to see which one used to download these files 

 edge history:    `C:\Users\<username>\Appdata\Local\Microsoft\Edge\User Data\Default\history`
 Chrome history:  `C:\Users\<username>\Appdata\Local\googel\chrome\User Data\Default\history`
 FireFox history: `C:\Users\<Username>\AppData\Roaming\Mozilla\Firefox\Profiles\<ProfileFolder>\places.sqlite`

 go ahead and export all of them, but we won't find firefox history 

 ![](Images/6.png)

 then open file with `DB browser (SQLite)`
 
 at Browse data choose urls 
 
 I found the url related to app downloded in edge history

  ![](Images/7.png)

  Answer: `Microsoft Edge`




#### Q4: By examining Windows Mail artifacts, we found an email address mentioning three IP addresses of servers that are at risk or compromised. What are the IP addresses?

  open link in the lab 
    
  ![](Images/8.png)

  we will find this 

  ![](Images/9.png)
   ` Users\<user>\Appdata\Local\Packages\microsoft.windowscommunicationsapps_8wekyb3d8bbwe\LocalState\`
  go ahead and export this file from its location 

  ![](Images/10.png)
   
   open this file unsing `HxD`

   

#### Q5: By examining the malicious executable, we found that it uses an obfuscated PowerShell script to decrypt specific files. What predefined password does the script use for encryption?
 
 extract the .exe file from Q1 then user a strings tool to extract all strings in it. 

  I 
  ![](Images/11.png) 
