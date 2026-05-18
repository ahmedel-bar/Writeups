# Packets Primer Writeup
## lab link [Packets Primer](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
Download the packet capture file and use packet analysis software to find the flag.
```


First, I opened the packet capture file in Wireshark to inspect the network traffic. 
I noticed a short TCP conversation between `10.0.2.15` and `10.0.2.4` using destination port `9000`.

After selecting the TCP packet that contained data, I inspected the packet bytes in the lower pane.
The ASCII view showed the flag directly inside the TCP payload.

<img width="1919" height="962" alt="Screenshot 2026-05-18 152227" src="https://github.com/user-attachments/assets/44fa4557-246d-4e8d-9f56-463811b3dc90" />


Then, I followed the TCP stream in Wireshark to view the full TCP conversation. The flag was visible in clear text inside the stream.


<img width="1277" height="786" alt="Screenshot 2026-05-18 152434" src="https://github.com/user-attachments/assets/7340961e-e42e-43a5-89a4-9ca215fbf841" />

<img width="1842" height="980" alt="Screenshot 2026-05-18 152350" src="https://github.com/user-attachments/assets/d8e9842f-0d58-4e5a-afc8-743fc2282827" />


The extracted flag contained extra spaces, so I used CyberChef to remove the whitespace and recover the correct flag format.


<img width="1532" height="525" alt="Screenshot 2026-05-18 152507" src="https://github.com/user-attachments/assets/17cf7120-cb9b-4506-a259-fa7f7c0d01f4" />




*the flag might be different from account to another*

Answer: `picoCTF{p4ck37_5h4rk_01b0a0d6}`

# The end. I hope this has been helpful to you.
