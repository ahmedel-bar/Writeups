# Sleuthkit Intro Writeup
## lab link [Sleuthkit Intro](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
Download the disk image and use mmls on it to find the size of the Linux partition. Connect to the remote checker service to check your answer and get the flag.

Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.
```


First, I downloaded the file using `wget` and decompressed it with `gunzip`. 
Then, based on the challenge scenario, I used `mmls` to identify the Linux partition and determine its size.

<img width="1456" height="655" alt="0" src="https://github.com/user-attachments/assets/88b7406e-c1c0-45e6-b479-1a1d75457a32" />


After that, I launched the challenge instance to obtain the `nc` connection details. 
Finally, I connected to the service, submitted the Linux partition size, and received the flag.

<img width="655" height="599" alt="1" src="https://github.com/user-attachments/assets/7983b54b-b1ec-404b-952c-04866ba38b85" />

<img width="847" height="730" alt="2" src="https://github.com/user-attachments/assets/8ad4ffe7-1788-44a7-b7be-7ae493db37e3" />

<img width="1046" height="187" alt="3" src="https://github.com/user-attachments/assets/8713eb85-dc11-44d1-b78f-9dd8e29b1745" />



*the flag might be different from account to another*

Answer: `picoCTF{mm15_f7w!}`

# The end. I hope this has been helpful to you.

