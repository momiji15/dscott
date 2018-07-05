---
title: 'Perspectives on the rtweet, twitteR, and streamR packages'
author: D. Scott
date: '2018-07-04'
slug: twitter-vs-rtweet
categories: 
  - data collection
tags:
  - rtweet
  - twitteR
  - streamR
  - twitter
---


From time to time I get e-mails about an [article](https://opensource.com/article/17/6/collecting-and-mapping-twitter-data-using-r) I wrote around a year ago about collecting tweets using the twitteR package and I recently got one this week which must have been the R deities' way to tell me to stop stalling and get to it in writing a post about my experiences using the rtweet, twitteR, and streamR packages. ;) I'm going to do this from a different perspective and talk about it from not only learning it myself but teaching it as well.

I have used three R packages to collect tweets. The first one that I used was the [twitteR](https://www.rdocumentation.org/packages/twitteR/versions/1.1.9) package when I wanted to just do a one-time collection of tweets and get acquainted with the process of collecting tweets. Then, I got introduced to the [streamR](https://github.com/pablobarbera/streamR) package in which you can collect tweets in real time. Just like Goldilocks found the bowl of porridge that was just right for her, I found that streamR was just right for my needs which was to continuously collect tweets that mention the VA (Veteran's Affairs) health system or VA hospitals. It was quite scary to set that up being a beginner R user and I almost ran away into the forest just like Goldilocks...but I persevered and was able to continously collect tweets for two years. Shortly after I finished up my data collection, a new R package called [rtweet](http://rtweet.info/) appeared and of course I had to try it. I was able to quickly do some cool stuff with it other than just collecting tweets such as being able to make charts and maps. I thought it was so cool that I decided to change up my teaching project for a certificate I am doing and incorporated rtweet in my project. I was initially going to just go with twitteR and streamR because I taught it two years ago when I was a TA for the Programming for GIS class offered at my school. But I'm glad I made that change because it allowed me to observe learners' reactions in learning each package and I was able to get feedback on my teaching from some kind volunteers who observed the class.

So which package do I like better or I felt the students grasped on to quicker? To give a quick answer, I feel that students were able to use the rtweet package with more ease than the other two. They had more snafus with trying to learn the twitteR and streamR packages. I also had that experience, but it wasn't like it was difficult to use the streamR and twitteR packages. I'll break all this down by package:

##### twitteR
I had a good experience with the twitteR package, however since I wanted to continuously collect tweets in real-time, I ended up using the streamR package. Since I had two lab sessions in which I could teach the course material, I decided on teaching both the twitteR and streamR package. If you have limited time or you are teaching a one-time workshop, I would suggest that you start teaching twitteR first because if your learners know how to use twitteR, they know how to use streamR. Also, if you are looking for historical trends in tweets, this is the package for you. 

One thing I did not like about this package is that you cannot do a boolean search(that I'm aware of...and if so oops my bad). Some of the issues my students had was with the OAuth. I had no idea how to fix that, but being the TA, one is expected to have the answers, so I just tried all of my tricks and like magic, it worked.  

##### streamR
This is the package that I used heavily. I didn't have too many issues with this package, but one issue I had with it was that I wanted to just let this package do its thing continuously without me having to check on it regularly. You can make it where the package collects tweets for a set amount of time, but that is not guaranteed to keep on running due to the facts of computer server life.  if there was an interruption with the server, I wanted the script to just restart and keep on putting. However, you cannot do this within the package itself. I ended up turning my desktop into a data collection device for two years and painstakingly figured out how to run a [CRON job](https://code.tutsplus.com/tutorials/scheduling-tasks-with-cron-jobs--net-8800) in which I could walk away and not worry about the collection stopping because I had the script run every three hours and I kept the time period to collect tweets to be three hours. Maybe there was an easier way, but at that time I was a beginner in R and really had limited knowledge on such things so I just had to go with what I ended up finding with the many Google searches that I did. 


##### rtweet
Ahhh and finally on to rtweet. The main issues I had with twitteR and streamR were not really issues with rtweet. You can use boolean terms! As for continously keeping the connection open, I haven't personally done it too much to give any commentary on it, but it is cool that you can do what you would have to do in two packages in one package. I also like that you can create plots and maps of your tweets within the package. As for my students, they did not have too many problems with the rtweet package. The issues they had more had to do with what was going on with the lab computers, and the fact that they felt that the instructional materials could get more indepth about the functionality of the packages. Here's one example of one of the projects my students did.

[Allergies Spring 2018](http://rpubs.com/xtianpedraza/allergyseasondoc)

##### So Which One is Better?
Well.......for me, I will start using rtweet from now on. There's still some things I want to try out with the package.  However, I wouldn't discount streamR or twitteR. In the end, it is really what you are more comfortable with and which tutorials click for you. As for my students, it seemed that they did have an easier time with rtweet than with twitteR and streamR. But then again, that depends on the previous programming experiences of the students. Some classes have more overall experience with programming than other classes so one must factor that in as well. If you're interested in learning how to use any of these packages, check out my tutorials on my Github!

[From App to Map Part 1: Collecting tweets using the REST API](https://github.com/momiji15/apptomap/blob/master/atmpt1_werise.Rmd)

[From App to Map Part 2: Mapping your tweets on a leaflet map](https://github.com/momiji15/apptomap/blob/master/atmpt2_werise.Rmd)

[R Ready to Map](https://github.com/momiji15/apptomap/tree/master/R%20Ready%20to%20Map)

