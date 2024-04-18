---
title: "sniffMyPackets Version 2 - The Release"
date: "2014-12-30"
categories: 
  - "archive"
---

If you follow me on Twitter you have probably noticed me bombarding you with tweets about the next release of sniffMyPackets, well today it's officially released in a Beta format. There is still a lot of work to be done and over the next few weeks/months expect a lot of changes. The purpose of this blog post today is to give you an overview about what this new version is all about, my hope is that you download it, try it and let me know what you think.

**DISCLAIMER:**

This is a beta, I've tested the code as much as possible but you might still have issues, if you do please raise an Issue ticket on the respective GitHub repo.

**What's New:**

The original sniffMyPackets was the first piece of code I wrote and it was really more a journey of discovery than an attempt at producing anything that could be classed as "production ready". This release of sniffMyPackets is my attempt at turning it into something that is not only useful (well hopfully) but also that can be used and shared among analysts within an organisation.

There are two main changes to this release (aside from better Python code), the first is the use of a database backend (currently MongoDB), the second is the introduction of a web interface. These two components work in harmony with the Maltego transforms to give you more information without overwhelming the Maltego graphs but still giving you the visibility you need from within Maltego.

**Database:**

The database is used by the Maltego transforms to store additional information when transforming a pcap file. When building Maltego graphs you need to be able to tell a story but at the same time not overwhelm the graph with too much information (especially when building large graphs). Each transform when run not only returns a Maltego entity but also writes additional information back into the database. You can then use this information to "replay" a pcap file without needing the actual pcap files (just the Session ID).

For example, when you first add a pcap file into a blank graph you need to index it. This process generates a unique Session ID for that pcap file (based on MD5 hash) which is what is returned to your Maltego graph. What also happens is that additional information is written to the database, which is then displayed in the web interface. Below is the information collected from just that single transform:

- MD5 Hash
- SHA1 Hash
- Comments
- Upload Time
- File Name
- Working Directory
- Packet Count
- First Packet
- Last Packet
- PCAP ID (or Session ID)
- File Size

While you may think this slows the transform down, I've made ever attempt for that not to be the case. A lot of the Python code for this release has been rewritten to speed execution and remove redundant code.

**Web Interface:**

Personally this part has been the greatest challenge for me (but at the same time it's awesome), the whole purpose behind this addition is to allow you to share the analysis of a pcap file with anyone. As you run transforms from Maltego on a pcap file the web UI is populated with the "detailed" information. Even if you close the Maltego graph and delete the pcap file that information is still there and you can still "replay" that back into Maltego (cool or what..).

The web UI has some cool features:

File upload functionality so you can upload pcap files or artifacts from Maltego to the web server to allow for other people to download them. Full packet view, when a TCP/UDP stream is indexed in Maltego you can view each individual packet from the web interface (based on Gobbler).

**Other Stuff:**

A lot of the Python code has changed, for example the old method of extracting TCP & UDP streams using tshark has been replaced with pure Python code, this means it's quicker and removes the need to have tshark/wireshark installed (and stops issues arising when the command syntax changes). The way that sniffMyPackets checks the file types has been turned into a Python function, likewise the way that we check to make sure a pcap file isn't a pcap-ng format is now a pure Python function.

I've also started to write 3rd party API plugins as well, currently you can submit an artifact MD5 hash to VirusTotal to see if there is an existing report available. In the future the functionality to upload the file to VirusTotal will also be added. I'm also planning on adding CloudShark functionality so you can upload files to a CloudShark appliance or their cloud service.

**Vagrant Machine:**

Now one of my concerns when I decided to add a web interface and a database backend was whether or not people had the infrastructure available to do accomodate this, so in order to try and make it more accessible I created a Vagrant machine. Vagrant is an awesome tool if you do development work, it allows you to script the creation and build of a machine that is repeatable and gives the same results time after time. Within the sniffMyPackets-web repo there is a vagrant folder. If you have Vagrant & Virtualbox installed (required I'm afraid), you can simply type (from within that vagrant folder):

`vagrant up`

This will create a Linux server, download, install and configure the MongoDB database and then the required libraries for the web interface before starting it running. The initial vagrant up can take about 20 minutes (as it has to download stuff) but after that your vagrant up will have your server ready in minutes.

**Maltego Machines:**

There are currently two Maltego Machines available. Both serve a different purpose (kind of obvious I know).

**\[SmP\] - Decode pcap:** This is run after you have indexed a pcap file, it will extract all the TCP/UDP streams, IPv4 Addresses, DNS requests, HTTP requests etc. straight into your Maltego graph.

**\[SmP\] - Replay Session (auto):** If you want to replay a pcap file you can add a Session ID entity (with a valid Session ID), return the pcap file and then run this Maltego Machine against the pcap file. This runs every 30 seconds (useful if you the community edition of Maltego) and will extract all the information in the database for that pcap file into a Maltego graph. This is awesome if you don't have the original pcap files or what to review information at a later stage that is already in the database.

**How to get it:**

The two GitHub repo's can be found here:

**Maltego Transforms:** https://github.com/SneakersInc/sniffmypacketsv2 **Web Interface:** https://github.com/SneakersInc/sniffmypacketsv2-web

I'm in the process of adding documentation and all that fluffy stuff so bear with me, but if you've used sniffMyPackets before then install instructions are the same.

If you want to see it in action there is YouTube video below:

https://www.youtube.com/watch?v=RtOMAneWlt8
