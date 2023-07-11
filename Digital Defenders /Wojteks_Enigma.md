# Wojtek's Enigma (Cryptography) - Digital Defenders 2023

## Description
> Intriguingly, amidst the chaos of World War II, an extraordinary tale unfolds. The Japanese forces, driven by their relentless pursuit of intelligence, managed to intercept a communication from Syria. Little did they know that within this intercepted communication lay the encrypted secrets of a distinguished client known as Wojtek .
>
> WOjtek's data was caught in the web of international espionage. His important data had been carefully encoded using a machine. The Japanese forces, recognizing the significance of this encrypted information, realized they had stumbled upon something extraordinary. The intricacy of the encryption posed a formidable challenge, leaving the Japanese forces in awe of the level of sophistication employed to protect Wojtek's data.

> **Resource** : [How Alan Turing Cracked The Enigma Code](https://www.iwm.org.uk/history/how-alan-turing-cracked-the-enigma-code)

> **FLAG FORMAT:** flag{}

## Files
1. chall.txt

## Initial Takeaway
On reading the description, and further on viewing the files, it became very clear this was a cipher to break with an enigma machine.

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
