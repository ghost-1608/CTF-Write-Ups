# Ghost (Web) - Digital Defenders CTF 2023

## Description
> While I was immersed in a thrilling session of Call of Duty, I suddenly got this amazing idea for a website. I'm quite excited to show it to you. Sometimes, unexpected sparks of inspiration can arise during gaming sessions, and this time, it resulted in a web development concept that I believe has real potential. With bated breath, I presented my creation to the world, eager to showcase its unique blend of functionality and security. As I navigated through the intricacies of the website, I couldn't help but feel a surge of pride. Drawing from the adrenaline-fueled atmosphere of gaming, I incorporated cutting-edge measures to ensure ironclad file protection. My creation promised a sanctuary where users could confidently entrust their precious data, shielded by layers of state-of-the-art encryption and impenetrable security features. So, without further ado, let me share my creation with you. I'm eager to hear your thoughts and see if my gaming-inspired idea can truly make an impact in the world of web development. I promise you, your files will be very secure.
>
> The flag is in _/tmp/flag.txt_

> **FLAG FORMAT:** bi0s{...}

## The Website
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/850271a8-a164-4009-b926-44d5777a44ce)
The website is looks pretty straightforward with a file upload option in sight.

I decided to see how it operates, and so uploaded a random png file.
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/44d135e4-e61b-43c3-b5fe-db894b9b5cec)
  
On hitting "Upload", there's a text that appears at the bottom of the page
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/b4798878-a9a8-4f1b-ba66-4c21b3d2a7b2)
  
On following the instructions and heading to `/uploads/<filename>`, we can access our file again.
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/c124d686-f2ca-45f5-8f6d-099bff1e78cb)

## Initial Takeaway
The first thing that caught my eye was of course the little text (baddly) placed right above the file select box.
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/11d79eb7-8557-4b20-abf7-15a8966a04bb)

Nothing immediate struck me on seeing this message, so I moved on. The next point of interest obviously was the fact that a file can be accessed, seemingly unmodified, after uploading it.
I also tried with text files, and got prompted by the 'Save File' dialog.
This gave me the idea that perhaps some script could be executed by uploading it to the website. Another point to note was the address of the page. The index page had a `.php` extension.
So perhaps I could exploit the site with some PHP script.

## Execution
Too test out if I could indeed exploit the file upload vulnerability with PHP, I wrote a Hello World program in PHP.
```PHP
<?php
echo "Hello, World!";
?>
```

Naming the file `script.php`, I uploaded the file on the website. Sure enough, on navigating to `/uploads/script.php`, I saw the familiar greeting
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/7c47588e-f165-463f-833b-d1e0d4f2a935)
  
In the description, they had mentioned that the flag is in `/tmp/flag.txt`. So, at this point, there was only one small change to make to the script.

I wrote a new script, very similar to the previous, but with the addition of a "payload".
```PHP
<?php
$output = shell_exec('cat /tmp/flag.txt');
echo "<pre>$output</pre>";
?>
```
  
The script executes the shell command `cat /tmp/flag.txt` and stores the output in the variable `output`. Then finally it prints it out in the browser.
After uploading and submitting the script, the output gives us what we need
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/bd76ec4f-c204-4bbe-bce9-c0719a62626c)

The only thing that remained unsolved was the meaning of the tiny message above the file selection box. Maybe it's there coz of how insane the price of COD games are.

**(DISCLAIMER):** One thing to note is that according to the information released by the organisers, some CTF challenges use dynamic flags. Hence, the flag I solved the challenge with and the flag at the time of writing this write-up are different.
The method however remains the same.
  
**FLAG:** `bi0s{RtU0M0F7GtX2Ho4r0FmCLA==}`
