---
title: "Maltego: Enhance your....entity"
date: "2014-09-23"
categories: 
  - "archive"
---

So over the weekend I started off my series on how you can create your own Maltego transforms, quickly and without much effort. Today I wanted to expand on that and look at how we can expand on the original 'GetRobots' transform to make use of some of the additional properties within an entity.

If you remember our **GetRobots** transform made use of two builtin Maltego entities. The starting entity (the one you run the transform on) was the Website entity. The Website entity has two additional properties that can be set.

These are:

- Ports
- SSL Enabled

[![Screen Shot 2014-09-21 at 09.04.00](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-09-04-00.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-21-at-09-04-00.png)

By default these are set to Port 80 and SSL isn't enabled. The _'Ports'_ property allows you to enter an array of different ports that your target website might be running on, the _'SSL Enabled'_  is essentially a True or False option. In order to make use of these values (or allow for them to be changed) we need to change our original transform slightly.

**NOTE:** The python code is this transform might not be pretty but its more to show you how to call the additional properties rather than impress you with my Python skills.

OK, so in the GitHub repo (found [HERE](https://github.com/catalyst256/MyFirstTransform)), you will see I've created a new file called **GetRobots-mk2.py** this is the same base code as the original transform but with some tweaks. So lets go through them;

`from MaltegoTransform import * import requests`

m = MaltegoTransform() m.parseArguments(sys.argv)

So first off we removed the `'website = sys.argv[1]'` line and replaced it with `'m.parseArguments(sys.argv)'`. This is essentially telling the transform to make use of all the entity properties (regardless of how they are set). We now need to call these properties and store them in variables so we can use them.

`website = m.getVar('fqdn') port = m.getVar('ports') port = port.split(',') ssl = m.getVar('website.ssl-enabled') robots = []`

Now you may be wondering where I got the value names from, there are two places you can look. The first is this PDF from Paterva which explains the core entities and the values available which you can find [HERE](http://www.paterva.com/web6/documentation/TRX_documentation20130403.pdf).

The second is from within Maltego (this is the way I tend to do it), click on _Manage - Manage Entities_ scroll down to the Website entity, click on the three little blue dots on the right, then click on the _'Additional Properties'_ tab on the top.

On the left you will see a list of properties, if you select one the top right window will change to show you the details for that property. The _'Name'_  value is the important value and when you are pulling out these properties in your transforms it's the one that will case you headaches (well it does me anyway).

[![Screen Shot 2014-09-22 at 20.59.29](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-22-at-20-59-29.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-22-at-20-59-29.png)

So back to the code. We use the _'getVar'_  and the associated entity name (for that property) to store the values in a variable. The _'ports'_  properties is stored as a list so we can iterate over it later, the _'ssl-enabled'_  is a boolean so will return either **True** or **False**.

The rest of the code is tweaked slightly to make use of these new variables, which you can see below.

`try: for c in port: if ssl == 'true': url = 'https://' + website + ':' + str(c) + '/robots.txt' r = requests.get(url) if r.status_code == 200: robots = str(r.text).split('\n') for i in robots: ent = m.addEntity('maltego.Phrase', i) ent.addAdditionalFields("url","Original URL",True,url) else: m.addUIMessage("No Robots.txt found..") else: url = 'http://' + website + ':' + str(c) + '/robots.txt' r = requests.get(url) if r.status_code == 200: robots = str(r.text).split('\n') for i in robots: ent = m.addEntity('maltego.Phrase', i) ent.addAdditionalFields("url","Original URL",True,url) else: m.addUIMessage("No Robots.txt found..") except Exception as e: m.addUIMessage(str(e))`

We've added two extra components, first we create a _'for'_ loop and iterate through each value stored in _'ports'_. This allows our transform to cater for multiple (or single) ports. We then throw in some _'if'_  logic to see if the _'ssl-enabled'_  property is set. If it is we change the url to be _'https'_ and chuck in the port number for good measure. Like I said, it's not pretty but it works.. :)

The final addition piece to the puzzle is we have declared some additional properties in the _'Phrase'_  which if you check yourself will see don't exist. The _'Phrase'_  entity has only one value, but Maltego allows you to create dynamic values which you can then use later on in other transforms. To do this, we changed the way we create the entity, in order to use some of the more 'advanced' features of 'addEntity' we created a variable called _'ent'_  which then allows use to add additional properties among other things.

`ent = m.addEntity('maltego.Phrase', i) ent.addAdditionalFields("url","Original URL",True,url)`

The 'addAdditionalFields' function has these options (taken from Paterva's documentation).

**addAdditionalFields(fieldName=None,displayName=None,matchingRule=False,value=None)** Set additional fields for the entity **fieldName:** Name used on the code side, eg displayName may be "Age of Person", but the app and your transform will see it as the fieldName variable **displayName:** display name of the field shown within the entity properties **matchingRule:** either "strict" for strict matching on this specific field or false **value:** The additional fields value

If you install the new transform following the previous posts instructions and run it you will see that the new additional property is created as a _'Dynamic Property'_.

[![Screen Shot 2014-09-22 at 21.26.12](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-22-at-21-26-12.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-22-at-21-26-12.png)

[![Screen Shot 2014-09-22 at 21.33.21](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-22-at-21-33-21.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-22-at-21-33-21.png)

If you were to then write a new transform for our new 'Phrase' entity you would call the dynamic property using.

`ent.addAdditionalFields("**url**","Original URL",True,url) url = m.getVar('**url**')`

Hope that all makes sense?? Next time we are going to look at **Maltego Machines**...
