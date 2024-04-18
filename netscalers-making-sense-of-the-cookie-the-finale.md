---
title: "Netscalers: Making sense of the cookie – the finale"
date: "2012-01-23"
categories: 
  - "archive"
---

[![](images/robot-trans-mini.jpg "Robot-Trans-Mini")](http://theitgeekchronicles.files.wordpress.com/2012/02/robot-trans-mini.jpg) So this is the final part to my Netscaler cookie series. If you haven't read the other two blog posts you may want to just so this makes a bit of sense..

[Part 1](http://itgeekchronicles.co.uk/2012/01/03/netscaler-making-sense-of-the-cookie-part-1/)

[Part 2](http://itgeekchronicles.co.uk/2012/01/06/netscalers-making-sense-of-the-cookie-part-2/)

All make sense now?? (probably not but it's polite to ask)..

Before I get started I just want to clear something up. I am in no way shape or form a programmer.. It's one of those areas that up until recently has made my head hurt (and not just from banging my head on the desk a lot) but it is an area that I want to improve on and the best way for me to learn is to do.

So how do you end a series of blog posts about Netscaler cookies and how to decrypt them.. well you write a program to do it for you. I decided to use python to write my little decryption program as it will run on both Windows and Linux (I've even tested it to make sure) and it seems to be used a lot by InfoSec type people.

Now this is my first ever python program/script/application and in fact it's the very first time I've ever written something like this (unless you count the macro I wrote in Word 7 that did a cypher substitution encryption), so yes while the code might not be perfect and possibly badly written the important thing is that it works. :)

Now before I get to the part where I give you the link to the script (is script the right word??) here's how it works (in basic terms).

The script is designed to do 2 things, it accepts an Netscaler Cookie from the command line;

`python nsccookiedecrypt.py NSC_rfse-gesfe-etsgsvs...` (not the complete cookie)

It then runs two `re.search` functions to separate the cookie name (the Netscaler load balancer vserver name) and then the Server IP (IP address of the server your are persistent too).

Once it has these variables, it performs two decryption actions, the first is the cipher substitution to give you the real Server Name;

`Service Name=qerd-fdred-dsrfrur-erdded`

It then runs the XOR decryption based on the key that was mentioned in Part 2 of my series to give you then Server IP;

`Server IP=63.17.71.92`

Currently the script outputs both to the command line, it's not exactly high end coding but it's not a bad start for me.

You can find the script [HERE](http://code.google.com/p/netscaler-cookie-decryptor/), I've tested in on over a dozen real life Netscaler Cookies, so I'm 90% happy it will work in all cases, it doesn't use any fancy imports so you should be good to go with just a standard python install.

If you find any bugs or want to let me know how to make it better, please drop me a line. Over time once I get better at coding I will probably improve it. I've created a new "Page" on my blog with links to the code and hopefully over time I will add to it.

If you want to modify the script for your own uses, please do, however if you let me know so I can keep tabs on how it's being used and what I can do to improve it.

I would like to thank [Alejandro Nolla](https://twitter.com/#!/z0mbiehunt3r) for inspiring me to write this (check out his load balancer finder) and [Daniel Grootveld](https://twitter.com/#!/shDaniell) for helping me with the XOR decryption (and by help I mean stopping me from using a Excel spreadsheet).

Happy decrypting.
