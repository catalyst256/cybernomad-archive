---
title: "Building your own Whois API Server"
date: "2016-03-02"
categories: 
  - "archive"
---

So it's been a while since I've blogged anything not because I haven't been busy (I've actually been really busy), but more because a lot of the things I work on now I can't share (sorry). However every now and again I end up coding something useful (well I think it is) that I can share.

I've been looking at [Domain Squatting](https://en.wikipedia.org/wiki/Cybersquatting) recently and needed a way to codify whois lookups for domains. There are loads of APIs out there but you have to pay and I didn't want to, so I wrote my own.

It's a lightweight Flask application that accepts a domain, does a whois lookup and then returns a nice JSON response. Nothing fancy, but it will run quite happily on a low spec AWS instance or on a server in your internal environment. I built it to get around having to fudge whois on a Windows server (lets not go there).

In order to run the Flask application you need the following Python libraries (everything else is standard Python libraries).

- Flask
- pythonwhois (this is needed for the pwhois command that is used in the code)

To run the server just download the code (link at the bottom of page) and then run.

`python whois-server.py`

The server runs on port 9119 (you can change this) and you can submit a query like this:

http://localhost:9119/query/?domain=example.com

You will get a response like the picture below:

[![](https://theitgeekchronicles.files.wordpress.com/2016/03/screen-shot-2016-03-02-at-11-06-12.png?w=776)](http://itgeekchronicles.co.uk/2016/03/02/building-your-own-whois-api-server/screen-shot-2016-03-02-at-11-06-12/#main)

From here you can either roll it into your own tool set or just it for fun (not sure what sort of fun you are into but..).

You can find the code in my GitHub repo [MyJunk](https://github.com/catalyst256/MyJunk) or if you just want this code it's [here](https://raw.githubusercontent.com/catalyst256/MyJunk/master/whois-server.py).

There may well be some bugs but I haven't found any yet, it runs best on Linux (or Mac OSX).

Any questions, queries etc. etc. you know where to find me.
