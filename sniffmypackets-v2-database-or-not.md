---
title: "sniffMyPackets V2: Database or not??"
date: "2015-01-29"
categories: 
  - "archive"
---

When I started the work on sniffMyPackets version 2 I decided to make it default, to using a database backend. The decision around this was based on trying to get the most out of the pcap files without crowding the Maltego graph. I knew at the time that this means that people who want to use my code would have to have additional infrastructure for it to run. I've tried to minimize the pain by introducing a Vagrant machine that will build a MongoDB database instance and install the new web interface (which is awesome).

Recently I was asked if you "HAD" to use a database and I realised that this decision, might limit the number of people who will use sniffMyPackets. Which that in mind I have now rewritten the Maltego transforms to work with or without a database. The limitation is that don't get to use all the transforms (such as replaying a session) but you still get the same output on the Maltego graph.

The changes have been pushed to the GitHub repo which is linked below:

https://github.com/SneakersInc/sniffmypacketsv2

By default the database support is switched off, the good news is that it only takes one change to enable it. When you create the Canari profile a **sniffmypacketsv2.conf** file is created in the **src/** directory. So for example:

`canari create-profile sniffmypacketsv2 -w /root/localTransforms/sniffmypacketsv2/src`

This creates the `sniffmypacketsv2.conf` file in the `/root/localTransforms/sniffmypacketsv2/src` directory. The configuration file comes preconfigured with several options most of which you can change to meet your needs. The important one for the database support is under the \[working\] section and is called 'usedb' (see pictures below).

To enable database support, simply change the value from **0** to **1** and you are good to go (so to speak). You can turn it on and off whenever you want.

Database Support Off:

[![databaseoff](https://theitgeekchronicles.files.wordpress.com/2015/01/databaseoff.png?w=235)](https://theitgeekchronicles.files.wordpress.com/2015/01/databaseoff.png)

Database Support On:

[![databaseon](https://theitgeekchronicles.files.wordpress.com/2015/01/databaseon.png?w=172)](https://theitgeekchronicles.files.wordpress.com/2015/01/databaseon.png)

Any Maltego transform that won't work without the database just returns a message to your Output window about needing database support. To be honest most of the important ones work without database support so you should notice much difference.

If you decide not to use the database then you will be missing out on this cool looking web interface (see below)

[![coolwebinterface](https://theitgeekchronicles.files.wordpress.com/2015/01/coolwebinterface.png?w=640)](https://theitgeekchronicles.files.wordpress.com/2015/01/coolwebinterface.png)

Over the next few weeks, more Maltego transforms will be created/added and I've got some cool features planned in terms of Maltego TDS based transforms and for the web interface as well. I'm also going to start working on adding NetFlow support to sniffMyPackets as well so you can throw even more stuff into the mix.

I will also be working on the documentation and will run a blog series on some of cool features you might not notice straight away.

Oh my online demo site is still online (which sometimes stops working but that's not my code more the server) and you can find it at:

http://demo.sniffmypackets.net

As always feedback is welcome (good or bad).

Enjoy.
