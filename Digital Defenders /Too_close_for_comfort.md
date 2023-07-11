# Too close for comfort (Cryptography) - Digital Defenders 2023

## Description
> In the realm of Numeria, where numbers hold the key to ancient secrets, a legend whispers of a challenge called "Too Close for Comfort." It tells of a cryptic code that guards a hidden chamber, where twin primes, magically entwined, hold the power to unlock unparalleled wisdom and boundless treasures.
>
> Numeria, once a realm of harmony and enlightenment, now finds itself in the grip of uncertainty. The Twin Chamber, a vault rumored to contain the answers to life's deepest mysteries, has remained elusive, its entrance guarded by a code steeped in complexity. But hope resurfaces as the seekers of knowledge discover a glimmer of possibility within the enigmatic challenge, "Too Close for Comfort."
>
> Within the cryptic code lies a hidden flag—an enigmatic phrase that embodies the essence of the Twin Chamber's secrets. It is a symbol of wisdom and prosperity, waiting to be unveiled by those daring enough to traverse the intricate path of encryption.
>
> Armed with their cryptographic tools, the seekers embark on a quest that explores the delicate dance between twin primes. The code, a fusion of ancient mathematical techniques and modern encryption algorithms, guides them on a journey through the enigmatic realm of closely spaced primes.

> **FLAG FORMAT:** flag{}

## Files
1. chall.py
2. output.txt

## Initial Takeaway
The description mentions "twin primes" and their relations. So, I assumed the challenge was going to be related.
```python3
from gmpy2 import next_prime,invert
from Crypto.Util.number import *

flag = "flag{REDACTED}"
flag = bytes_to_long(flag.encode())
p = getPrime(512)
q = next_prime(p)
n = p*q
e = 65537
phi = (p-1)*(q-1)
d = invert(e,phi)
c = pow(flag,e,n)
print("n : ",n)
print("c : ",c)
```
Looking at the code, we see `p` is a random 512 bit prime, `q` is the prime immediately after it. Since we're looking for primes, my initial thought was to look it up on [Alpertron](https://www.alpertron.com.ar/).
Once we have that figured out, the rest is simple.

(Disclaimer: Here onwards I've truncated the value of the actual numbers since they're too big to display here most of the times)
  
output.txt contents:-
```
n :  512626214217629185260936...
c :  363527081252374307647600...
```
## Execution
From the website, I got the factors
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/5c7639c2-12b2-4c59-85ec-f56d44a61134)

Luckily both the values (`p` and `q`) were obtained

Now, all I had to do was to calculate the plaintext. For that I used [](https://www.dcode.fr/rsa-cipher)
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/31cfccfa-0595-404a-bd92-404cfeceb070)

![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/632c45e9-a46f-4c3a-b527-3e09be4af91d)


**FLAG:** `flag{Cl0s3_pr!m3s_c4n_3as!1y_b3_s01v3d_us!ng_f3rm47}`
