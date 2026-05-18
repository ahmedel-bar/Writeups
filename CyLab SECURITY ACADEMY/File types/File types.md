# File types Writeup
## lab link [File types](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can.
```

First, I downloaded the challenge file using wget.

<img width="1919" height="511" alt="Screenshot 2026-05-18 175359" src="https://github.com/user-attachments/assets/b1567c78-d69b-40a5-8415-9db178309c67" />


After downloading it, I tried to open it as a PDF file, but it did not open correctly. So, I checked the file type using the file command.

<img width="1919" height="908" alt="Screenshot 2026-05-18 175014" src="https://github.com/user-attachments/assets/35a1272e-3eb0-4e2f-8f0b-e1a07b970f3e" />

The output showed that the file was not a real PDF file. It was actually a shell script.

Then, I copied the file to a new file called Flag.sh and gave it execute permission using chmod.

<img width="915" height="480" alt="Screenshot 2026-05-18 180007" src="https://github.com/user-attachments/assets/d732a007-ffae-405b-bb69-af3ab636f912" />

After that, I ran the script. The script extracted a new file called flag.

I checked the new file using the file command and found that it was an ar archive. So, I extracted it using the ar command.

<img width="1910" height="888" alt="Screenshot 2026-05-18 180712" src="https://github.com/user-attachments/assets/0af63afa-23a2-40d8-a97e-b8f650987e3f" />


After extracting it, I checked the file type again. It was a cpio archive, so I extracted it using cpio.

<img width="1549" height="266" alt="Screenshot 2026-05-18 180738" src="https://github.com/user-attachments/assets/34525801-8741-4397-960c-2c3e361613b0" />


Then, I continued checking the file type after every extraction using the file command. Each time, the file was compressed or archived using a different format.

I found several layers, such as bzip2, gzip, lzip, LZ4, LZMA, lzop, and XZ.

For each layer, I used the suitable tool to decompress it. For example, I used bunzip2 for bzip2, gunzip for gzip, lzip for lzip, lz4 for LZ4, xz for LZMA and XZ, and lzop for lzop.

<img width="1333" height="614" alt="Screenshot 2026-05-18 180841" src="https://github.com/user-attachments/assets/f12ddb77-6cdc-46b9-85d9-e4d56b9c749b" />


<img width="1584" height="432" alt="Screenshot 2026-05-18 181124" src="https://github.com/user-attachments/assets/7e193c9c-10f3-4186-ba72-73d3f5aeeb97" />


<img width="866" height="263" alt="Screenshot 2026-05-18 181453" src="https://github.com/user-attachments/assets/f845d16b-42dd-40db-8352-b8cb00066454" />


<img width="1181" height="620" alt="Screenshot 2026-05-18 181814" src="https://github.com/user-attachments/assets/cbeb15e5-df1e-470c-a5c5-0749d0353d66" />


<img width="1101" height="607" alt="Screenshot 2026-05-18 182210" src="https://github.com/user-attachments/assets/972ea4c5-c1fd-4442-9d0f-00f6cb08f2fa" />




After extracting all layers, I finally got an ASCII text file.

When I opened it using cat, I found a hexadecimal string.

Finally, I decoded the hexadecimal string using xxd with the -r and -p options.

This converted the hex string back to readable text and revealed the flag.

<img width="1013" height="508" alt="Screenshot 2026-05-18 182242" src="https://github.com/user-attachments/assets/6ef8e129-ff5c-44fc-8f5c-ae12bf37fdb8" />




*the flag might be different from account to another*

Answer: `picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_79b01c26}`

# The end. I hope this has been helpful to you.
