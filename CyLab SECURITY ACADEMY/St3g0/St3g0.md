# St3g0 Writeup
## lab link [St3g0](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
Download this image and find the flag.
```

First, I downloaded the PNG image using `wget` and confirmed that it existed in the directory. 
Then, I used `exiftool` to check its metadata. The image appeared to be a normal PNG file, and the metadata did not contain the flag.

<img width="1877" height="846" alt="0" src="https://github.com/user-attachments/assets/0463b49a-65ad-48ed-a587-2910595be82a" />

Then, I opened the image to inspect it visually. It appeared to be a normal picoCTF logo image with no visible flag.

<img width="1912" height="972" alt="1" src="https://github.com/user-attachments/assets/86cb20ee-1528-44c3-851f-a62da9b24f66" />

After that, I used `zsteg` to analyze the PNG image for hidden data. The tool found the flag hidden in the least significant bits of the RGB channels.

<img width="1816" height="271" alt="2" src="https://github.com/user-attachments/assets/362ef574-9db4-4173-8cf3-d048f3bc8c6c" />




*the flag might be different from account to another*

Answer: `picoCTF{7h3r3_15_n0_5p00n_a9a181eb}`

# The end. I hope this has been helpful to you.
