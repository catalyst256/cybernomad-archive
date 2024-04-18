---
title: "Honeypot: Kippo Pi"
date: "2013-05-14"
categories: 
  - "archive"
---

I'm sure you are all aware of the awesome RaspberryPI machines and no doubt you've thought of a number of fun things to run on these little machines whether it's a media server, home automation system, web server or a penetration testing drop box. Up until last week I had resisted the urge to buy one just for the sheer fact I couldn't think of anything "interesting" to do with one, I mean there are only so many installs of Backtrack/Kali you can have (I even have Backtrack on my phone).

With my focus at the moment on network forensics and malware I decided to get a RaspberryPI and see how it works as a HoneyPot. The first one I've installed is Kippo and this post is about how to get Kippo running on the RaspberryPI.

Kippo is a low interactive SSH honeypot that allows you to capture SSH based attacks and see what those evil hackers are up to. I'm not going to bore you with a long explaination of Kippo because well it's early and I've not had much coffee, instead you can click [HERE](https://code.google.com/p/kippo/) and go have a read yourself.

The following instructions aren't the product of my own mind, rather they have been taken from **Leon van der Eijk** (or **@lvdeijk** for short) awesome BSides London Kippo Workshop crib sheet.

So to start with you need the following (or close to):

1. RaspberryPI (kinda obvious but..I have the model B)
2. SD Card (I bought a 32GB SanDisk SDHC card, because I want to install other stuff on it)
3. Physical network connection for your RaspberryPI (to download stuff)
4. A home router/firewall that you can do port forwarding on.
5. Coffee (or beverage of your choice)

So first off we need to install some of the dependencies to get Kippo running. SSH onto your RaspberryPI and as the pi user run the following command:

`sudo apt-get install subversion python-twisted python-mysqldb mysql-server apache2`

The mysql-server and apache2 packages are so we can log Kippo to MySQL and run the kippo-graph website (nice pretty pictures). If you don't want that functionality just don't install them (but I would if I were you).

**NOTE:** Remember the MySQL password you enter during the install as you will need that later.

So now we need to get a copy of Kippo, we are going to use SVN for this:

`svn checkout http://kippo.googlecode.com/svn/trunk/ kippo-read-only`

Now your choice for installation is your own, by default this command will download Kippo into **/home/pi/kippo-read-only**.

Now if you installed MySQL as I suggested we need to do some database magic. Basically we are going to create a new database called Kippo and then assign a user and password for Kippo to use. So here we go:

First log into MySQL:

`mysql -h localhost -u root -p`

You should be prompted to enter your password (now you did remember it didn't you).

Once logged in you need to create your database:

`create database kippo;`

And then assign the necessary rights:

`GRANT ALL ON kippo.* TO 'kippo'@'localhost' IDENTIFIED BY 'Kippo-DB-pass';`

**NOTE:** The password for the kippo user within the kippo database is 'Kippo-DB-pass' you can change it if you want.

You can now exit from MySQL using:

`exit (tricky I know)`

OK still with me? Right now we need to populate the database 'kippo', browse to this folder location:

**/kippo-read-only/doc/sql**

Within this folder you should find a file called **mysql.sql** now we need to load that into the database:

`mysql -u kippo -p use kippo; source mysql.sql;`

If that worked without errors you should now have a populated database, you can check by typing this within the MySQL prompt:

`show tables;`

If that returns some tables, one should be called TTY (I think, like I said it's early) then we are all good and you can type:

`exit`

To exit out. We now need to create a **kippo.cfg** file, don't panic it's easy. From the root of the **/kippo-read-only** folder type this:

`cp kippo.cfg.dst kippo.cfg`

Now we need to edit the kippo.cfg file with the database details. Using your favourite command line editor (nano is installed so I used that). Navigate the file and find the **\[database\_mysql\]** section (should all be commented out), un-comment all the fields (including the \[database\_mysql\] one) and modify the values so it looks something like this:

`[database_mysql] host = localhost database = kippo username = kippo password = Kippo-DB-pass`

So we are nearly there. Now kippo uses port **2222** to run its honeypot on, in order to send evil hackers to that port you need to use your home router (which hopefully can do port forwarding) to send all traffic for port 22 to 2222. I'm not going to explain how to do this because everyone's router is different. So go ahead and configure that port forwarding ready for when we start kippo.

If by chance you have put your honeypot directly on the internet you need to the following additional steps:

`sudo apt-get install iptables sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222 sudo iptables-save > /etc/iptables.rules`

Port forwarding all sorted? Good now we need to change the default port for your ssh server to something a little "higher". Use this command to change the listening port for the sshd to **65534**:

`sudo sed -i 's:Port 22:Port 65534:g' /etc/ssh/sshd_config`

And then restart your ssh service (you will get kicked off):

`sudo /etc/init.d/ssh restart`

Back with me? Cool right so essentially you now have a working a Kippo honeypot (hopefully). You can actually at this point start it up. Again it's a simple process from your **/kippo-read-only** folder run the following command:

`sudo ./start.sh`

You can check it's loaded properly by looking in the **/kippo-read-only/log/kippo.log** file which should show it starting up properly and you can then run this command to check:

`sudo netstat -antp | grep 2222`

Which should return an entry saying port 2222 is listening a python process is running.

**NOTE:** The kippo.log file will also contain all the connection information and any commands that are run. The root password for the Kippo honeypot is '**123456**' you can change this by editing the **/kippo-read-only/data/userdb.txt** file and restarting kippo.

Now we are going to finish this off by installing the kippo-graph application that gives you lots of pretty pictures.

So lets install the extra bits we need:

`sudo apt-get install libapache2-mod-php5 php5-cli php5-common php5-cgi php5-mysql php5-gd sudo /etc/init.d/apache2 restart`

That should have hopefully installed all the necessary php components you need to run kippo-graph, now let's get kippo-graph:

`wget http://bruteforce.gr/wp-content/uploads/kippo-graph-0.7.4.tar sudo mv kippo-graph-0.7.4.tar /var/www cd /var/www sudo tar xvf kippo-graph-0.7.4.tar --no-same-permissions cd kippo-graph sudo chmod 777 generated-graphs`

Before you open the website you need to edit the /var/www/config.php file with your database properties from earlier, I can't remember where exactly but its not a big file. Once you've done that you are ready to browse to:

`http://<raspberrypi_ipaddress>/kippo-graph/index.php`

That should be it, once you have some data you can populate the graphs and "see" what the evil hackers are up to.

So I'm hoping that this has all worked and you now have a small discrete, energy-efficient honeypot running on your home network. Mine has been running since last night and I've had 4 connections to port 22 but no login attempts so far (thought evil hackers didn't sleep).

Give me a shout if something doesn't work and I will try my best to help you out.
