---
title: "Maltego: Going Remote.."
date: "2014-10-08"
categories: 
  - "archive"
---

Today we are going to talk about TDS (Transform Distribution Server) transforms, as we discussed in earlier posts these bad boys run on a remote server and allow you to easily share your transforms with anyone (well people you want to).

There are some considerations you need to bear in mind when building TDS transforms.

The server you are hosting them on needs to have all the dependencies required to run (common sense really) If you are hosting transforms that are public and they spam the crap out of someone else's systems you are probably the one going to get the blame Use a local development server to test them before pushing to live and think about using a private code repository (trust me it's painful otherwise)

You also need to think about what you want your TDS transform to do, for example TDS transforms (Public ones) are great for web-based queries (API calls etc) but you probably wouldn't want to run anything that was CPU intensive or needed an asset uploaded from your local machine in order to work. You can purchase an internal TDS server from Paterva which might remove some of these restrictions (not sure how they work to be honest).

I've not written a lot of TDS transforms (at the moment) but in this post we are going to take the GetRobots local transform we've been working on and move it to a TDS server. In order to use TDS transforms you need the following:

- An account on Paterva's Public TDS server (https://cetas.paterva.com/TDS/)
- An internet facing server to host the transforms (needs to be able to run WSGI based applications)
- The Maltego TDS bundle (http://www.paterva.com/TRX\_Ubuntu.tgz)
- The awesome TDS documentation from Paterva (http://www.paterva.com/web6/documentation/TRX\_documentation20130403.pdf)
- Some patience

I'm not going to cover building a web server to host the WSGI application as the documentation linked above does a pretty good job (it worked for me so you should be fine). What we are going to look at today is how to take the local transform and convert it to a TDS transform and what that means in terms of code changes (it's not that much actually).

So going back to the very first iteration of the GetRobots transform, lets just have a look at the code we wrote.

\[code language="python"\] #!/usr/bin/env python

\# Maltego transform for getting the robots.txt file from websites

from MaltegoTransform import \* import os import requests

website = sys.argv\[1\] m = MaltegoTransform() robots = \[\]

try: r = requests.get('http://' + website + '/robots.txt') if r.status\_code == 200: robots = str(r.text).split('\\n') for i in robots: m.addEntity('maltego.Phrase', i) else: m.addUIMessage("No Robots.txt found..") except Exception as e: m.addUIMessage(str(e))

m.returnOutput() \[/code\]

Only 24 lines of code, the good news is that we are not going to have to change a lot to make this a TDS transform. Lets jump straight in and see what the TDS version will look like.

\[code language="python"\] import requests from Maltego import \*

robots = \[\]

def trx\_GetMeRobots(m): TRX = MaltegoTransform() try: website = m.Value TRX.addUIMessage(str(website)) url = 'http://' + website + '/robots.txt' TRX.addUIMessage(str(url)) r = requests.get(url) if r.status\_code == 200: robots = str(r.text).split('\\n') for i in robots: ent = TRX.addEntity('maltego.Phrase', i) else: TRX.addUIMessage("No Robots.txt found..") except Exception as msg: TRX.addUIMessage("Error:"+str(msg),UIM\_PARTIAL) return TRX.returnOutput() \[/code\]

See told you it wasn't that bad, lets look at the important changes.

`from Maltego import *`

Here we have change the Maltego module we import, the TDS based transforms use a different Python library (which has better functionality in some areas).

`def trx_GetMeRobots(m):`

For the TDS transforms we use a Python function, the name is of your choice but you need to ensure you have the '(m)' defined as this is used to pass the entity value(s) from your Maltego graph into the transform.

`TRX = MaltegoTransform()`

We've used TRX as the variable for the MaltegoTransform() call rather than 'm' as we already used 'm' in the function and it helps me remember its a TDS transform. Again we use a try/except loop and as you can see the only real difference is that I've added some 'UIMessage' updates along the way (which was more for debugging).

`TRX.addUIMessage(str(website)) TRX.addUIMessage(str(url))`

So hopefully by this point you should have the following:

- A web server configured to work with the TDS package
- Your new TDS transform written

The next part is to tweak the TRX.wsgi code to make use of our new transform. The code is below:

\[code language="python"\] import os,sys

\# To run in debug, comment the next two lines and see # end of this file

os.chdir(os.path.dirname(\_\_file\_\_)) sys.path.append(os.path.dirname(\_\_file\_\_))

from bottle import \* from Maltego import \*

\# Transform Libraries goes here: from GetRobots import \*

\##### TRANSFORM DISPATCHER starts here -->

#-----> GetRobots - Collect contents of robots.txt from a website @route('/GetRobots', method='GET') def GetMeRobots(): if request.body.len>0: return(trx\_GetMeRobots(MaltegoMsg(request.body.getvalue())))

\## ---> Start Server ## To start in debug mode: Comment the line below... application = default\_app()

\## ... and uncomment line below #run(host='0.0.0.0', port=9001, debug=True, reloader=True) \[/code\]

Most of the code above is the default so I'm just going to cover the bits I've changed. The first bit is this:

`from GetRobots import *`

Essentially this imports our GetRobots code so it's available to be called by the transform dispatcher. Next we create a new route so we can call the transform from within Maltego.

\[code language="python"\] #-----&gt; GetRobots - Collect contents of robots.txt from a website @route('/GetRobots', method='GET') def GetMeRobots(): if request.body.len&gt;0: return(trx\_GetMeRobots(MaltegoMsg(request.body.getvalue())))\[/code\]

The _'@route('/GetRobots', method='GET')'_ statement is the URL that you want to reference your transform on, the default for the _'method'_ is "ANY" but I've changed it to "GET" so it's not as open. The _'def GetMeRobots():'_ function is just to hold our call to the transform, the first line of the function _'if request.body.len>0:'_ makes sure that we only perform the call to the transform if the body length isn't blank (or zero).

The important part of the next line of code is the call to your transform (based on function name) _'return(trx\_GetMeRobots(MaltegoMsg(request.body.getvalue())))'_. You need to ensure that the name of the function, in this case _'trx\_GetMeRobots'_ matches then name of your function for your transform.

Still with me? So we now have the following all setup and ready to roll.

- - A web server configured to work with the TDS package
    - Your new TDS transform written
    - A modified TRX.wsgi configured for your new transform

So there are two steps left, we now need (using our TDS account) to set up the Seed & Transform. This is a quick process. Log into your TDS account (https://cetas.paterva.com/TDS/)

[![Screen Shot 2014-10-08 at 12.23.51](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-12-23-51.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-12-23-51.png)

Select "Seeds" from the menu, then click on the "Add Seed" button.

[![Screen Shot 2014-10-08 at 12.24.51](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-12-24-51.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-12-24-51.png)

We need to fill in some basic details.

Seed Name - A name for your seed, you can create seeds based on the transform roles or environments Seed URL - Create a unique name which will act as the path to your transforms. If you want them "private" make the URL suitably long and stupid. Transforms - This will be blank at the moment

Click on "Add Seed" and then all being well you should get a message saying the seed was added successfully. Next we need to add the transform, there isn't an option for "Home" so you need to click on the hyperlink at the top of the page labelled 'Paterva Transform Distribution Server 0.1'.

[![Screen Shot 2014-10-08 at 12.48.46](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-12-48-46.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-12-48-46.png)

Back on the home page, click on 'Transforms', then click on "Add Transform".

[![Screen Shot 2014-10-08 at 12.50.41](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-12-50-41.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-12-50-41.png)

We now need to add in some details, we will do the minimum just to save on time (and me typing).

Transform Name - The name of the transform. It doesn't support spaces or non alpha numeric characters. Transform URL - This is the URL to YOUR server running the transform and the name of the path to the transform (based on what you entered in the TRX.wsgi file). Input Entity - For our purpose that is maltego.Website, you can add your own entities as well Owner - This is based on your TDS account details. Disclaimer - By default a disclaimer is added but you can add your own text as well. Description - An optional area to put a more detail about what your transform does. Version - Again optional, good if you are making big changes just so people are aware. Author - Again pulls these details from your TDS account. Debug - Tick to enable debugging (explains itself really). Transform Settings - We haven't defined any and I haven't worked out what they are really for. Seeds - You need to select the seed you created, this adds the transform to that seed.

[![Screen Shot 2014-10-08 at 13.03.14](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-03-14.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-03-14.png)

Click "Add Transform", and again you should get a message something like 'Successfully Added \[TransformName\]'

The final task is to add the Seed into Maltego so it will add the transform and make it available. This is a nice simple process and you just need the full Seed URL, here's one I built earlier.

https://cetas.paterva.com/TDS/runner/showseed/49f742b02c27ab2a88b4299a4fb56f31abc6298c

Load up Maltego and click:

Manage - Discover Transforms - Discover Transforms (advanced)

[![Screen Shot 2014-10-08 at 13.06.23](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-06-23.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-06-23.png)

You will now get a little wizard which only needs two pieces of information.

Name - You can call this what you what URL - The Seed URL

[![Screen Shot 2014-10-08 at 13.08.24](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-08-24.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-08-24.png)

Click Add and then Next

[![Screen Shot 2014-10-08 at 13.08.31](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-08-31.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-08-31.png)

The wizard will go off and verify all the Seeds are available and then present you with a list.

[![Screen Shot 2014-10-08 at 13.08.57](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-08-57.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-08-57.png)

All you need to do on this is click Next to carry on (they should all be ticked). This will produce a list of all the transforms that will be loaded into Maltego. Scroll through the list and you should be able to see this.

[![Screen Shot 2014-10-08 at 13.11.11](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-11-11.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-11-11.png)

Click Next and it will import tha transforms. All being well you should get a similar window to the one below and that is it.

[![Screen Shot 2014-10-08 at 13.12.22](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-12-22.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-12-22.png)

Now to test it, create a new graph and drag a Website entity onto it, right-click and select.

Run Transform - Other transforms - GetRobots

[![Screen Shot 2014-10-08 at 13.13.25](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-13-25.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-13-25.png)

The first time you run a TDS transform you will be prompted to accept the disclaimer which if you want it to run you should accept.

[![Screen Shot 2014-10-08 at 13.14.41](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-14-41.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-14-41.png)

This will run the GetRobots transform from the remote TDS server and should produce the same results as the local transform we wrote.

[![Screen Shot 2014-10-08 at 13.15.41](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-15-41.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/10/screen-shot-2014-10-08-at-13-15-41.png)

So there you go, your first TDS transform now wasn't that fun. The final blog post in the Maltego series will go over exporting your entities, transform sets and the such so that other people can use them.
