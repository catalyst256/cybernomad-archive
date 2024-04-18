---
title: "Open Source Cyber Intelligence - Course Review"
date: "2015-10-08"
categories: 
  - "archive"
---

**DISCLAIMER:** This review is based on my own experience attending the course, and is no way affiliated with the training provider or my current employer. All opinions stated in the review are my personal views and should be treated as such.

A couple of weeks ago (might be more) I spent the week in London on a training course (well actually it was two but..). The courses were run by [QA](http://www.qa.com) and are part of their Cyber security range. Details of the courses are below (just in case you want to go on them).

[Open Source Cyber Intelligence - Introduction](http://www.qa.com/hot-topics/cyber-security/cyber-intelligence/open-source-cyber-intelligence---introduction) (3 days)

[Open Source Cyber Intelligence - Advanced](http://www.qa.com/hot-topics/cyber-security/cyber-intelligence/open-source-cyber-intelligence---advanced) (2 days)

Now it's important to note at this point that the courses are focused on "Cyber" based Open Source intelligence techniques rather than the more generic Open Source Intelligence which in my mind is more about stalking people, sorry I mean tracking people.

Below is a brief outline of what the courses contained (taken from the QA website).

**Open Source Cyber Intelligence - Introduction**

Introduction Module 1 - History of the Internet and the World Wide Web Module 2 - How devices communicate Module 3 - Internet Infrastructure Module 4 - Search Engines Module 5 - Companies and people Module 6 - Analysing the code Module 7 - The Deep Web Module 8 - Social Media Module 9 - Protecting your digital footprint Module 10 - Internet Communities and Culture Module 11 - Cyber Threat Module 12 - Tools for investigators Module 13 - Legislation

**Open Source Cyber Intelligence - Advanced**

Module 1 - Advanced search and Google hacking Module 2 - Mobile devices; threats and opportunities Module 3 - Protecting your online footprint and spoofing Module 4 - Advanced software Module 5 - Hacking forums and dumping websites Module 6 - Encryption and anonymity tools Module 7 - Tor, Dark Web and Tor Hidden Services (THS) Module 8 - Bitcoin and Virtual Currencies Module 9 - Other Dark Webs and Darknets Module 10 - Advanced evidential capture

**NOTE:** The courses are designed for people of any skill level which is why when you look at some of the module titles and think "Why are they teaching networking basics" it may seem a little bit random.

It's also important to point out that the prerequisite for the advanced course is that you have completed the introduction course first.

**Course Review:**

The size of the class (for both courses) was smaller than I expected but that wasn't a bad thing as it gave us the chance to ask questions and provide a steer on the direction of the conversations without feeling like we were stopping lots of people from learning (and getting their monies worth).

You get a preconfigured workstation that has all the tools you need for the course as well as a Kali virtual machine, multiple browsers, plugins etc. and the workstation has plenty of grunt (CPU & Memory) to not become bogged down when running lots of things.

The instructor for both courses was a guy called [Max Vetter](https://uk.linkedin.com/pub/max-vetter/21/3b7/673) who has loads of experience in this area and made sure we understood the content of the course and that it was also fun (check out "If Google was a Guy" on [Youtube](https://www.youtube.com/results?search_query=if+google+was+a+guy)).

Now I'm no OSINT expert, but I have worked in IT for nearly 20 years and I am a bit of a OSINT wannabe so for me the introduction course was a bit slow. Don't get me wrong, the content and the method it was delivered was awesome, but if you know about networking and how to do whois lookups or view source code in websites, you may find the introduction course not to your liking.

If however you know how to do all of the above, but have never done it within a OSINT type scenario then the course will be really useful (and fun) as it will enable you to understand how to use the information you collect in order to track and trace "cyber bad guys".

For example if you find a "bad" domain, you can query whois to find out who registered it, then using a bit of Google-fu (Google hacking is covered in the introduction course) see if you can use the details from the whois information to find any other domains the suspect might have registered.

Let me show you, looking at the whois information for _qa.com_ (the training provider) you will see the following:

_For more information on Whois status codes, please visit https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en. Domain Name: qa.com Registry Domain ID: 113160\_DOMAIN\_COM-VRSN Registrar WHOIS Server: whois.1and1.com Registrar URL: http://1and1.com Updated Date: 2013-05-24T22:04:57Z Creation Date: 1994-10-25T04:00:00Z Registrar Registration Expiration Date: 2015-10-24T04:00:00Z Registrar: 1&1 Internet AG Registrar IANA ID: 83 Registrar Abuse Contact Email: abuse@1and1.com Registrar Abuse Contact Phone: +1.8774612631 Reseller: Domain Status: clientTransferProhibited https://www.icann.org/epp#clientTransferProhibited Registry Registrant ID: Registrant Name: Alexandra Kubicka Registrant Organization: QA-IQ Ltd Registrant Street: 80 Cannon Street Registrant Street: 4th Floor Registrant City: London Registrant State/Province: ABE Registrant Postal Code: EC4N 6HL Registrant Country: GB Registrant Phone: +44.8450559501 Registrant Phone Ext: Registrant Fax: +44.8450559502 Registrant Fax Ext: Registrant Email: IT@QA.com Registry Admin ID: Admin Name: Alexandra Kubicka Admin Organization: QA-IQ Ltd Admin Street: 80 Cannon Street Admin Street: 4th Floor Admin City: London Admin State/Province: ABE Admin Postal Code: EC4N 6HL Admin Country: GB Admin Phone: +44.8450559501 Admin Phone Ext: Admin Fax: +44.8450559502 Admin Fax Ext: Admin Email: IT@QA.com Registry Tech ID: Tech Name: Hostmaster ONEANDONE Tech Organization: 1&1 Internet Ltd. Tech Street: 10-14 Bath Road Tech Street: Aquasulis House Tech City: Slough Tech State/Province: BRK Tech Postal Code: SL1 3SA Tech Country: GB Tech Phone: +44.8716412121 Tech Phone Ext: Tech Fax: +49.72191374215 Tech Fax Ext: Tech Email: hostmaster@1and1.co.uk Nameserver: ns1.dnsexit.com Nameserver: ns2.dnsexit.com Nameserver: ns3.dnsexit.com Nameserver: ns4.dnsexit.com DNSSEC: Unsigned_ Then using one of the values from the whois (telephone number, email address, contact name(s)) you can get Google to find other domains that have the same whois information.

Within the Google search bar enter the following query:

_site:whois.domaintools.com +44.8450559501_

You will get back all the registered domains (stored in whois.domaintools.com) that have that phone number in the whois information. If like me you are a Python addict (it's ok to admit it) you can automate this kind of process and write the information to something (flat file, database etc etc) but that's not covered in the course.

The last two days of training was the fun stuff, the advanced course covered all the good stuff like Tor, "Darknet" forums, bitcoin and other grey online communities. Again I had played around with most of this (and read books etc), and again the advanced course is again aimed at all skill levels but there was a good mixture of theory and practice.

Over the two days you get to mess around (within legal guidelines) with different "Darknets" such as Tor, I2P, Freenet and GNUnet which is quite fun and interesting to see how mature some of the others are in comparison to Tor. You also learn about the history of Bitcoin (which in itself is quite interesting) and look at the different ways to track bitcoin transactions and all the other various "alt coins" that have sprung up (did you know there was a Bieber coin (BRC)??).

**Summary:**

For me I really enjoyed both courses, yes I knew a lot of the stuff already (not being big-headed) but it's always good to have an expert validate what you know, especially when you have taught yourself most of it (and now I have certificates). It also introduced me to lots of new OSINT resources (websites, tools etc) as well as helped focus the "flow" of the data you can collect, and some better ways of processing it and reusing it for other purposes. Plus it also opened up lots of other opportunities for new Maltego transforms (I bet you thought this was a Maltego free blog post).

**Pros:**

- Excellent instructor
- Good facilities (lots of free coffee and biscuits)
- Good course content which was delivered well
- Fun (that's important)

**Cons:**

- Might not be the course for you if you already do OSINT related work or have a deep technical background around "Cyber"

If you have any questions or queries just give me a shout.

Adam
