---
title: "Maltego: Creating entities.."
date: "2014-09-29"
categories: 
  - "archive"
---

So far in this blog series we have talked about how you can create new Maltego transforms and machines easily. Today we are going to cover creating new entities for you to use and/or abuse.

Creating new entities is (like creating transforms) quite easy and painless, there are a couple of things you need to keep in mind but we will cover those as we go.

To create a new entity within Maltego click:

**Manage - Manage Entities**

[![Screen Shot 2014-09-29 at 12.55.55](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-12-55-55.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-12-55-55.png)

You will now get the Entity Manager window.

[![Screen Shot 2014-09-29 at 12.56.36](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-12-56-36.png?w=272)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-12-56-36.png)

Once again (like most things in Maltego) you get a '**New Entity Wizard**'. Lets have a look at the options you have to supply.

- **Display name** - This is the name of your entity as it will appear in the '**Palette**' within Maltego, try to keep it short and sweet
- **Short description** - A quick one liner of what the entity is for
- **Unique type name** - Again like your transform make it unique and relevant to the entity type
- **Inheritance**
    - **Base Entity** - Inheritance allows you to create a new entity but '_inherit_' the properties of an existing entity. This means if you want to create a new entity for '**IPv4 Address**' which has additional fields for your needs you can '_inherit_' the base properties and the transforms that can be run against the base entity into your new entity (it's actually a really cool feature).
- **Icons** - For each new entity you create you have to set an icon. You can either use a built-in one or use a new one. For our purpose we are going to use an existing one.

Click on **Browse**

[![Screen Shot 2014-09-29 at 12.56.49](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-12-56-49.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-12-56-49.png)

Make sure you have selected the 'Default' tab at the top, you should now see a whole stack of icons you can pick.

[![Screen Shot 2014-09-29 at 13.06.17](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-06-17.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-06-17.png)

Pick an icon, any icon and then click on 'OK'. You should hop back to the Entity Wizard and your Icon image should now be populated with your choice.

[![Screen Shot 2014-09-29 at 13.12.17](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-12-17.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-12-17.png)

Still with me?? Cool right-click 'Next' and we progress onto the next page. This is where we have to define the properties for the entity. If you use Inheritance this is typically grayed out as it will want to use the existing properties from the entity you are inheriting from (but you can add additional ones if you like). Here we need to define the main property for the entity, this is the one that we will set by default each time we return this entity type.

[![Screen Shot 2014-09-29 at 13.14.37](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-14-37.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-14-37.png)

A lot of the fields are set by default but we can go through them just so you know what they are for.

- **Property display name** - This is the name of the property when you open the entity. I tend to keep it the same as the Display name as I set as this is the main property and it just makes sense to me to have them the same.
- **Short description** - This is a short description of the entity property (rather than the description of the entity)
- **Unique property name** - I have a habit of setting this to the same as the entity unique type name (when using Canari) but this is what you will need to call in your transform code when setting the value.
- **Data type** - By default this is a 'string' value, however you can set the following types:
    - string
    - int
    - date
    - double
- **Sample value** - If you drag your entity into a graph, this is the value that will get set and display for this property.

We will just tweak a couple of values before moving on.

[![Screen Shot 2014-09-29 at 13.24.01](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-24-01.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-24-01.png)

Now we are done, click on 'Next' and lets carry on. We can now add our new entity into an existing or new category.

[![Screen Shot 2014-09-29 at 13.24.27](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-24-27.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-24-27.png)

You can add your new entity into one of the existing categories which are listed below (and are the ones listed in your palette within Maltego).

- Devices
- Infrastructure
- Locations
- Penetration Testing
- Personal
- Social Network

For our purpose we are going to create a new category called '**MyFirstTransform**'. If you want to create a new category just type it into the box.

[![Screen Shot 2014-09-29 at 13.26.50](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-26-50.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-26-50.png)

Click **Finish**.

All being well in your '**Palette**' pane on the right you should see the new category called '**MyFirstTransform**' and in the entity list you should see the '**Robot Entry**' entity.

[![Screen Shot 2014-09-29 at 13.27.41](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-27-41.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-27-41.png)

Before we move on I just want to cover creating additional properties for our new entity. This is handy if you want to use more than one property in executing a transform or storing in your entity. From within the Entity Manager click on the 3 little blue dots on the right of the new entity.

[![Screen Shot 2014-09-29 at 13.30.57](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-30-57.png?w=272)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-13-30-57.png)

Click on the '**Additional Properties**' tab at the top, you will see the original property that we created so we are going to add another (just for giggles). Click on '**Add Property**' in the top right.

[![Screen Shot 2014-09-29 at 14.20.09](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-14-20-09.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-14-20-09.png)

We are going to add a new property for the dynamic property we called in the origin GetRobots-mk2 transform. Type the following into the available fields.

**Name** - properties.orgurl **Display Name** - Original URL **Type** - String

[![Screen Shot 2014-09-29 at 14.21.59](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-14-21-59.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-14-21-59.png)

The field name is used for the unique name, and display name is the "friendly description".

You may notice that the list of available types is a lot longer then when we created the entity. I'm not sure why but it is, 'string' for example has to options:

- string
- string\[\]

The _'string'_ type is for a single string value, while the _'string\[\]'_ allows you to provide a range of string values (like the 'ports' property in the Website entity).

Click **OK** to add the new property and then click **OK** again to return to the Entity Manager.

[![Screen Shot 2014-09-29 at 14.26.11](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-14-26-11.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-14-26-11.png)

Awesome, isn't this fun. Now we need to tweak our existing transform to make use of the new entity. Lets have a look at the code again to refresh our memory.

\[code language="python"\] #!/usr/bin/env python

\# Maltego transform for getting the robots.txt file from websites

from MaltegoTransform import \* import requests

m = MaltegoTransform() m.parseArguments(sys.argv)

website = m.getVar('fqdn') port = m.getVar('ports') port = port.split(',') ssl = m.getVar('website.ssl-enabled') robots = \[\]

try: for c in port: if ssl == 'true': url = 'https://' + website + ':' + str(c) + '/robots.txt' r = requests.get(url) if r.status\_code == 200: robots = str(r.text).split('\\n') for i in robots: ent = m.addEntity('maltego.Phrase', i) ent.addAdditionalFields("url","Original URL",True,url) else: m.addUIMessage("No Robots.txt found..") else: url = 'http://' + website + ':' + str(c) + '/robots.txt' r = requests.get(url) if r.status\_code == 200: robots = str(r.text).split('\\n') for i in robots: ent = m.addEntity('maltego.Phrase', i) ent.addAdditionalFields("url","Original URL",True,url) else: m.addUIMessage("No Robots.txt found..") except Exception as e: m.addUIMessage(str(e))

m.returnOutput()\[/code\]

To make use of the new entity we just need to change two lines of code (there are two instances of this though). The original lines code of code that define your entity returned is below:

`ent = m.addEntity('maltego.Phrase', i) ent.addAdditionalFields("url","Original URL",True,url)`

We simply need to change this to our new entity type and property values.

`ent = m.addEntity('adammax.robotentry', i) ent.addAdditionalFields("property.orgurl","Original URL",True,url)`

This is what the code should look like now.

\[code language="python"\] #!/usr/bin/env python

\# Maltego transform for getting the robots.txt file from websites

from MaltegoTransform import \* import requests

m = MaltegoTransform() m.parseArguments(sys.argv)

website = m.getVar('fqdn') port = m.getVar('ports') port = port.split(',') ssl = m.getVar('website.ssl-enabled') robots = \[\]

try: for c in port: if ssl == 'true': url = 'https://' + website + ':' + str(c) + '/robots.txt' r = requests.get(url) if r.status\_code == 200: robots = str(r.text).split('\\n') for i in robots: ent = m.addEntity('adammax.robotentry', i) ent.addAdditionalFields("property.orgurl","Original URL",True,url) else: m.addUIMessage("No Robots.txt found..") else: url = 'http://' + website + ':' + str(c) + '/robots.txt' r = requests.get(url) if r.status\_code == 200: robots = str(r.text).split('\\n') for i in robots: ent = m.addEntity('adammax.robotentry', i) ent.addAdditionalFields("property.orgurl","Original URL",True,url) else: m.addUIMessage("No Robots.txt found..") except Exception as e: m.addUIMessage(str(e))

m.returnOutput()\[/code\]

Save the changes and then within Maltego run the **GetRobots** transform again, and you should see your new entity spring into life (fingers crossed).

[![Screen Shot 2014-09-29 at 14.35.50](http://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-14-35-50.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/screen-shot-2014-09-29-at-14-35-50.png)

So there you go, the basics behind creating your own entities. Next time we are going to talk about exporting all the Maltego goodness we have created.
