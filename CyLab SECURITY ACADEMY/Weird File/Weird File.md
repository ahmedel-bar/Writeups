# Weird File Writeup
## lab link [Weird File](https://learn.cylabacademy.org/library?page=5&category=4)

## Scenario
```
What could go wrong if we let Word documents run programs? (aka "in-the-clear").
```

Firstly, I downloaded the challenge file using `wget`.

<img width="1919" height="476" alt="0" src="https://github.com/user-attachments/assets/4e3bce3a-83c6-4f62-b5e5-6a4c0c97daba" />

I noticed that the file extension was .docm, which indicates a Microsoft Word document that may contain VBA macros.

To analyze the macros without opening the file in Microsoft Word, I used `olevba`.


<img width="1919" height="997" alt="1" src="https://github.com/user-attachments/assets/a7d9e2e0-1bec-48b4-b211-a85181248fc1" />


The output showed a VBA macro inside ThisDocument.cls. I found an AutoOpen() function, which means the macro executes automatically when the document is opened.

Inside the macro, I noticed a suspicious Shell() command running Python code.

There was a Base64-encoded data.

So, I copied the encoded text and decoded it using base64 and found the flag.


<img width="683" height="125" alt="2" src="https://github.com/user-attachments/assets/35d41a4d-b557-4cb4-9f66-bef0f6b03532" />



*the flag might be different from account to another*

Answer: `picoCTF{m4cr0s_r_d4ng3r0us}`

# The end. I hope this has been helpful to you.
