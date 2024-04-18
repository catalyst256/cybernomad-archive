---
title: "OSCP - Useful resources"
date: "2012-10-10"
categories: 
  - "archive"
---

Inspired by a conversation I had on twitter today with @Balgan (who has just started his OSCP adventure, so everyone wish him luck), and due to the fact that I've now managed to root all the boxes in the lab (thank you, thank you) I thought I would post some of the interweb based resources I've found useful during my adventure (without giving anything away about the content of the labs).

You probably already know most of these but here they are anyway..

1. Google - yes I know I keep going on about this "Google" thing, but think of it like this, unless you are a InfoSec God that can just look at a machine and get it to pop a shell (a bit like the Fonzie) then you aren't re-inventing the wheel and the chances are someone has already exploited what you are looking at.
    
2. [exploit-db.com](http://exploit-db.com) - Goes without saying that this should be your first port of call when looking for an exploit. I'm not keen on the search feature so see point 3.
    
3. `/pentest/exploits/exploitdb` - This is on your local copy of backtrack and has is a download of the exploit-db.com, exploit archive. I like the `./searchsploit` tool and you can grep the output etc. etc. You can either download the archive file from the website, or update it manually using wget and some bash magic. Or if you are lazy my update code is below:
    

`#!/bin/bash echo "Downloading latest exploit-db archive file" wget http://www.exploit-db.com/archive.tar.bz2 -O /tmp/archive.tar.bz2 echo "File successfully downloaded" echo "Decompressing archive file to /pentest/exploits/exploitdb/" tar -xvjf /tmp/archive.tar.bz2 -C /pentest/exploits/exploitdb/ echo "Decompression complete, reseting executable properties for files.csv" chmod +x /pentest/exploits/exploitdb/files.csv echo "Tidying up downloaded file" rm /tmp/archive.tar.bz2 echo "Update complete."`

1. [g0tmilk.blogspot.co.uk](http://g0tmi1k.blogspot.co.uk/) - g0mi1k's post about [basic linux privilege escalation](http://g0tmi1k.blogspot.co.uk/2011/08/basic-linux-privilege-escalation.html) was a live saver for me. Without it I would have been limited to rooting Windows boxes which lets face it can be a bit boring (sorry Mr Microsoft its just too easy).
    
2. [Got Meterpreter? Pivot!](http://pen-testing.sans.org/blog/2012/04/26/got-meterpreter-pivot) - This really helped me get my head around pivoting through networks, yes it's only doing it with Metasploit, but you never know when you might need to.
    
3. [SSH gymnastics with proxychains](http://pauldotcom.com/2010/03/ssh-gymnastics-with-proxychain.html) - Again another really useful blog post about other ways to tunnel, pivot (do a little dance) your way through networks.
    
4. [7 linux shells using built-in tools](http://lanmaster53.com/2011/05/7-linux-shells-using-built-in-tools/) - There is a link to this on g0tmi1k's blog post above but it's worth a mention by itself because well it's awesome and getting shell is never a bad thing.
    
5. [Pivoting into a network using PLINK and FPipe](http://exploit.co.il/hacking/pivoting-into-a-network-using-plink-and-fpipe/) - If you lean more towards Windows, then here is a good post about using the Windows-based tools to pivot. My personal preference was Linux but everyone is different.
    
6. [Corelan Team](https://www.corelan.be/) - If you want some light reading on exploit writing, check these guys out. Awesome content that even I can understand.
    
7. [CVE Details](http://www.cvedetails.com/) - This is a no brainer really, you can find all sorts of exploits (some of the less common ones) here and the nice thing is that down the bottom of the page for an exploit it will tell you if any Metasploit modules relating to it exist.
    

Well that's all I can think of, let me know if I've missed any obvious ones (no one is perfect remember).

Have fun..
