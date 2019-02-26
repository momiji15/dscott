---
title: 'Network Analyst Troubleshooting Checklist'
author: D. Scott
date: '2019-02-26'
slug: lessons-learned-dealing-with-network-analyst
categories:
  - GIS
tags:
  - GIS
  - network analyst
  - troubleshooting
---

This past week, I was having a moment dealing with Network Analyst. I need to use this tool for my dissertation research, and for some odd reason, I thought it was going to be a piece of cake getting this part done. Oh how I was wrong. I felt like Rocky in Rocky III when he underestimated Clubber Lang and got knocked out. 

<p align= "center">
<img src = "https://media.giphy.com/media/7Oy9GJGNMIzcc/giphy.gif" width = "500 px">
<p></p>

I used the [OD Cost Matrix](http://desktop.arcgis.com/en/arcmap/latest/extensions/network-analyst/od-cost-matrix.htm)  in order to find the least cost paths from my origins(population centroids derived from census tracts) to destinations(OBGYN providers) and vice-versa.  When I first ran the analysis, it could only find 50% of the locations. However, I buckled down, kept my eye on the Tiger, and as of yesterday, all of the locations have been found.

<p align = "center">
<img src= "https://media.giphy.com/media/W9G8OK82R3dfO/giphy.gif" width = "500 px">
<p></p>

Here's the process that I went through in order to fix this network. If you are having problems with the OD Cost Matrix not locating points, ask yourself these questions. And while you're doing this, I recommend binging a show or set of movies that you like because the process is painful. In my case, it was Rocky I-V. Which is why you see all the Rocky GIFS in this post.

### 1. Did you isolate the points in question?
If you didn't,  I highly recommend it. Go ahead and export those problem points into a separate shapefile. What I did to create this "ProblemPoints" shapefile was copy the error message after the OD Cost Matrix Analysis, put it in an Excel sheet, changed it from <b>Text to Columns</b>(you can find this under the Data tab) in which I isolated the names of the problem points and replaced the quotes from double to single(not sure if I needed to do this but oh well). Then, I added commas using the formula that you see on the Excel sheet. I did this so I can easily select them because seriously, do you want to write each location name out when you make the select by attributes query? I don't think so. 

<p align = "center">
<img src = "img/excel.JPG" width = "500 px">
</p>

After you are finished messing around in Excel, then go back to ArcMap, open your attribute table, and do a <b> Select by Attributes</b> query. In the query box, something to the effect of "Name" IN ('Location 1', 'Location 2', 'Location 3').


<p align = "center">
<img src = "img/attributeselection.JPG" width = "500 px">
</p>


Of course modify this based on your own data. After you run the query, go into your Table of Contents, Right-click on the shapefile you did the query on and Export the data.

### 2. Did you run a service area analysis to see where the network breaks?
After you export your problem points, go ahead and run a Service Area analysis to see where your lines break. Click on the <b>Network Analyst</b> button and click on <b>New Service Area</b> in the drop down menu. If you haven't done it already, Click on the <b>Network Analyst window</b> button on the right of the <b>Network Analyst</b> button(it's an icon with a table and little flag).

Now you need to change some settings. If you have your Network Analyst Window open, Make sure your Service Area analysis is the one on display and click on the settings button on the right of the Network Analyst Window. Now it's time to change some things. Click on the <b>Line Generation</b> tab and click on <b>Generate Lines </b>. 

<p align = "center">
<img src = "img/generatelines.JPG" width = "500 px">
</p>


Click on the <b>Polygon Generation</b> tab and uncheck <b>Generate Polygons</b>.
<p align = "center">
<img src = "img/generatepolygons.JPG" width = "500 px">
</p>

Click on the <b>Network Locations</b> tab and under <b>Finding Network Locations</b>, set that Search Tolerance up to an obscene distance.

<p align = "center">
<img src = "img/networklocations.JPG" width = "500 px">
</p>

Once you make those changes, press OK and press the <b>Solve</b> button on the Network Analyst toolbar to solve your Service Area analysis.

