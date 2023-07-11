# bl4ckscr33n_ex3cuti0n (Digital Forensics) - Digital Defenders CTF 2023

## Description
> In the realm of digital forensics, a perplexing challenge emerged where investigators struggled to recover crucial blackscreen crash after a pragram being run and we want the history from volatile memory. Despite their expertise, the intricacies of the task proved formidable, leaving them unable to retrieve vital information needed for a critical investigation. Recognizing the importance of this missing piece, the investigation team turned to a skilled analyst known for their expertise in memory forensics. With a deep understanding of memory structures and relentless determination, analyst delved into the memory dump, meticulously examining the artifacts left behind. Can you recover the missing data and help the investigation team solve the case?

> **FLAG FORMAT:** bi0s{...}

> **HINTS:**
> 1. Can you retrieve all the commands that the hacker executed?

## File(s)
1. chall.tar.xz

## Initial Takeaway
Extracting the contents of the tarball gave one file `chall.raw`. The description gives the information that the provided file is a memory dump.
Going ahead by this information, I planned on using volatility (Python2). The plan was to run an `imageinfo` first to figure out the profile, then perhaps run a file scan or a process list scan, and then build upon that.

## Execution
Before proceeding with using volatility though, I decided to run a customary `file` command on the file.
```bash
$ file chall.raw
```
This returned
```
chall.raw: Windows Event Trace Log
```
So, now I was equiped with the information that it was a memory dump of a Windows machine.

Starting with volatility, I ran the image info scan.
```bash
$ vol.py -f chall.raw imageinfo
```
After waiting for some time, I got the following output
```
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x86_23418, Win7SP0x86, Win7SP1x86_24000, Win7SP1x86
                     AS Layer1 : IA32PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/ghost/ctf/digital_defenders/actual/blackscreen_execution/chall.raw)
                      PAE type : No PAE
                           DTB : 0x185000L
                          KDBG : 0x82935c28L
          Number of Processors : 4
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0x82936c00L
                KPCR for CPU 1 : 0x80d9c000L
                KPCR for CPU 2 : 0x8c01e000L
                KPCR for CPU 3 : 0x8c059000L
             KUSER_SHARED_DATA : 0xffdf0000L
           Image date and time : 2023-06-11 14:25:53 UTC+0000
     Image local date and time : 2023-06-11 07:25:53 -0700
```
The main reason for running this command was to get the profile information. Out of the 4 suggested profiles, I went with the latest and most popular "Win7SP1x86".
  
Now, one piece of crucial information is regarding which OS we're dealing with. Unix-based OSes have a terminal history file typically stored somewhere on the system.
However, the image given is from Windows, specifically Windows 7. The primary command line on older versions of Windows was CMD, and CMD doesn't store the command history in a local file.
The reason this piece of information is important is the hint provided. There's some sort of clue in the command history. Since files wouldn't have helped me, I chose to go with looking up the processes list.

Running a scan of running processes in the image (saving the output because of the size of the file)
```bash
$ vol.py -f chall.raw --profile=Win7SP1x86 pslist > pslist.txt
```
Out of the processes in the list, two caught my eye.
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/08ef5f71-3488-47f7-a460-afda1106dcb3)
  
and
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/1e0dca20-25f7-4dbc-b616-b4291d240b8f)

But out of the two, CMD seemed to be the most promising.

So, I memdumped the process
```bash
$ vol.py -f chall.raw --profile=Win7SP1x86 memdump -p 980 -D out/
```
The dump was generated inside the folder `out`. So, I went there and the first thing I though might be useful was see the hex view of the file. I renamed the file to `cmd.dmp` for my convinience and opened it in hexedit.
```bash
$ hexedit cmd.dmp
```

There wasn't anything sticking out except for one detail, which I didn't realise till slightly after. Since there wasn't anything in particular to attract my attention, I closed hexedit.
Next, I proceeded to check the strings in the file just in case. However, what I did notice running the `strings` command is that not all the strings that I saw in hexedit were visible.
So I re-opened hexedit to see what the deal was, and realised this:-
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/7f1903a6-5503-4716-9d5d-2292f398feb0)
  
Certain strings like the one above, weren't continuously laid, so I looked at the hex values

![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/8b6e338c-e1e4-4dce-89fe-e14c7984dd69)

These strings were layed out in such a manner that there was a null byte after every character. So, grepping any coherant string from the output of the `strings` command wouldn't yield you anything.
So, I quickly designed a small python script allowing me to convert regular text to bytes with null bytes after every character so that I could search the file.
```python3
#!/usr/bin/python3

import sys

if __name__ == '__main__':
    l = list(sys.argv[1].encode())
    i = 0
    while i < len(l) - 1:
        l[i] = hex(l[i])[2:]
        l.insert(i + 1, '00')
        i += 2
    l[-1] = hex(l[-1])[2:]
    print(''.join(l))
```
  
Using this, I encoded the text "bi0s{"
```bash
$ ./script.py bi0s{
```
Which returned
```
62006900300073007b
```

Finally, I re-opened `cmd.dmp` in hexedit, and searched for the bytes `62006900300073007b`.

And there she was
  
![image](https://github.com/ghost-1608/CTF-Write-Ups/assets/64543976/8bf5c192-dc9b-4bf4-befb-b1515e0c363b)

I copied the part containing the flag thorugh the terminal and got rid of the hex bytes. The only thing remaining was to remove the dots from the ASCII text, which I did by running it through a `replace()` in python
```python3
>>> 'b.i.0.s.{m.3.m.0.r.y._.s.u.p.r.e.m.4.c.y.}'.replace('.', '')
bi0s{m3m0ry_suprem4cy}
```

**FLAG:** `bi0s{m3m0ry_suprem4cy}`
