---
title: "sniffMyPackets (Beta) - Released!!"
date: "2013-04-02"
categories: 
  - "archive"
---

Hello readers, so I just want to say something before I get into the "meat" of this post.... (bear with me)

I don't work in InfoSec, I don't have a full-time job where I poke holes in systems, or look at IDS logs and pcap files all day long (would be nice but..) but what I do have is a passion for InfoSec and a desire to give back to the community. I've never met 98% of the people who read this blog, but over the last 15 months a lot of you have inspired me to push myself harder and further than I thought possible so this is my way to pay some of that back..

I give you **sniffMyPackets**, Maltego transforms (based on the Canari Framework) for analysing pcap files..

I decided to write these transforms after the Cyber Security Challenge published a cipher challenge that centered around a pcap file, that and my interest in packets meant this seems liked a good way to mix the two (no I haven't cracked the cipher challenge).

Now this is a **BETA** now "beta" means different things to different people so this is my definition:

1. I'm not a developer/coder, I've been writing stuff in Python for less than a year. The transforms in this package work and while not perfect they are functioning. I've tested them against as many different pcaps as I feel is necessary to make sure they work properly.
    
2. This is a BETA, so things like error handling are missing from some transforms, I will get around to it but I'm learning as I go so bear with me.
    
3. This is by no means a finished product, I will continue to add transforms as I go and if you want anything specific let me know and I will add it (or you can add it yourself), but please send me a pcap file if you can.
    
4. If (more likely when) you find a problem log it as an issue on the github.com site, the same if you think of something that will improve the package.
    
5. I don't have a full license of Maltego so I've only tested with the community edition within Backtrack (unless someone wants to donate a license..).
    
6. I've only tested this on Backtrack, for 2 reasons.. 1) I don't own a Mac Book, 2) Windows doesn't play nicely with Scapy and lets face it, it's not the best platform for pcap analysis.
    

I think that's about it in terms of "BETA". I've written a lot of transforms using Scapy (yes yes I love Scapy) but as people often say, "Use the best tool for the job" so some of them use tshark which has been cool because I've learnt a lot more about both tools. Writing python code with Scapy isn't hard (even for me), but making them "appear" nicely in Maltego has been challenging at times and I'm not 100% happy with the way the entities link, but like I said it's a BETA... :)

I've created a wiki that lists the entities and transforms available and I will update them as I go, you can find the wiki here: https://github.com/catalyst256/sniffMyPackets/wiki

If you want to have a play with the transforms you can go here: https://github.com/catalyst256/sniffMyPackets

I also did a short video about the transforms (some of which have changed now) but you can find it here:

http://www.youtube.com/watch?v=GHKuHPfZW5g

So have a play, let me know what you think (good or bad) and I will let you know about updates (new transforms etc etc) when I write them. I may even create a new twitter account so I don't annoy people with updates all the time.
