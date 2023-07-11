# IS IT AN RSA??? (Cryptography) - Digital Defenders 2023

## Description
> The enigmatic Cyber Ninja clan devised an extraordinary encryption technique known as the RSA Art of Shadows.
>
> In this ancient cryptographic art, every element is intricately interconnected, embodying the essence of their mystical power. Legend speaks of a forbidden connection between n3, the sacred modulus of the third level, and the elusive shuriken factor, s.
>
> The Cyber Ninjas now question the resilience of their moduli amidst the shadows of uncertainty. Unravel this enigma and reveal the extent of the moduli's vulnerability to achieve enlightenment

> **FLAG FORMAT:** flag{}

> **HINT:**
> 1. Try taking a look at the bit lengths.

## Files
1. chall.py
2. cipher.txt

## Initial Takeaway
Judging by the title, the challenge obviously has something to do with the RSA encryption, and since the way it's framed, the challenge is to figure out if it really forms an RSA encryption or not.

The contents of chall.py
```python3
from Crypto.Util.number import getPrime

p = getPrime(1024)
q = getPrime(1024)
r = getPrime(1024)
s = getPrime(1024)

m = #REDACTED
n1 = p*q
n2 = q*r
n3 = #REDACTED
e = 65537
c = pow(m,e,n3)

with open("cipher.txt","w") as f1:
    f1.write("n1 : {n1}\nn2 : {n2}\nn3 : {n3}\nc : {c}")
```

And, the contents of cipher.txt
```

```

## Execution
`chall.txt` had the following details
```
𝑬𝒏𝒔𝒖𝒓𝒆 𝒕𝒉𝒆 𝒄𝒍𝒊𝒆𝒏𝒕'𝒔 𝒔𝒆𝒄𝒖𝒓𝒊𝒕𝒚 𝒂𝒕 𝒂𝒍𝒍 𝒕𝒊𝒎𝒆𝒔
 
agin{afkkxf_7e3_ib4d}

𝑴𝒐𝒅𝒆𝒍 : 𝑴3
𝑹𝒆𝒇𝒍𝒆𝒄𝒕𝒐𝒓 : 𝑼𝑲𝑾 𝑩

𝑹𝑶𝑻𝑶𝑹 1 : 𝑽𝑰
𝑷𝒐𝒔𝒊𝒕𝒊𝒐𝒏 : 1𝑨
𝑹𝒊𝒏𝒈 : 2𝑩

𝑹𝑶𝑻𝑶𝑹 2 : 𝑰
𝑷𝒐𝒔𝒊𝒕𝒊𝒐𝒏 : 3𝑪
𝑹𝒊𝒏𝒈 : 4𝑫

𝑹𝑶𝑻𝑶𝑹 3 : 𝑰𝑰𝑰
𝑷𝒐𝒔𝒊𝒕𝒊𝒐𝒏 : 5𝑬
𝑹𝒊𝒏𝒈 : 6𝑭

𝑷𝑳𝑼𝑮𝑩𝑶𝑨𝑹𝑫 : 𝒃𝒒 𝒄𝒓 𝒅𝒊 𝒆𝒋 𝒌𝒘 𝒎𝒕 𝒐𝒔 𝒑𝒙 𝒖𝒛 𝒈𝒉
```
So, I headed over to [](https://cryptii.com/pipes/enigma-machine) to solve this challenge.

I entered the values as mentioned
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/2f5b8fdc-3cae-4e24-8c81-218836b722a8)

And thus got the flag.

**FLAG:** `flag{wojtek_7h3_be4r}`
