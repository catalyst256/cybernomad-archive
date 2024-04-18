---
title: "Netscaler: rpcNode issue"
date: "2011-10-31"
categories: 
  - "archive"
---

[![](images/robot-trans-mini.jpg "newheadshot")](http://theitgeekchronicles.files.wordpress.com/2012/02/robot-trans-mini.jpg)A few weeks ago I upgraded the firmware on our front facing Netscaler appliances. I have a love/hate relationship with Citrix firmware upgrades, the last one managed to break the hardware on one of the appliances which resulted in it re-ordering the network interfaces (a bit annoying when it's 2am). This time round the firmware applied fine however on my post implementation review I discovered the following error in the Netscaler logs (ns.log and auth.log which can be found in /var/log). _Oct 31 09:52:00 hostname sshd\[8640\]: Failed password for #nsinternal# from nsip port 16051 ssh2 Oct 31 09:52:00 hostname sshd\[8641\]: Failed password for #nsinternal# from nsip port 8474 ssh2_

The impact of this error was that SSL certificates couldn't be applied to the appliance as it responded with an error, other than that the appliance was working fine and HA failover was still functioning as expected (however the error moved when failed over to the secondary).

I logged a call with Citrix Support who on checking the configuration that the rpcNode password was different;

_1) IPAddress: NSIP Password: 87s9das8d7a8d0a9d8a9 SrcIP: NSIP Secure: ON 2) IPAddress: HA NSIP Password: 8c7d34kj3434m3kl34k3ll34k3 SrcIP: NSIP Secure: ON_

Now neither myself or Citrix have any idea why this would have changed during a firmware upgrade but it had. Citrix's recommendation was to break the HA pair and then recreate it. Awesome, however because these appliances are used heavily 24/7 it meant that I would have to use GSLB to fail our live websites over to the other datacentre and then mess around with HA at silly o'clock in the morning.

Not being one to always take the advice of others (but then who does) I decide to experiment with an idea, rather than breaking the HA I would simply just reset the rpcNode passwords on the primary appliance. My logic being that as the passwords were different already changing them wouldn't matter.

Logging on via SSH (I love command line stuff), I executed the following commands on the primary Netscaler:

_set rpcNode NSIP -secure yes -srcip NSIP -password cleartextpassword (sets the rpcNode password on the primary appliance) set rpcNode HA Node -secure yes -srcip NSIP -password cleartextpassword (sets the rpcNode password for the HA node)_

Once I had changed the rpcNode password I checked /var/log/auth.log and the previous failure message had gone and was replaced with:

_Oct 31 12:14:00 hostname sshd\[9968\]: Accepted password for #nsinternal# from NSIP port 24449 ssh2 Oct 31 12:14:00 hostname sshd\[9967\]: Accepted password for #nsinternal# from NSIP port 16042 ssh2_

The final test was adding a new SSL Certificate to the appliance which was succesful with no error.. SUCCESS

The Geek
