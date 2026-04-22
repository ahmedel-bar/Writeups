# Sysinternals lab writeup 
lab link [Sysinternals](https://cyberdefenders.org/blueteam-ctf-challenges/sysinternals/)
## Category : Endpoint Forensics
## Scenario
```
A user thought they were downloading the SysInternals tool suite and attempted to open it,
but the tools did not launch and became inaccessible. Since then,
the user has observed that their system has gradually slowed down and become less responsive.
```
## Tools 
  - FTK imager
  - registry explorer
  - timeline explorer
  - amchaceparser.exe
  - virus total
  - notebook.exe
    

first you need to extract lab file using any uncompression tool like winrar using password `cyberdefenders.org`
then open the image file in FTK_Imager 

file >>>> Add evidence item
![ftk](Images/1.png)

then select image file 
![ftk](Images/2.png)


### Q1: What was the malicious executable file name that the user downloaded?

go ahead and search for user download
I found it in user public 

![ftk](Images/3.png)

Answer: `SysInternals.exe`


### Q2: When was the last time the malicious executable file was modified?

form the same page you can find that 

![ftk](Images/4.png)

convert 9 PM. to 21 to match answer format

Answer: `2022-11-15 21:18`


### Q3: What is the SHA1 hash value of the malware?

you can find sha1 value in Amchace.hve 
Amchace : `C:\Windows\AppCompat\Programs\Amcache.hve`

go ahead and export it 

export it and its .log files

![ftk](Images/5.png)

parse this file using `AmcacheParser.exe` from eric tools

use this command 
`AmcacheParser.exe -f Amcache.hve --csv  <directory to save out>`

the out as you can see

![ftk](Images/6.png)

open files with `timeline explorer` form eric tool
and search for sysinternal

I found the answer in the file named `20260422113454_Amcache_UnassociatedFileEntries.csv`

![ftk](Images/7.png)


Answer: `fa1002b02fc5551e075ec44bb4ff9cc13d563dcf`



### Q4: Based on the Alibaba vendor, what is the malware's family?

search in  [Virus total](https://www.virustotal.com/) using hash value from previuos question

as the report below
[VT Report](https://www.virustotal.com/gui/file/72e6d1728a546c2f3ee32c063ed09fa6ba8c46ac33b0dd2e354087c1ad26ef48)

you can find that 

![vt](Images/8.png)

Answer: `Rozena`


### Q5: What is the first mapped domain's Fully Qualified Domain Name (FQDN)?

from the same report in the previous question and in the relation tab 
you can see the next 

![vt](Images/9.png)

the domain that detected before as malicious

Answer: `www.malware430.com`



### Q6: The mapped domain is linked to an IP address. What is that IP address?

I searched for this in virus total but found no thing 

so, I turned back to search on iamge file to search in `windows host file` 

which map ip with domain name 

windows host file in this path : `C:\Windows\System32\drivers\etc\hosts`

go ahead and export it 

![vt](Images/15.png)

then open it with any text editor, you can find the next 

![vt](Images/16.png)


Answer: `192.168.15.10`






### Q7: What is the name of the executable dropped by the first-stage executable?
Using the Virus total report above 

naviagte to behavior tab

![vt](Images/18.png)

![vt](Images/17.png)

Answer: `vmtoolsIO.exe`






### Q8: What is the name of the service installed by 2nd-stage executable?

from the same photo above you can see the answer 

![vt](Images/19.png)

Answer: ` VMwareIOHelperService`











# The end, I hope you found this useful. 



































































