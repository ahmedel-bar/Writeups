# Sleuthkit Apprentice Writeup
## lab link [Sleuthkit Apprentice](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
Download this disk image and find the flag.

Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.
```


First, I downloaded the disk image using `wget`. The downloaded file was compressed as `.gz`, so I decompressed it using `gunzip`.

After that, I used `mmls` to inspect the partition table of the disk image. 
The output showed a DOS partition table with multiple partitions, including Linux partitions and a Linux swap partition.

<img width="1613" height="578" alt="0" src="https://github.com/user-attachments/assets/c246fba6-8042-4985-9ca1-222cc8a96c0a" />


After identifying the partitions with `mmls`, I used `fls` with the first Linux partition offset `2048` to inspect its files.
This partition mainly contained boot-related files, so I moved to the other Linux partition.

Then, I used `fls -o 360448 -r` to recursively list the files in the second Linux partition and filtered the output for `.txt` files.
This revealed two interesting files: a deleted `flag.txt` file and a `flag.uni.txt` file.

Since `flag.uni.txt` was still allocated, I used `icat` with its inode number to extract its contents from the second Linux partition.
The file contained the flag.


<img width="1252" height="847" alt="1" src="https://github.com/user-attachments/assets/d528a176-fa6e-46f2-9476-5252d7931ff0" />




*the flag might be different from account to another*

Answer: `picoCTF{by73_5urf3r_3497ae6b}`

# The end. I hope this has been helpful to you.
