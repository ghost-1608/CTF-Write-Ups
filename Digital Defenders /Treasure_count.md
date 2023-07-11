# Treasure count (Cryptography) - Digital Defenders 2023

## Description
> Embark on a captivating journey known as the Treasure Count, where hidden riches and encrypted mysteries await. The challenge centers around AES in CTR mode, an encryption algorithm that guards the path to untold treasures. As a curious explorer, you delve into the realm of cryptography, deciphering each encrypted block to uncover the hidden message. With each step, you unravel the intricate workings of AES in CTR mode, gaining insights into its power and nuances. Navigate through a series of cryptographic puzzles, cracking codes and decrypting fragments along the way. The final test lies within a file named "flag.png," holding the key to unlocking the true location of the Treasure Count.
>
> **Reference:** ()[http://www.libpng.org/pub/png/spec/1.2/PNG-Structure.html https://youtu.be/6EbyCGrdKh8]

> **FLAG FORMAT:** flag{}

> **HINT:**
> 1. What if the counter doesn't count at all? Would it be easier then?

## Files
1. chall.py
2. chall.txt

## Initial Takeaway
```python3
import os
from Crypto.Cipher import AES

KEY =

class CustomCounter(object):
    def __init__(self, value=os.urandom(16), step_up=False):
        self.value = value.hex()
        self.step = 1
        self.stup = step_up

    def increment(self):
        if self.stup:
            self.new_value = hex(int(self.value, 16) + self.step)
        else:
            self.new_value = hex(int(self.value, 16) - self.stup)
        self.value = self.new_value[2:]
        return bytes.fromhex(self.value.zfill(32))

    def __repr__(self):
        self.increment()
        return self.value


def encrypt():
    cipher = AES.new(KEY, AES.MODE_ECB)
    ctr = CustomCounter()
    output = []
    with open("//home//disco//Desktop//flag.png", 'rb') as file:
        block = file.read(16)
        while block:
            keystream = cipher.encrypt(ctr.increment())
            xored = [x^y for x, y in zip(block, keystream)]
            output.append(bytes(xored).hex())
            block = file.read(16)

    return {"encrypted": ''.join(output)}

with open('/home/disco/Desktop/chall.txt','w') as chall:
    chall.write(encrypt()['encrypted'])
```
Reading the above code, the `encrypt()` function reads the `flag.png` mentioned in the description. It does this with AES-ECB with a custom counter. So, after studying AES ecnryption a little bit, I realised I had to get the key, and counter values during encryption.

## Execution
After going through the given code, I formulated what I had to do.
I'd need to XOR the values obtained from the counter and ciphertext block.

So, I went and made a decrypting script
```python3
from Crypto.Cipher import AES

pnghead = b'\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR'
l = []

with open('chall.txt', 'r') as f:
    block = bytes.fromhex(f.read(32))
    xored = [x^y for x, y in zip(block, pnghead)]
    x = bytes(xored)

with open('chall.txt', 'r') as f:
    block = bytes.fromhex(f.read(32))
    while block:
        xored = [x^y for x, y in zip(block, x)]
        l.append(bytes(xored))
        block = bytes.fromhex(f.read(32))

with open('out.png', 'wb') as f:
    f.write(b''.join(l))
```

On running, `out.png` was produced, which on opening gave:-
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/a4978d3d-9884-4d22-9ea5-487a7a0171e1)


**FLAG:** `flag{7h3_0n3P1eC3_15_R34L!!}`
