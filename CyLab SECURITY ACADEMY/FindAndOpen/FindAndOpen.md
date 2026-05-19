# FindAndOpen Writeup
## lab link [FindAndOpen](https://learn.cylabacademy.org/library?page=3&category=4)

## Scenario
```
Someone might have hidden the password in the trace file.

Find the key to unlock this file. This tracefile might be good to analyze.
```

Firstly, I downloaded the challenge files using `wget`. There were two files: `flag.zip` and `dump.pcap`.

I tried to unzip `flag.zip`, but it was protected with a password. Since I did not know the password, I opened the `dump.pcap` file in Wireshark to search for useful information.

<img width="1571" height="860" alt="0" src="https://github.com/user-attachments/assets/6b11970f-3ab4-4751-8c79-92879e9811bb" />

While analyzing the packets, I found readable ASCII text in the packet bytes section. One packet contained the message:

`Flying on Ethernet secret: Is this the flag`

<img width="1919" height="1020" alt="1" src="https://github.com/user-attachments/assets/ff0a2a75-eb7f-4623-a8c0-c069ef9c6100" />


This was only a hint. Then I found another message saying:

`Could the flag have been splitted?`

This told me that the secret was split across multiple packets.

<img width="1917" height="962" alt="2" src="https://github.com/user-attachments/assets/c78c5dba-d613-42ab-bca6-96e49fc204bf" />


I continued checking the packets and found several Base64-like strings. I copied the packet bytes as ASCII text and pasted the useful parts into CyberChef.

Copy -> ...as ASCII Text

<img width="1918" height="1018" alt="3" src="https://github.com/user-attachments/assets/f716db9f-6004-4222-a1c4-17ca40fd6d72" />

<img width="1919" height="679" alt="Screenshot 2026-05-19 184805" src="https://github.com/user-attachments/assets/8b74888c-75c4-40aa-b10c-700584017c0c" />


<img width="1919" height="837" alt="Screenshot 2026-05-19 184939" src="https://github.com/user-attachments/assets/fb02eaf6-c8fd-4599-835b-9759ae2251ec" />


<img width="1919" height="515" alt="Screenshot 2026-05-19 185003" src="https://github.com/user-attachments/assets/dbc1a27f-06ee-48fd-b60e-b7b974036f47" />

Some of the strings I found were:
```
iBwaWNvQ1RGe1C
AABBHHPJGTFRLK
VGhpcyBpcyB0aGUgc2VjcmV0OiBwaWNvQ1RGe1lZNERJTkdFTl9LZF8=
PBwaWUvQ1RGe1M
babakbjaASKBSACVVAVSDDSSSSDSKJBJS
PBwaWUvQ1RGe1M
```
In CyberChef, I used the `From Base64` operation and enabled `Remove non-alphabet chars`. The decoded output revealed the secret password:

`This is the secret: picoCTF{R34DING_LOKd`


<img width="1551" height="560" alt="Screenshot 2026-05-19 185106" src="https://github.com/user-attachments/assets/32054968-5620-4070-b5f7-5ecb8c135b30" />

I used this password to extract the zip file. This time, the extraction worked and produced a file called `flag`.

Finally, I opened the file using `cat flag` and got the final flag:

`picoCTF{R34DING_LOKd_f1l56_succ3ss_5ed3a878}`

<img width="1032" height="630" alt="Screenshot 2026-05-19 185645" src="https://github.com/user-attachments/assets/7e7fb853-a0dc-468b-9e89-a590b335f9c8" />





*the flag might be different from account to another*

Answer: `picoCTF{R34DING_LOKd_fil56_succ3ss_5ed3a878}`

# The end. I hope this has been helpful to you.



