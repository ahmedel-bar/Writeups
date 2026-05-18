# Sleuthkit Intro Writeup
## lab link [Sleuthkit Intro](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
Download the disk image and use mmls on it to find the size of the Linux partition. Connect to the remote checker service to check your answer and get the flag.

Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.
```

Firstly, I downloaded the challenge file using `wget`. The downloaded file was named `anthem.flag.txt`.

After that, I opened the file using the `cat` command to inspect its content. 
The file contained a long text titled "ANTHEM" with many parts, so I searched inside the file for the flag format.

<img width="1918" height="903" alt="Screenshot 2026-05-18 190527" src="https://github.com/user-attachments/assets/e510f651-074c-4abb-813a-fa44a7b399ce" />


I used `grep` with the keyword `pico` to find any text related to the flag.

<img width="750" height="83" alt="Screenshot 2026-05-18 190552" src="https://github.com/user-attachments/assets/8a9e5b04-b11d-4da2-83da-4f8a44c144cd" />



*the flag might be different from account to another*

Answer: `picoCTF{gr3p_15_@w3s0m3_4c479940}`

# The end. I hope this has been helpful to you.
