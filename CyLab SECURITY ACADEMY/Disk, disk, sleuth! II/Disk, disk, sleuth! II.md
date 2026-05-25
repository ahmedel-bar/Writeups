# Disk, disk, sleuth! II Writeup
## lab link [Disk, disk, sleuth! II](https://learn.cylabacademy.org/library?page=5&category=4)

## Scenario
```
All we know is the file with the flag is named down-at-the-bottom.txt...
```

Firstly, I downloaded the compressed disk image using `wget`.

Then I decompressed the image using `gunzip`.

<img width="1902" height="462" alt="0" src="https://github.com/user-attachments/assets/32618451-f092-45e1-bcdc-cf0258b99340" />

To inspect the partition table inside the disk image, I used `mmls`.
The output showed a Linux partition starting at sector `2048`.

<img width="1129" height="720" alt="1" src="https://github.com/user-attachments/assets/e81623dc-69d2-407f-a4bd-aa2c40df7345" />

So, I used the offset 2048 with fls to list the files and directories inside the filesystem.


then, I recursively search in the directories and files using `fls` and grep for filename given in scenario. 

To extract the file contents, I used `icat` with the inode number.

<img width="1021" height="422" alt="2" src="https://github.com/user-attachments/assets/8cc2e155-5bc9-4bd7-a269-6e1b211d3640" />

The file contents displayed the flag characters in ASCII art style.

After removing the extra formatting and combining the characters, I recovered the flag:

<img width="1142" height="272" alt="3" src="https://github.com/user-attachments/assets/f2de3694-0f83-465f-8c99-5bc59e546bef" />



*the flag might be different from account to another*

Answer: `picoCTF{f0r3ns1c4t0r_n0v1c3_4bd721f2}`

# The end. I hope this has been helpful to you.
