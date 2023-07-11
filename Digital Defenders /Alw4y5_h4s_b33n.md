# Alw4y5_h4s_b33n (Digital Forensics) - Digital Defenders CTF 2023

## Description
> In the realm of digital forensics and incident response, a perplexing case unfolded that left investigators scratching their heads. A notorious cybercriminal was suspected of tampering with crucial data in an organization's database, but they left behind no trace of their activities. As the investigators combed through the digital evidence, they stumbled upon a series of seemingly innocuous files, including images of various landscapes. However, one image stood out—an aerial shot of a bustling wildscape. Intriguingly, the investigators noticed subtle inconsistencies in the dimention of the image, but their efforts to uncover a hidden message or altered data within the image proved fruitless. Can you help the investigators find the hidden message?

> **FLAG FORMAT:** bi0s{...}

## File(s)
1. chall.jpg

## Initial Takeaway
The very first thing that caught my eye was the incorrect spelling of 'dimension' (spelt as 'dimention' instead). There were two reasons this could be: to put emphasis on the word (which I realised was the case probably, later), or a password of some kind (what I initially assumed).
Apart from that, looking at the image didn't reveal anything. I thought I'd try using `file`, `exiftool`, and some steganography tools to start with, and then proceed accordingly.

## Execution
Using `file`, and `exiftool` didn't reveal any anomalies. The file also opened fine without looking corrupted, or without any errors.
I thought of using `binwalk` to detect files present within. This was a mistake though, as I spent hours trying to fix an extracted `.tiff` files off its errors.
On obviously being unsuccessful in that, I realised the typo I had spotted earlier.

Now reducing the dimensions didn't make any logical sense, so I chose to increase them. I found the SOF0 (0xFFC0) marker which points to the location where the dimensions of the image are stored in the file
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/0aebf727-7da4-4472-bc49-ff774bbafb14)

The first two bytes following the marker is the size segment. Following which is one byte storing the bits per pixel. After which 2 bytes are reserved for the height, followed by the width.
I initially tried increasing both values by some amount. The resultant image got corrupted. So, I stuck to only one dimenion. I tried changing the height value.
Finally I found the sweet spot to be around 750.

So I set the values to 1280x750
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/ca8ed3b6-ac5d-49b3-bf99-9280646a3562)

Opening the image thus gave the full picture (pun intended).
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/758063b0-0115-4243-a2a6-6a83b359b6f1)

  
**FLAG:** `bi0s{th3_dimention_tr1ck}`
