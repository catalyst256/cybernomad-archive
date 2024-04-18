---
title: "OSINT: Email Verification API"
date: "2016-12-28"
categories: 
  - "osint"
---

When I started writing Python my focus was on building tools, then I realised that the tools I built I never actually used, mostly because at the time my job wasn't security related and it was more of a hobby. Now I work in security and the majority of the code I write is focused around collecting, processing and displaying data. I'm lucky I love the work I do, for me the "fun" is around solving problems and because of that the code I write (and I still write a LOT of it) has evolved.

Open Source Intelligence (OSINT) is my new "hobby" there is some cross over into my job but for the most part I write code to perform OSINT related functions. One of the things I discovered on my OSINT journey is that there are lots of API's, data feeds out there and all sorts that you can use but a lot you have to pay for and in some cases it's not always clear on how the data is collected or stored. When faced with issues like this I tend to work on the following principal, "if in doubt, write it yourself".

A while back I discovered a new Python library that is just awesome when you want to create your own API's called flask\_api (an extension of the Flask framework) and I've been using it a lot lately to create new "things".

Today I wanted to share with you my email verification API, essentially you pass an email address to it and it will check to see if it can determine whether or not the email is "valid". It will also check to see if the target email server is configured as a "catch-all" (in other words any email address at the specified domain will be accepted).

All of this is return in a lovely JSON formatted output which makes it really easy to use as part of a script of another service and even easier to dump the returned data into a database (if you like that sort of thing) and to ease installation there is also a Docker container available if you are into that sort of thing.

The **README.md** in the Github repository has the necessary installation instructions but its nice and simple once you've cloned the repo.

- pip install -r requirements.txt
- python server.py

Then to test it you simply use this URL:

**_http://localhost:8080/api/v1/verify/?q=anon@anon.com_** (replacing anon@anon.com with your target email address). If all works, you should get something similar to the screenshot below.

[![emailverify-screenshot](https://theitgeekchronicles.files.wordpress.com/2016/12/emailverify-screenshot.png?w=756)](http://itgeekchronicles.co.uk/2016/12/28/osint-email-verification-api/emailverify-screenshot/)

The code can be found [HERE](https://github.com/catalyst256/sneakersinc-emailverify)

Any questions, queries, feedback etc. etc. let me know.
