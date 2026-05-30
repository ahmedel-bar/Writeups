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

I then used ExifTool to inspect the file:

```bash
exiftool mystery
```

The output suggested that the file was intended to be a PNG image but contained structural corruption.

## Verifying the PNG Signature

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

## Investigating the PNG Structure

Running ExifTool again revealed the following warning:

```text
PNG image did not start with IHDR [x4]
```

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

## Identifying Additional Corruption

After fixing the `IHDR` chunk, ExifTool was able to parse more metadata from the image, including:

* Image Width: 1642
* Image Height: 1095

However, another warning appeared:

```text
Invalid PNG chunk size
```

To investigate further, I used:

```bash
pngcheck -v mystery.png
```

The output showed that the PNG structure was still corrupted after the `pHYs` chunk.

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

## Recovering the Flag

After repairing the corrupted PNG structure, the image opened successfully and revealed the hidden flag.

## Flag

```text
picoCTF{c0rrupt10n_1847995}
```































*the flag might be different from account to another*

Answer: `picoCTF{c0rrupt10n_1847995}`

# The end. I hope this has been helpful to you.
