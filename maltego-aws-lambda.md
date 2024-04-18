---
title: "Maltego: AWS Lambda"
date: "2018-06-18"
categories: 
  - "maltego"
  - "python"
---

One of the awesome things about Maltego (and Paterva, the company that makes it), is that they allow people like me, to host remote Maltego transforms (Transform Host Server) using a mixture of their Community iTDS server and your own infrastructure.

For a few years now I’ve been running remote Maltego Transforms on an AWS instance (t2.micro). All you need to get it up and working is a Linux server, Apache2 (you could probably use nginx) and a little bit of time. Paterva provide all the files and installation notes you need [HERE](https://docs.maltego.com/) and it works a treat.

If you work in tech you can’t really escape the flood of people talking about “Serverless” deployments and all that kind of stuff so I thought it would be cool to try and recreate the Transform Host Server but using Python 3.x, Flask (instead of Bottle), Zappa, and AWS’s Lambda functions.

Paterva’s original Maltego.py (the Python library for remote transforms) is written in Python 2.x, which means the first job was to tweak the file so Python 3.x didn’t get all funny about _print_ statements (the good news is the tweaked version is available in the Github repo below).

Once that was done, I then needed to recreate the web server component (which was originally written in Python’s bottle framework) to make use of the awesome Python Flask framework. This probably took the longest as I had to work out the differences between Flask & Bottle.

A skeleton Flask server is shown below, the transform I wrote to prove the concept was to simply connect to a website, and return the status code (200, 404 etc. etc.).

\[code lang=python\] #!/usr/bin/env python

from flask import Flask, request from maltego import MaltegoMsg from transforms import trx\_getstastuscode

app = Flask(\_\_name\_\_)

@app.route(‘/’, methods=\[‘GET’\]) def lambda\_test(): return ‘Hello World’

@app.route(‘/tds/robots’, methods=\[‘GET’, ‘POST’\]) def get\_robots(): return (trx\_getstastuscode(MaltegoMsg(request.data)))

if \_\_name\_\_ == "\_\_main\_\_": app.run() \[/code\]

The _lamba\_test_ function, is just there so I could make sure it was working, you can (and probably should remove it).

Now the next step was to deploy this to AWS’s Lambda service, being lazy I decided to try [Zappa](https://github.com/Miserlou/Zappa);

> Zappa makes it super easy to build and deploy server-less, event-driven Python applications (including, but not limited to, WSGI web apps) on AWS Lambda + API Gateway. Think of it as “serverless” web hosting for your Python apps. That means infinite scaling, zero downtime, zero maintenance — and at a fraction of the cost of your current deployments!

Zappa is super easy to use, just follow the instructions and make sure to use Python virtual environments (not like someone we won’t mention, who forgot). Provide Zappa with some AWS credentials that have the level of access and within minutes you will be deploying your new app as a AWS Lambda function (seriously it only takes a few minutes). It’s important that you take note of the URL provided at the end of the deployment as you will need it in the final stage.

The final stage to this great masterpiece is to configure your account on Paterva’s Community iTDS server to point to your new transform. The documentation is [HERE](https://docs.maltego.com/) if you’ve never done before. Just one thing to note, the \*Transform URL \*is the URL outputted by Zappa above (it should stay the same not matter how many times you deploy).

The nice thing about using AWS’s Lambda functions is that its really easy and quick to deploy and the pricing model works great if you aren’t expecting heavy usage (1 million requests per month or 400,000 GB-seconds per month on the free-tier). Now there is no reason for you not to be hosting Maltego transforms for the world to share…

All the files you need to deploy your first Maltego Lambda function are in my Github repo below, clone the repo, configure your virtual environment (there is a handy requirements.txt to help) and off you go.

[Simple example of how to use AWS's Lambda functions to host Maltego remote transforms](https://github.com/catalyst256/aws-maltegotransforms)
