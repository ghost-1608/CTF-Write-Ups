# Pr0j3ct_M3t4 - Digital Defenders CTF

## Description
> In the realm of digital forensics, a captivating case unfolded, where a meticulous examination of metadata became the key to uncovering a concealed crime. As investigators delved into the digital artifacts, their attention was drawn to the hidden secrets nestled within the metadata fields. Timestamps, geolocation coordinates, and authorship details held the potential to unveil the truth. Through methodical analysis, a significant discovery emerged—a series of covert conversations, seemingly innocuous at first glance but containing coded references to illicit activities, is there anything on the image that was shared?

> **FLAG FORMAT:** bi0s{...}

## Files
1. chall.jpg

## Initial Takeaway
Reading the description, I could conclude the challenge had something to do with the metadata of the file. I planned on trying the usual route `file` followed by using some metadata related tools like `exiftool`.

## Execution
I running `file`
```
$ file chall.jpg
```
Gave
```shell
chall.jpg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16,
comment: "Ymkwc3tleDFmX2Q0dDR9Cg==", baseline, precision 8, 1200x900, components 3
```
Right here, I noticed a comment in the file `Ymkwc3tleDFmX2Q0dDR9Cg==`. It obviously stood out to me as a base64 encoded text.
So, I decoded it using the terminal
```
$ echo Ymkwc3tleDFmX2Q0dDR9Cg== | base64 --decode
```
Which gave
```
bi0s{ex1f_d4t4}
```

**FLAG:** `bi0s{ex1f_d4t4}`
