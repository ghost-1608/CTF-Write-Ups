# brut3nf0rce (Digital Forensics) - Digital Defenders CTF 2023

## Description
> In the realm of digital forensics, a complex investigation unfolded involving a world class criminal engaging in a secret smuggle of sensitive information. As investigators pursued the trail, they discovered that the evidence of their illicit activities was concealed within a hidden file that is protected. Despite their expertise, the investigation team found themselves stymied by sophisticated encryption techniques and clever obfuscation that the file's password is not in rockyou.txt. Seeking an expert in the field, they turned to a skilled analyst renowned for their prowess in bhruteforcing. Armed with cutting-edge tools and an unwavering determination, the analyst delved into the digital labyrinth, meticulously analyzing the file as well as employing advanced techniques. Can you unravel the encryption where the key is less than 3 characters as mentioned by the criminal and the picture inside has a stegonography message hidden with one of the universal passwords. The success of the investigation rests in your hands as you navigate the intricacies of digital forensics to uncover the truth hidden within the virtual depths?

> **FLAG FORMAT:** bi0s{...}

## File(s)
1. chall.zip

## Initial Takeaway
The description was pretty clear in explaining the challenge. At first, we need to brute-force the zip file. Next, from the image, we need to use some forensics technique to uncover hidden data secured with some common password.

## Execution
I planned on using JohnTheRipper to crack the zip file.

First I converted the zip to john format
```bash
zip2john chall.zip > hash.txt
```

Next, I used john with an incremental mode for upto 3 character passwords (as mentioned in the description)
```bash
$ john --max-length=3 hash.txt
```
And,
```bash
$ john --show hash.txt
```
```
chall.zip/chall7.jpg:gz:chall7.jpg:chall.zip::chall.zip

1 password hash cracked, 0 left
```
  
Using the password, I unzipped the file. Out came a jpeg file
```
-rwxrwxrwx 1 ghost ghost 1074541 Jun 12 01:05 chall7.jpg
```

Apart from the customary `file` and related commands, I tried using `stegsolve` to no avail.

I finally tried `stegsolve`, and got success
```bash
$ stegseek chall7.jpg
```
```
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "batman1"
[i] Original filename: "secret.txt".
[i] Extracting to "chall7.jpg.out".
```

The extracted file was a text file, which contained the flag.

**FLAG:** `bi0s{bruting_satisfaction_for_real}`
