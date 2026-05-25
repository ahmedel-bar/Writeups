# Disk, disk, sleuth! I Writeup
## lab link [Disk, disk, sleuth! I](https://learn.cylabacademy.org/library?page=5&category=4)

## Scenario
```
Use srch_strings from the sleuthkit and some terminal-fu to find a flag in this disk image.
```

Firstly, I downloaded the compressed disk image using `wget`.

Then I decompressed the image using `gunzip`.

<img width="1908" height="456" alt="0" src="https://github.com/user-attachments/assets/6ec904d7-23c4-46c7-a61b-a29463f91f2c" />

To inspect the partition table inside the disk image, I used `mmls`.

The output showed a Linux partition starting at sector `2048`.

Based on scenario, I searched directly for printable strings inside the image file using `strings`,
filtered the results with `grep` and I got the flag

<img width="916" height="344" alt="1" src="https://github.com/user-attachments/assets/20194fcc-46c5-4bad-b9d2-d86dd0e9ab71" />



*the flag might be different from account to another*

Answer: `picoCTF{f0r3ns1c4t0r_n30phyt3_5e56e786}`

# The end. I hope this has been helpful to you.
