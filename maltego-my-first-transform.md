---
title: "Maltego: My First Transform"
date: "2014-09-21"
categories: 
  - "archive"
---

On Friday I posted a challenge on twitter called "Transform Friday", you suggest a Maltego transform and I would have a go at writing it. There is no real reason behind the challenge other than I like writing Maltego transform and its a nice way to write something different to the normal packet related ones I do. My plan was to write them without using Canari (native Maltego transforms) and make them all available as TDS transforms (no installation required).

[![Screen Shot 2014-09-21 at 08.41.28](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-08-41-28.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-08-41-28.png)

The first challenger soon steeped up and much to my surprise it was Paterva (yes thats right the people that make Maltego).

[![Screen Shot 2014-09-21 at 08.41.44](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-08-41-44.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-08-41-44.png)

However it was then suggested to me that it would be quite cool if I wrote up how you (yes you) could create your own Maltego transforms. Now this is where I need to apologise, I've spent the last two years telling you all about the awesome (well I think they are awesome) Maltego transforms I've created but I've never told you how. Part of the reason behind this blog was to share information and try in some way to save you the pain I've suffered, that's why I wrote the Scapy guide after all.

So this will be the first in a series of blog posts about creating Maltego transforms. Today we are going to cover some of the basics and write a nice and simple transform (that's useful at the same time), and then over the coming weeks we will cover more advanced topics, like setting additional properties, changing link colour and finally creating TDS transforms.

There is a GitHub repo available with the code snippets in so you can follow along or just download the completed ones (link available towards the bottom).

The transforms will be written in Python (as that's the only one I can do) but Paterva do cater for a range of languages to be used as well as having some documentation available. Links for all the resources will be available at the bottom of this post and in the GitHub repo. I should at this point say the documentation for developing with Maltego is actually pretty good and this is not supposed to be a replacement, but more a compliment to what the guys at Paterva already provide.

For the purpose of these blog posts, I'm going to assume that you know what Maltego is and how to use it (even the basics is fine). So before we start writing the transform we are going to talk a little about the two key Maltego things you need for a transform.

**Entities & Transforms**

**Entities**

These the building blocks of a Maltego graph, they are both the start and the finish of a transform. The type of entity (e.g. Website) defines what transforms can be run against, once a transform has run then you typically get a different entity type as a result (and the cycle repeats).

For your first transform we are going to use two entities.

- **Website**
- **Phrase**

The website entity by defaults contains 'www.paterva.com' as the default value. You can change this to the website of your choice and you have the option of marking whether the website is "SSL Enabled" or not and the ports it's listening on.

[![Screen Shot 2014-09-21 at 09.04.00](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-09-04-00.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-09-04-00.png)

The Phrase entity is really simple, it literally a piece of text. It has no additional properties available.

[![Screen Shot 2014-09-21 at 09.13.28](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-09-13-28.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-09-13-28.png)

**Transforms**

Transforms are where the magic on Maltego occurs, they take your starting entity (in our case website) run some magic (well code) and return the results as another entity.

There are two types of transforms,

- **Local**
- **Remote**

**Local transforms** are ones that you have to install locally on your machine. All the components required to run that transform have to be installed on your machine (Python libraries for example). One of the benefits of using local transforms is that it widens your scope for what you can do (I'll explain that later), but it does limit the portability of those transforms (to a degree) as each person would need to install them (and the dependencies) before they can use them. There are a couple of ways to ease that pain but it's still a manual process.

**Remote transforms** run on TDS servers, the code for TDS transforms are executed on the TDS server so you remove yourself from having to worry about dependencies and libraries. The main benefit of TDS transforms is that they are truely portable, however they are limited to what you can do with them (well to my knowledge). For example if you have a custom transform that wants to read a file on your machine (or a local server) there isn't an easy way to execute that transform as TDS transform because the remote TDS server won't have access to that file. You would have to somehow get that file to a location that the TDS server could access it.

**Your First Transform**

So enough about the boring stuff lets get started. Your first transform is nice and simple we are going to use a Website entity and then write a transform to check for the robots.txt file, open it and then return each entry as a Phrase entity.

So the first thing you need is the Maltego Python library which can be found [HERE](http://www.paterva.com/web6/documentation/MaltegoTransform-Python.zip):

Extract the _MaltegoTransform.py_ to your working directory (where you are going to write the transform), for example mine is:

`/Users/Adam/Coding/Maltego/MyFirstTransform/`

Now create a new file in that directory called _GetRobots.py_

`pwd /Users/Adam/Coding/Maltego/MyFirstTransform touch GetRobots.py` Using your editor of choice lets open the file and start coding (I use Sublime Text 3).

First off lets add the Python sheebang and a comment about what this code is for.

`#!/usr/bin/env python`

`# Maltego transform for getting the robots.txt file from websites`

Now we need to import the Maltego transform library and the other Python libraries we need.

`from MaltegoTransform import * import sys import os import requests`

We will be using the _'requests'_  library to make the call to the website to retrieve the robots.txt file. We need the _'sys'_  library as the _sys.argv_ command is used to pass the value of the starting Maltego entity to the transform. So in our case the value of website (e.g. www.paterva.com) will be sent to our transform as _sys.argv\[1\]_.

Our next job is to create a variable to store that entity value in and something to store the output in.

`website = sys.argv[1] robots = []`

Now we are going to initialise the Maltego library so we can use it later on.

`m = MaltegoTransform()`

Still with me??

Ok lets write the code for using 'requests' to go and retrieve the robots.txt file (if it exists).

`try: r = requests.get('http://' + website + '/robots.txt') if r.status_code == 200: robots = str(r.text).split('\n') for i in robots: m.addEntity('maltego.Phrase', i) else: m.addUIMessage("No Robots.txt found..") except Exception as e: m.addUIMessage(str(e))`

m.returnOutput()

Now this isn't as bad as it looks (promise). Firstly we put everything in a _'try/except'_  bubble so that we have some error handling.

Next we make the initial GET request.

`r = requests.get('http://' + website + '/robots.txt')`

This adds the value we set on the Website entity and passes into the _'request.get'_  call with the 'robots.txt' added on the end.

We then add some logic in place to see if the robots.txt exists or not.

`if r.status_code == 200: robots = str(r.text).split('\n') for i in robots: m.addEntity('maltego.Phrase', i) else: m.addUIMessage("No Robots.txt found..")`

If the status code is **200** (that's a HTTP OK response), we store the output in the list variable 'robots' as text (instead of unicode) and then split into separate list entries based on each new line.

We then iterate through the list and for each line we create a new Maltego entity.

`m.addEntity('maltego.Phrase', i)`

This is where the magic happens. We initialise the Maltego Transform library earlier as _'m'_, we then use the function _'addEntity'_  to create an entity and set is as _'maltego.Phrase'_. We can then pass _'i'_ (which is from our _for_ loop) as the value to write to that entity.

There are two other statements that use part of the Maltego Transform library in our code.

`else: m.addUIMessage("No Robots.txt found..") except Exception as e: m.addUIMessage(str(e))`

The _'addUIMessage'_  function allows you to pass text back to the Maltego GUI when the transform is run (or running), it's ideal for error handling messages which is how we used it here. The _'else'_  statement is if the _status.code_ isn't **200** (which is usually a bad thing) and would return the message to the Maltego GUI and the _'except'_  statement will return any other error message that occurs (such as the website not being available etc.).

We finish the whole thing off my then calling the Maltego Transform library and telling it to return the output from our transform back into the GUI.

`m.returnOutput()`

Before we install this transform into Maltego we can actually check to make sure it works as expected. From your working directory simply run:

`python GetRobots.py www[dot]paterva[dot]com`

If it's all worked you should see something like this.

[![Screen Shot 2014-09-21 at 11.14.50](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-11-14-50.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-11-14-50.png)

What you might not have realised is that Maltego uses XML a lot of it's transforms and entities, the Maltego Transform library will automatically wrap your output up in the correct format so that displays properly within Maltego (after all who likes messing with XML).

Now that we have working code, we can add it into Maltego as our own transform. This is acutally really easy. Within the Maltego GUI, click

**Manage - Local Transforms**

You should get a popup window like the one below.

[![Screen Shot 2014-09-21 at 10.33.54](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-33-54.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-33-54.png)

On this first screen you need to enter some details about your transform.

**Display name** - This is how your transform appears in Maltego **Description** - A brief description about what your transform does **Transform ID** - A unique reference that Maltego uses (you can't change it once you've created it) **Author** - That's you.. **Input entity type** - We need to choose 'Website', this means that our entity will only run on Website entities (which is good for us) **Transform set** - Transform sets are a collection of transforms that relate to an entity type. I've chosen 'none' just for this example. You can create your own transform sets which we will cover in later posts

[![Screen Shot 2014-09-21 at 10.34.53](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-34-53.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-34-53.png)

Click Next....

[![Screen Shot 2014-09-21 at 10.38.51](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-38-51.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-38-51.png)

This is where we set the meat of the transform, as in actually point it to your code.

**Command** - This is the path to your interpretor so in my case '/usr/bin/python' (I'm using a Mac though) **Parameters** - This is where you point it to your actual GetRobots.py file **Working Directory** - Set this to your working directory.

[![Screen Shot 2014-09-21 at 10.40.48](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-40-48.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-40-48.png)

Click Finish....

So you've now added your transform into Maltego.. shall we run it??

Drag a website entity onto an empty graph in Maltego. Leave the default www.paterva.com address in there for now. Right click on the entity, then select

**Run Transform - Other Transforms - GetRobots**

[![Screen Shot 2014-09-21 at 10.45.44](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-45-44.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-45-44.png)

All being well you should get a number of Phrase entities returned containing the same output as when you run your script manually??? (fingers crossed)

[![Screen Shot 2014-09-21 at 10.48.00](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-48-00.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-10-48-00.png)

Add some other websites and see what you get back. If like me you are only using the Community Edition of Maltego then don't be worried if you only get 12 entities returned.

Congratulations you've just created your first Maltego Transform.. it wasn't that hard now was it, so what you waiting for?? :)

In the next post in this series we will explore some more of the features available when returning entites and look at how to use additional properties on an entity when running a transform and how to return additional properties once a transform is complete.

**Link to GitHub repo:**

https://github.com/catalyst256/MyFirstTransform

**Links to Maltego Developer documentation:**

http://www.paterva.com/web6/documentation/developer.php - Main Developer Site

http://www.paterva.com/web6/documentation/developer-local.php? - Local Transforms http://www.paterva.com/web6/documentation/localTransforms-SpecIII.pdf - Local Transform Specifications http://www.paterva.com/web6/documentation/MaltegoTransform-Python.zip - Python Library
