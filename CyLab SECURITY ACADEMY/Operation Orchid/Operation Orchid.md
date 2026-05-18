# Operation Orchid Writeup

## lab link [Operation Orchid](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
Download this disk image and find the flag.

Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.
```

First, I downloaded the compressed disk image using `wget`. Since the file was compressed as `.gz`, I decompressed it using `gunzip`.

After that, I ran `strings` on the disk image and searched for the keyword `pico`. 
The output showed many unrelated matches containing the word `pico`, but none of them appeared to be the flag.

<img width="1919" height="737" alt="Screenshot 2026-05-18 153659" src="https://github.com/user-attachments/assets/2a700bbd-095f-456e-9b2e-8779da5b9f23" />

After that, I used `mmls` to inspect the partition table of the disk image. 
The output showed multiple partitions, including two Linux partitions and one Linux swap partition.

I first checked the Linux partition starting at offset `2048` using `fls`, 
but it mainly contained boot-related files.


<img width="1164" height="661" alt="1" src="https://github.com/user-attachments/assets/ddc98b33-e507-4b57-bb38-cca00393fcda" />


After identifying the second Linux partition at offset `411648`, 
I recursively listed its files using `fls` and searched for text files. 
This revealed two files: `flag.txt` and `flag.txt.enc`.

I used `icat` to read `flag.txt.enc`, but its content was not readable and started with `Salted__`. 
This indicated that the file was encrypted using OpenSSL with a salt, 
so I needed to find the password or encryption command before decrypting it.

<img width="1383" height="605" alt="2" src="https://github.com/user-attachments/assets/a221534f-2bc6-4476-9fbf-513cc44c8338" />

After finding the encrypted file, I searched the file system for history files and found `.ash_history` in the root directory.
I extracted it using `icat` and reviewed the commands that had been executed on the system.

The history file revealed the OpenSSL command used to encrypt the flag:
`openssl aes256 -salt -in flag.txt -out flag.txt.enc -k unbreakablepassword1234567`

This command showed both the encryption algorithm and the password, which could be used to decrypt `flag.txt.enc`.

<img width="1234" height="830" alt="3" src="https://github.com/user-attachments/assets/92f978f1-9de2-4b34-9547-188abafe021f" />


After finding the encryption command and password in `.ash_history`, I extracted the encrypted file using `icat` and saved it as `flag.txt.enc`.

Then, I used OpenSSL with the same cipher and password to decrypt the file.
 Finally, I used `cat` to read `flag.txt` and recovered the flag.

<img width="1825" height="545" alt="4" src="https://github.com/user-attachments/assets/e89547a6-2708-4c9f-b8b4-d4b5ae35fe11" />




*the flag might be different from account to another*

Answer: `picoCTF{h4un71ng_p457_1d02081e}`

# The end. I hope this has been helpful to you.
