# Operation Oni Writeup
## lab link [Operation Oni](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
Download this disk image, find the key and log into the remote machine.

Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.
```

First, I launched the challenge instance to get the additional details. 
After launching it, the challenge provided the disk image download link and the SSH command for the remote machine.

<img width="648" height="567" alt="0" src="https://github.com/user-attachments/assets/f0eb0c80-63c6-493b-bb52-1435206ee8ff" />

<img width="664" height="573" alt="1" src="https://github.com/user-attachments/assets/2cbe2c17-0223-4c58-ac78-d5c9e9d38036" />

Then, I downloaded the compressed disk image using `wget`. The file was downloaded as `disk.img.gz`, 
so I decompressed it using `gunzip` to get the raw disk image for analysis.

<img width="1911" height="455" alt="2" src="https://github.com/user-attachments/assets/9f16102c-9051-47c2-a0db-c9c3bbd15477" />

After that, I used `mmls` to inspect the partition table and found two Linux partitions.
The first one was only contains boot files

<img width="1138" height="708" alt="3" src="https://github.com/user-attachments/assets/85b446e3-4189-445c-ba25-860797ac798b" />

I searched recursively for SSH-related files using `fls` and `grep`, and I found a hidden `.ssh` directory.
Then, I listed the contents of this directory and found two files: `id_ed25519` and `id_ed25519.pub`.

<img width="790" height="877" alt="4" src="https://github.com/user-attachments/assets/d504b7a3-ff86-474f-b7cf-5d50c403480c" />


The `id_ed25519` file was the private SSH key, while `id_ed25519.pub` was the public key. 

<img width="793" height="258" alt="5" src="https://github.com/user-attachments/assets/e9831939-7508-4819-8cc6-780cb0eabad1" />

I extracted the private key using `icat` and saved it into a file named `key_file`.

Then, I changed the key permissions using `chmod 600` because SSH requires private keys to have secure permissions.

Finally, I used the extracted private key with the SSH command provided by the challenge instance to log into the remote machine.
After logging in, I listed the files, found `flag.txt`, and used `cat` to read the flag.


<img width="1202" height="804" alt="6" src="https://github.com/user-attachments/assets/77efb288-37fc-46bf-91bb-b7f3fdccaa2d" />





*the flag might be different from account to another*

Answer: `picoCTF{k3y_5l3u7h_339601ed}`

# The end. I hope this has been helpful to you.
