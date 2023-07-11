# bl1ndf0ld (Digital Forensics) - Digital Defenders CTF 2023

## Description
> In the realm of digital forensics and incident response, an intriguing case came to light. A renowned hacker was accused of orchestrating a cyber attack on a high-profile organization. As investigators delved into the evidence, a peculiar picture sent by the accused piqued their curiosity. It appeared to be a simple landscape photograph, devoid of any obvious hidden messages or steganographic techniques. Hours turned into days, as the team meticulously analyzed every pixel, employed cutting-edge algorithms, and explored a myriad of steganographic possibilities. Despite their relentless efforts, the image seemed to hold no secret payload, leaving the investigators perplexed. The case challenged their assumptions, pushing them to consider alternative methods of communication beyond conventional steganography. With the case reaching a critical juncture, the team realized that the true answer might lie beyond the digital realm. A meticulous examination of the hacker's online activities and social connections revealed a complex network of encrypted messages exchanged through an obscure chat platform, leading to the breakthrough they desperately sought. Sometimes, the absence of hidden messages can reveal the most vital clue, redirecting the investigation towards unexplored avenues.

> **FLAG FORMAT:** bi0s{...}

## File(s)
1. bl1ndf0ld.png

## Initial Takeaway
Reading the description it became clear that this challenge didn't involve any "usual" tools for digital forensics. So, I thought maybe unlike the previous challenges, this one wouldn't focus on data, but more on steganography.

## Execution
Tried doing the usual `file` and `exiftool` to start with. Nothing unusual there.
Since they told this challenge was "different", I thought of trying out `stegsolve`, seeing if I could find anything.

So I launched `stegsolve`
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/cd08f275-33ca-42e8-a2bd-d34e5a583238)

Just hitting the right button (to move to the next filter) did the trick!

![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/18c34a6d-88f1-4d48-aeb5-118e400920c1)
    
**FLAG:** `bi0s{7h3_c00l_plan3_stegan0gr4phy}`
