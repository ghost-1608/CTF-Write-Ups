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
  
(Disclaimer: Here onwards I've truncated the value of the actual numbers since they're too big to display here most of the times)
  
And, the contents of cipher.txt
```
n1 : 209129100507960...
n2 : 238724269325638...
n3 : 623265548625163...
c : 3007656756310367...
```

After studying a the code a little and a bit on RSA encryptions, I came to take the assumption that `n3` might be a number greater than the multiple of multiple prime numbers.
  
I refered the internet a bit more on how I could solve such a problem and came up with the following steps that I'd approach:-
* Find the GCD of `n1` and `n2`, call it `x`.
* Figure out the relation of `n3` with `n1`, `n2` and `x`.
* And figure out the relation between `phi` and whatever I solved so far

## Execution
At first I found out the GCD of `n1` and `n2`, and found the relation of it with each
__
Next, I (after finding the GCD of `a`, `b`, `x` with `n3`), determined that I could write
```
n3 = a * b * x * P
```
Or,
```
P = n3 // (a * b * x)
```
  
Next is to check if either `a`, `b`, or `x` are factors of `P`
```python3
print(math.gcd(P, a))
print(math.gcd(P, b))
print(math.gcd(P, x))
```
```
12471612875027441617719...
14236548924427800678189...
1
```

Looking at the output, I realised that `a` and `b` were factors of `P`. Now, I re-wrote `n3` as
```
n3 = a^2 * b^2 * x * Q
Q = n3 // ((a * b * x) * (a * b))
```

Again verifying if either `a`, `b`, or `x` are factors of `Q`
```python3
print(math.gcd(Q, a))
print(math.gcd(Q, b))
print(math.gcd(Q, x))
```
```
12471612875027441617719...
1
1
```

Yet again, `a` and `b` were the factors of `Q`. Finally, let `n3` be
```
n3 = a^3 * b^2 * x * R
R = n3 // ((a * b * x) * (a * b) * a)
```

This time, running the very same GCD code with each `a`, `b`, and `x` all return `1`.
This means `n3 = ((a * b * x) * (a * b) * a)* d` (Taking `R` as `d`)

Finally, after some amount of coding (and unethically bypassing errors), I found out that all combinations give the same flag. Nevertheless, here's the code
```python3
import math

n1 = 2091291005079603183...
n2 = 2387242693256388391...
n3 = 6232655486251632785...
c = 30076567563103670244...

def egd(e, phi):
    k = 1
    while (1 + k * phi) % e: k += 1
    return (1 + k * phi) // e

x = math.gcd(n1, n2)
a = n1 // x
b = n2 // x
d = n3 // ((x * a * b) * (a * b) * a)
e = 65537
v = ['(a-1)*(x-1)', '(a-1)*(b-1)', '(a-1)*(d-1)', '(x-1)*(b-1)', '(x-1)*(d-1)', '(b-1)*(d-1)']
for phi in v:
    for i in [n1]:
        try:
            f = egd(e, eval(phi))
            pt = hex(pow(c, f, i))[2:]
            b = bytes.fromhex(pt)
            asciis = bt.decode("ASCII")
            print(f'{asciis}')
        except:
            pass
```
```
flag{1s_17_r34lly_an_RSA???}
```

**FLAG:** `flag{1s_17_r34lly_an_RSA???}`
