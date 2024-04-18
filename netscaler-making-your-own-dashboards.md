---
title: "Netscaler: Making your own dashboards"
date: "2011-10-20"
categories: 
  - "archive"
---

[![](images/robot-trans-mini.jpg "Robot-Trans-Mini")](http://theitgeekchronicles.files.wordpress.com/2012/02/robot-trans-mini.jpg) Welcome reader(s), as you will come to learn I will probably post a lot about Citrix Netscalers. The main reason for this is because where I work we have 9 Netscalers in total and I have the privilege of being the "Expert" on these wonderous hardware load balancers (ok that's enough fluffy talk about Netscalers).

The Citrix Netscalers are a very good piece of kit in terms of what they do, one side that I personally think they are lacking is on the reporting. The appliances have some built-in reporting that allows you to see some historical information and a dashboard for "live" information.

Aside from the built-in reporting Citrix have released a product called Citrix Command Center, this allows you to centrally manage some functions of the Citrix Netscaler (or Citrix WANscaler) in one Dashboard. Command Center allows you so view service/service group/load balancer status, automatically download Netscaler config files from your appliances, record Appliance Events and Alerts as well as the ability to execute predefined or custom scripts from Command Center to your appliances.

Don't get me wrong this is useful in it's own right and is a good addition to your estate if you have Netscaler appliances, however I needed something a little less complex for our 24/7 Control Center to be able to see a read only portal for the relevant information needed for support.

The good thing about Command Center (in my opinion) is that it runs on Microsoft SQL Server, which means I can use my limited SQL skills to pull out the data I want for my dashboards and throw it into a web page (again I'm not a web developer). The main areas of focus for my dashboards where:

**1\. Last 50 Events 2. GSLB Service Status (we use GSLB for site fail-over) 3. Service/Service Group Status** **NOTE:** This article only covers the SQL part of making your own Dashboard, I will leave the web page design to my readers as I've only worked on IIS (unless there are a lot of requests for this).

My first task was to get my head around the database schema for Command Center, thanks to the wonder of Visio I "reverse engineered" the database into a Visio diagram so I could refer to it without having to go through each table. The database schema can be found [HERE](http://theitgeekchronicles.files.wordpress.com/2011/10/commandcentre-database-scheme.png) to save you the trouble (it's a PNG to save any issues with not having Visio installed).

Right so we have our database, copy of the database schema and we know what we are looking for. Time to find that data and put it in the format I want (feel free to change this).

**NOTE:** I've started and ended each query with **\[Query Start\]** and **\[Query End\]** you do not need to include these in your query. Where I have entered something in UPPERCASE with a \_ separating words needs to be replaced with your relevant information (just query the table to see all the results to get a better idea).

1. **Last 20 Events** - This is a very useful report, not only does it show the state changes but it also any config changes, logins etc etc. If someone reports a "Netscaler Issue" this is the first place I check and you can change to show as many events as you want. The SQL Query for this is a straight forward one as seen below.

**\[Query Start\]** _\--Netscaler Command Center --Last Top 50 Events displayed in date order (most recent first) --Written by IT Geek 20/10/2011_

select Top 50 --The +3600 on the DATEADD is to allow for Timezone change, it might not be necessary for your appliance DATEADD(s, TTime/1000+3600, '19700101') as \[Date & Time\], --Within this CASE statement I change the native IP address of the Netscaler Appliance to a more "Friendly Name" case Source WHEN 'NETSCALER\_IP' THEN 'FRIENDLY\_NAME' --I've changed the headers to more friendly headings using the AS statement End as \[Source\], text as \[Events\], entity as \[Description\] --The table for the Events is called "Event" from Event --You can change this WHERE statement to exclude entries you don't want to see or aren't interested in or you can just remove it. where entity not like 'Power%' and text not like 'User: #nsinternal#' order by \[Date & Time\] desc **\[Query End\]**

Hopefully (and I have tested them) this should display in SQL Query as the last 50 events from Command Center.

1. **GSLB Service Status** - So in our configuration we have GSLB configured to allow our Active/Passive configuration to be failed over between our data centres. The GSLB dashboard shows which service on which site is either UP, Down, Out of Service or Going Out Of Service (these are the reported status for the Netscaler).

The SQL query for this is a bit more complex, on my dashboard I use one query for one site and another for the remote site and then just display them side by side.

**\[Query Start\]** _\--Netscaler Command Center --GSLB Service Status --Written by IT Geek 20/10/2011_

SELECT \* FROM ( SELECT \*, ROW\_NUMBER() OVER (PARTITION BY \[Netscaler\], \[GSLB Name\] ORDER BY \[Last Polled Time\] DESC) AS RecentFirst FROM ( Select DISTINCT --Within this CASE statement I change the native IP address of the Netscaler Appliance to a more "Friendly Name" --because they are a HA pair they are displayed as 2 IP's for each site case NSIP WHEN 'NETSCALER\_IP' THEN 'FRIENDLY\_NAME' WHEN 'NETSCALER\_IP' THEN 'FRIENDLY\_NAME' ELSE 'UNKNOWN' END AS \[Netscaler\], --The +3600 on the DATEADD is to allow for Timezone change, it might not be necessary for your appliance DATEADD(s, EPTime/1000+3600, '19700101') as \[Last Polled Time\], SVCFULLNAME as \[GSLB Name\], case svcstate WHEN '4' THEN 'Out of Service' WHEN '1' THEN 'Down' WHEN '7' THEN 'Up' WHEN '5' THEN 'Going Out of Service' ELSE 'UNKNOWN' END AS Health, SVCIP as \[Internal IP\], SVCPORT as \[Port\] from MESERVICES ) RAWDATA ) SEQUENCED WHERE SEQUENCED.RecentFirst = 1 AND ( CASE --Within this CASE I tell my SQL query to ignore GSLB services that below to the remote site and then --vice versa (trust me it works) WHEN \[Netscaler\] = 'REMOTE\_FRIENDLY\_NAME' AND \[GSLB Name\] NOT LIKE 'GSLB\_SERVICE\_NAME\_LOCAL\_SITE' THEN 1 WHEN \[Netscaler\] = 'LOCAL\_FRIENDLY\_NAME' AND \[GSLB Name\] NOT LIKE 'GSLB\_SERVICE\_NAME\_REMOTE\_SITE' THEN 1 ELSE 0 END ) = 1 AND --This AND statement is used to only show the site you are interested in, I use a '%name%' query to specify --but that depends on your naming convention \[GSLB Name\] like 'DEFINE\_WHICH\_SITE YOU CAN TO CHECK AGAINST' order by \[GSLB Name\], \[Netscaler\] **\[Query End\]**

I admit this query might not make sense when you look at it here, but if you want to use it then drop me an email and I would be happy to help sort out my ramblings into something sensible.

**3\. Service/Service Group Status** - This last query allows me to check service group members status, as well as showing me the relevant server IP and port details. It comes in handy when we get complaints about something not working.

**\[Query Start\]** _\--Netscaler Command Center --Service Group member status --Written by IT Geek 20/10/2011 SELECT \* FROM ( SELECT \*, ROW\_NUMBER() OVER (PARTITION BY \[Netscaler\], \[Service Group Name\], \[Server IP\] ORDER BY \[Last Polled Time\] DESC) AS RecentFirst FROM ( Select DISTINCT --Within this CASE statement I change the native IP address of the Netscaler Appliance to a more "Friendly Name" --because they are a HA pair they are displayed as 2 IP's for each site case NSIP WHEN 'NETSCALER\_IP' THEN 'FRIENDLY\_NAME' WHEN 'NETSCALER\_IP' THEN 'FRIENDLY\_NAME' WHEN 'NETSCALER\_IP' THEN 'FRIENDLY\_NAME' WHEN 'NETSCALER\_IP' THEN 'FRIENDLY\_NAME' ELSE 'UNKNOWN' END AS \[Netscaler\], --The +3600 on the DATEADD is to allow for Timezone change, it might not be necessary for your appliance DATEADD(s, EPTime/1000+3600, '19700101') as \[Last Polled Time\], SVCGRPFULLNAME as \[Service Group Name\], SVCGRPMMBRIP as \[Server IP\], SVCGRPMMBRPORT as \[Server Port\], case SVCGRPMMBRSTATE WHEN '4' THEN 'Out of Service' WHEN '1' THEN 'Down' WHEN '7' THEN 'Up' WHEN '5' THEN 'Going Out of Service' ELSE 'UNKNOWN' END AS Health from MESVCGROUP ) RAWDATA ) SEQUENCED WHERE SEQUENCED.RecentFirst = 1 AND \[Netscaler\] not like 'UNKNOWN' order by \[Service Group Name\], \[Netscaler\], \[Last Polled Time\] desc_ **\[Query End\]**

You will have to excuse me if my commenting isn't up to standard but as I'm not use to doing it I wasn't sure what to include. Any questions please let me know and I will be happy to help.

Hope this can be of help to you (only if you have Netscalers).

The Geek
