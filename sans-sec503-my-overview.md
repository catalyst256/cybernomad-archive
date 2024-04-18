---
title: "SANS SEC503 - My overview"
date: "2013-07-15"
categories: 
  - "archive"
---

So last week I attended the SANS Summer London 2013 event and take part in the **SEC503 - Intrusion Detection in-depth** course. You may remember that this was one of the prizes I won for in the _Cyber Security Challenge_ Masterclass (I was part of the winning team).

The course is run over 6 days and aims to give you a good working knowledge of network packets (IPv4, IPv6, Snort etc. etc.), to allow you to be a better Intrusion Analyst. I picked this course due to the fact I'm a bit of a packet monkey and spend far too much time playing with Scapy.

I don't mind admitting I was a bit nervous about attending the course, it's a SANS course after all (you know the best of the best type thing) and my working knowledge of TCP/IP was a bit limited but I was determined to make the most of it. I arrived on the first day at the Hilton Hotel in Kensington & Chelsea (awesome venue, great food and facilities) and signed in.

Once I was armed with my free t-shirt and ID badge I wandered off to find the classroom and get setup. I decided to sit at the back for no real reason other than it gave me a better view of the whole classroom (no I wasn't shoulder surfing I promise..). The material for the class consisted of 7 books, one for each day and the exercise book. Flicking through them didn't really help my nerves as there was a lot within the first few days to do with hex and reading "raw" network packets. On the plus side there was mention of Scapy.. :)

I won't bore you with the day by day details so here are highlights:

Day 1 -2: This was all about packets (bet you didn't guess that) and covered the IP and TCP headers in great depth. Everything from reading them as "raw" packets and working out the hex -> dec values for different offsets to how to pick out bad packets.

Day 3 - Cover the application layer of the TCP/IP stack as well as a section on crafting packets. Much to my delight there was a section on Scapy and one of the highlights of the week was when my instructor announced that I had written and Scapy guide which was "quite good" (quite good is better than OK). I admit I took satisfaction in noticing people navigate to my blog to have a look at it.. :)

Day 4 - This was about Snort, a product I had briefly touched on with Security Onion and was keen to learn more about. We covered architecture, performance tuning and rule writing which was actually good fun (sad I know).

Day 5 - We focused on Network Forensics, using a mixture of pcap files and SiLk network flows to determine attacks and then we talked about log coloration and some of the products available.

Day 6 - Challenge Day.. unfortunately I missed that last day due to engineering works messing up my trains, and I didn't really fancy a 3.5 hour trip to London via bus. I have got the challenge files so I will be doing it later this week.

So that's it, my first ever SANS course. Overall an excellent experience, the instructor **Johannes Ullrich, Ph.D.** was just brilliant both in his delivery of the content (I didn't fall asleep once) and with his expert knowledge of the subject.

The course did teach me a couple of things (aside from all that packet related stuff wedged into my head) which I will share.

1. I actually know more than I realised, the first day was hard but by the second day I was getting to grips with the packets and it really started to make sense. There were a couple of points that when we were discussing exploits and what something meant if you see it in a packet I was one of the few to raise my hand to say "I know". We also cover buffer overflows (in terms on what they look like on the network) and due to my OSCP I was more than comfortable understanding what it all meant (cross training is good).
    
2. The work I've been doing with Scapy and sniffMyPackets is going to benefit (well it already has), in the downtime during the course I've rewritten some of my transforms to work better and I have plenty more in the pipeline based on the course. I also "understand" better the content of what I've been writing and how to pull packets apart in a more efficient manner.
    

If you are thinking about a SANS course (and work will pay) I would highly recommend them. **Awesome instructors**, **awesome content** and you won't be disappointed.

Now to start preparing for the exam...
