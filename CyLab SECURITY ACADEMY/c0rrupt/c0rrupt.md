# c0rrupt Writeup
## lab link [c0rrupt](https://learn.cylabacademy.org/library?page=5&category=4)

## Scenario
```
We found this file. Recover the flag.
```


First, I downloaded the file and checked its type:

```bash
file mystery
```

The output indicated that the file type could not be identified.

<img width="1919" height="796" alt="Screenshot 2026-05-30 152726" src="https://github.com/user-attachments/assets/c9ef2feb-78d8-42d0-b557-3244c4e51001" />



Using a hex editor (HxD), I compared the file header against the standard PNG signature from Gary Kessler's file signature database.

Expected PNG signature:

```text
89 50 4E 47 0D 0A 1A 0A
```

The file contained:

```text
89 65 4E 34 0D 0A B0 AA
```

Several bytes in the PNG signature had been modified.

After replacing the corrupted bytes with the correct PNG signature, I saved the file as:

```text
mystery.png
```

<img width="1257" height="902" alt="Screenshot 2026-05-30 153417" src="https://github.com/user-attachments/assets/aaa4f67a-a7e2-4890-bc69-0b44f8560617" />

<img width="1272" height="528" alt="Screenshot 2026-05-30 153456" src="https://github.com/user-attachments/assets/c0aebce1-9036-4982-b312-b33f61e24bc6" />

<img width="1875" height="798" alt="Screenshot 2026-05-30 153436" src="https://github.com/user-attachments/assets/d2d133e4-da2a-4d45-ab99-6477037bb355" />



I then used ExifTool to inspect the file:

```bash
exiftool mystery
```

The output suggested that the file was intended to be a PNG image but contained structural corruption.





## Investigating the PNG Structure

Running ExifTool again revealed the following warning:

```text
PNG image did not start with IHDR [x4]
```

<img width="1033" height="451" alt="Screenshot 2026-05-30 154704" src="https://github.com/user-attachments/assets/e895bf3a-90bd-41d8-8d35-dbcdc8a8a35b" />

<img width="1418" height="366" alt="Screenshot 2026-05-30 155031" src="https://github.com/user-attachments/assets/7bc78503-c3a2-491f-8fa6-6f8d4d55e162" />

This indicated that the first PNG chunk was corrupted.

According to the PNG specification, the first chunk after the PNG signature must be:

```text
IHDR
```

which corresponds to:

```text
49 48 44 52
```

However, the file contained:

```text
43 22 44 52
```

I manually corrected the chunk type in HxD.

<img width="1311" height="728" alt="Screenshot 2026-05-30 155056" src="https://github.com/user-attachments/assets/4f244b21-7cfa-443a-a548-445dcf0c4f18" />


## Investigating PNG Metadata

While examining the file with ExifTool, I noticed an unusual value in the PNG metadata:

```text
Pixels Per Unit X : 2852132389
Pixels Per Unit Y : 5669
Pixel Units       : meters
```

The `Pixels Per Unit X` value appeared abnormally large compared to the Y value, which suggested that the metadata might have been intentionally modified.

To verify whether this corruption was affecting the image, I inspected the `pHYs` chunk in HxD and located the following bytes:

```text
AA 00 16 25 00 00 16 25 01
```

The first four bytes represent the X pixel density, while the next four bytes represent the Y pixel density.

I modified the X value to match the Y value:

```text
00 00 16 25 00 00 16 25 01
```

After saving the file, ExifTool reported:

```text
Pixels Per Unit X : 5669
Pixels Per Unit Y : 5669
```


<img width="1167" height="874" alt="Screenshot 2026-05-30 154357" src="https://github.com/user-attachments/assets/1a1771b8-a473-4dc5-83c2-17a9ee712c37" />


<img width="1196" height="909" alt="Screenshot 2026-05-30 154405" src="https://github.com/user-attachments/assets/fd388b13-3da9-40f7-80a9-d8ca09fb3895" />

However, the image still failed to open and `pngcheck` continued to report structural errors. This indicated that the abnormal pixel density values were not the root cause of the corruption, so I continued investigating other PNG chunks.


## Identifying Additional Corruption

After fixing the `IHDR` chunk, ExifTool was able to parse more metadata from the image, including:

* Image Width: 1642
* Image Height: 1095

However, another warning appeared:

```text
Invalid PNG chunk size
```

<img width="1242" height="664" alt="Screenshot 2026-05-30 155242" src="https://github.com/user-attachments/assets/1447ec0e-3212-4d8c-b287-b31688ef63e2" />


To investigate further, I used:

```bash
pngcheck -v mystery.png
```

The output showed that the PNG structure was still corrupted after the `pHYs` chunk.


<img width="1308" height="314" alt="Screenshot 2026-05-30 160238" src="https://github.com/user-attachments/assets/0e0165d8-1807-434a-8582-7d99d8fc50fa" />

Examining the file in HxD revealed that the next chunk length field had been intentionally modified:

```text
AA AA FF A5
```

This produced an invalid chunk length.

The chunk type was also corrupted:

```text
AB 44 45 54
```

Instead of the valid PNG image data chunk:

```text
49 44 41 54
```

which represents:

```text
IDAT
```

I corrected the corrupted bytes and saved the file.

<img width="1254" height="609" alt="Screenshot 2026-05-30 160203" src="https://github.com/user-attachments/assets/871ada06-909d-4c9b-9849-4a2dae03d59c" />



After repairing the corrupted PNG structure, the image opened successfully and revealed the hidden flag.

<img width="1066" height="570" alt="Screenshot 2026-05-30 160226" src="https://github.com/user-attachments/assets/b43f545d-2a2a-46c1-ad55-f1783c61242b" />




*the flag might be different from account to another*

Answer: `picoCTF{c0rrupt10n_1847995}`

# The end. I hope this has been helpful to you.
