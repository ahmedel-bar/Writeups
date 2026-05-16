# Timeline 0 Writeup
## lab link [Timeline 0](https://learn.cylabacademy.org/library?page=2&category=4)

## Scenario
```
Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format.

```

First, I used `wget` to download the image file, then decompressed it with `gunzip`.
![99](Images/0.png)

First, I used `mmls` to examine the partition layout, but it did not reveal any useful information. 
Then, I used `fls` to list the directories inside the image file.

![99](Images/1.png)


After that, I created a MAC timeline to analyze the file system activity chronologically.

![99](Images/2.png)

I used `Timeline Explorer` from Eric Zimmerman's tools to analyze the `.csv` timeline file. 
During the analysis, I found a suspicious entry with suspicious date associated with this inode.

![99](Images/3.png)


Therefore, I used `icat` with the inode number to extract and view the file contents.
The extracted output was Base64-encoded. I decoded it using `base64 -d`, then wrapped the decoded result in the `picoCTF{}` flag format.
![99](Images/4.png)





*the flag might be different from account to another*

Answer: `picoCTF{71m311n3_0u7113r_h3r_43a2e7af}`


# The end. I hope this has been helpful to you.