### 3. Did you create and check the topology of your shapefile?
Brace yourself, this is going to be a long process. After you run the service area analysis, notice where the lines break. Like really notice where they break. Zoom on in. If you don't see roads connecting where they should, you're going to have to do something about it. What I did was to make a topology rule and check my shapefile against that rule. For me, the lines were indeed not connecting where they should. What I did to do this was to create a <b>File Geodatadase</b> and then a <b>Feature Dataset</b> within my geodatabase. I named it "TopologyCheck" to keep the name simple. Import your shapefile within the Feature Dataset. Now it's time to set some rules to get your shapefile in order. Right-click on your feature dataset, click on <b>New</b> and <b>Topology</b>. Keep on going through the prompts and then make sure that you click on the <b>Add Rule</b> button on the <b>Specify rules for the topology</b> prompt. The rule I chose was <b>Must Not Have Dangles</b>. Press Next, check the last window to see if everything is right, and click Finish.

<p align = "center">
<img src = "img/topology.JPG" width = "500 px">
</p>

Now you need to see how much of a hot mess you are dealing with. Click on your topology, go to the <b>Errors</b> tab and click on the <b>Generate Summary</b> button to see how many errors you have. Take a breath, because it might be bad. But not as bad as you think. But still bad.

After you do that, make sure you add your topology to the Table of Contents. If you get a prompt asking to add all participating feature classes that participate in the topology to the map, press yes. Now you need to start editing your roads feature class. Go to <b>Editor</b>, <b>Start Editing</b> and choose our road shapefile/feature class that you wish to clean up. 

If you look at your Table of Contents, you will see the symbology for the various types of errors that occur based on your topology rules:

<p align = "center">
<img src = "img/errorinspector2.JPG" width = "300 px">
</p>

Click on the <b>Error Inspector</b> button(little table with an x on it), click on <b>Search Now</b> and see what errors are present in your data. To make it more manageable, I would zoom to one error and then click on <b>Visible Extent Only</b> to see the errors within the extent that you currently see.

<p align = "center">
<img src = "img/errorinspector1.JPG" width = "500 px">
</p>

Now it's time to fix those errors. You are going to need either snap the roads to connect to the other roads or you need to mark the error as an exception. You can do this by right-clicking on the feature in question in the Error Inspector. A lot of my errors were exceptions since the dangle was warranted (i.e. the end of the road). Like I said before, make sure you have something playing in the background because this is a tedious process. It took me a couple of days to fix my errors. I definitely was feeling like I was going under a Rocky IV training montage because it was mentally(and physically because I got headache while doing it) taxing.

<p align = "center">
<img src = "https://media.giphy.com/media/26tPkINmJHvdfanGU/giphy.gif">
</p>

Hearts on fire and all of that. Actually, I finished it all by the time I got to Rocky V(which is the worst movie out of the franchise by the way).

### 4. Did you update your shapefile with the new changes?
You're going to have to re-calculate some geometries and add any additional data to any newly created lines you made during the topology check. For me, I needed to re-calculate the geometry and add speed limits to my newly created lines.

### 5. Did you make a new network with your updated shapefile?
You absolutely have to do this if you want to see if your fixes did the trick. In order to bring some order to the chaos that can happen, I would recommend either creating a new workspace or putting your data in a new data frame. I exported the roads feature class I had in my TopologyCheck geodatabase and created a new network to just keep everything organized.  

### 6. Did you re-calculate the location fields?
Once you load your locations in the OD Cost Matrix, you are going to have to recalculate the location fields since you are using an updated road network. You just need to right-click on your Origins and Destinations and click on <b>Recalculate Location Fields</b>.

### 7. Did you re-run your OD Cost Matrix?
Go ahead, do it. If you still have errors, then you're going to have to do 2-7 again on the checklist.

Keep at it, and hopefully everything will be fixed(or a lot better than before). It took me at least three times to do so. I did tried to cut corners by only fixing the roads near the points which could not be found, but I ended up having to fix all of the errors in my roads shapefile. Sometimes you just have to go the distance to get everything to work! It sucks and is time-intensive, but you have to do what you have to do. It was definitely a learning experience for me so I hope you find this checklist useful when trying to troubleshoot with Network Analyst!

<p align = "center">
<img src = "https://media.giphy.com/media/11IRfBXYw64gZW/giphy.gif">
</p>
