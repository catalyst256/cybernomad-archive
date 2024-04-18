---
title: "Netscaler: Making sense of the Cookie - part 1"
date: "2012-01-03"
categories: 
  - "archive"
---

[![](images/robot-trans-mini.jpg "newheadshot")](http://theitgeekchronicles.files.wordpress.com/2012/02/robot-trans-mini.jpg)

Today was the first day back after my Christmas break, so it was a bit "slow". Never to sit around being bored, I was writing up some notes on Netscaler cookie's for an ethical hacker called Alejandro Nolla who has written up a cool application for checking to see if a domain has a load balancer behind it. You can find the application [here](http://code.google.com/p/loadbalancer-finder/) or follow Alejandro on Twitter at [@z0mbiehunt3r](https://twitter.com/#!/z0mbiehunt3r)

Anyway while typing up my info I discovered something about the Netscaler cookies that I hadn't noticed before. The Netscaler cookies are by default "encrypted" in 3 parts. Below is the extract from Citrix regarding Netscaler cookies:

> The format of the cookie that the NetScaler appliance inserts is: NSC\_XXXX= where: NSC\_XXXX is the virtual server ID that is derived from the virtual server name. ServiceIP is an encrypted representation of the service IP address. ServicePort is an encrypted representation of the service port.

So the 3 "encrypted" parts are Virtual Server ID, ServiceIP and then ServicePort. After a bit of coffee I realised something about the Virtual Server ID, it is "encrypted" using a substitution cipher, for example a=z, d=c etc. etc. the name "NSC\_mc\_udru" would be "NSC\_lb\_test" on the Netscaler as the configured load balancer name.

Now it might not seem much to you, but I was happy with my discovery, my next challenge finding out how the ServiceIP and ServicePort is encrypted. This is an NSC cookie

> ffffffff3c19594d45525d5f4f58455e445a4a423660

Now to me at first it looked like HEX the first 8 F's equally to 255 255 255 255 which seemed like it was a subnet address you use to reference a single host (as you would expect from a persistence cookie), I also know that **af** when converted from hex to dec equals 175 but the server IP actually starts with 172. I've converted the rest from hex to dec but the numbers are out for the server IP. At the end of the example above I know that for port 80 (http) the value is **3660** only changes if the port changes, the rest seems to stay the same.

So I'm a third of the way there.. maybe I will never break the encryption but it's fun trying and it's given my brain a good workout. If you can spot something I've missed then let me know.

**NOTE:** I have since worked out how to decrypt the whole cookie and wrapped it up in a python script which you can find [HERE](https://github.com/catalyst256/Netscaler-Cookie-Decryptor)

The Geek
