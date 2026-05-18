# Enhance! Writeup
## lab link [SEnhance!](https://learn.cylabacademy.org/library?page=4&category=4)

## Scenario
```
Download this image file and find the flag.
```

Firstly, I downloaded the file using `wget` and opened it in the browser to see its content. 

<img width="1835" height="349" alt="Screenshot 2026-05-18 184130" src="https://github.com/user-attachments/assets/6850baf9-5da5-4866-83da-6324582b1f0e" />


<img width="1905" height="1012" alt="Screenshot 2026-05-18 184509" src="https://github.com/user-attachments/assets/eda91586-2788-44a1-8cf6-1c71d10364c5" />



Then, I checked the file type using the `file` command. The output showed that the file was an SVG image and also ASCII text.
Since SVG files are XML-based, I opened the file with `cat` to inspect the source code.

<img width="1248" height="872" alt="Screenshot 2026-05-18 184721" src="https://github.com/user-attachments/assets/01d951bf-8cea-4017-b2ae-b4f8dac6017c" />



Inside the SVG file, I found a hidden `<text>` element containing multiple `<tspan>` tags. 
The flag characters were split across these tags. 
The text was hidden because the font size was extremely small and the fill color was white,making it invisible in the image viewer.

<img width="1882" height="844" alt="Screenshot 2026-05-18 184746" src="https://github.com/user-attachments/assets/dba65264-9946-4b4f-bdb2-900fba71a98e" />

```
<tspan>p </tspan>
<tspan>i </tspan>
<tspan>c </tspan>
<tspan>o </tspan>
<tspan>C </tspan>
<tspan>T </tspan>
<tspan>F { 3 n h 4 n </tspan>
<tspan>c 3 d _ a a b 7 2 9 d d }</tspan>
```
```
p
i
c
o
C
T
F { 3 n h 4 n
c 3 d _ a a b 7 2 9 d d }
```





*the flag might be different from account to another*

Answer: `picoCTF{3nh4nc3d_aab729dd}`

# The end. I hope this has been helpful to you.
