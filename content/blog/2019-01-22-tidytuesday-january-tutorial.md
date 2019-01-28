---
title: "January TidyTuesday Tutorial! Plotting TV Ratings!"
author: "D. Scott"
date: '2019-01-22'
slug: tidytuesday-january-tutorial
tags:
  - tidytuesday
  - TV Shows
  - data wrangling
  - R for Beginners
categories:
  - tidytuesday
  - tutorial
  - data wrangling
---

This is my first time participating in TidyTuesday, so please be kind.

<p align = "center">
<img src = "https://media.giphy.com/media/qUIm5wu6LAAog/giphy.gif"> 
</p>

Actually, I just decided to make a tutorial about it since what I did was pretty simple 
that I think a beginner could do it. I'm still in vacation mode so the cogs in my brain aren't turning as quickly.  Despite that, it took me a couple of hours to get
things done because I had to think what I wanted to do with the data. And it's probably
going to take me even more time to write out this post because I'm experimenting with using some new things myself(such as putting in GIFs and using gganimate) and I was at a conference for the past few days.

### Context
I do love watching a TV show or 100, so this TidyTuesday gives me the opportunity to do a simple data-driven exploration of some TV shows that are near and dear to my heart when I was a kid. So let's start. The [dataset](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-01-08) that I am using is curated by Sara Stoudt and provided by The Economist.

### Purpose 
The purpose of this tutorial is to make a simple animated plot of television shows. I/you will make a plot of:

1. Average ratings of Hercules: The Legendary Journeys and Xena: Warrior Princess by season.
2. Average ratings of Babylon 5 and Star Trek: Deep Space Nine by year.
3. Share of Babylon 5 and Star Trek Deep Space nine by year.
4. Share of Hercules: The Legendary Journeys and Xena: Warrior Princess by season.
<br></br>

### Tutorial Difficulty
Beginner? If you have some experience with installing and loading packages, you will be fine. If you have some basic understanding of how to use ggplot, even better. And if you know some basics of dplyr well you are awesome. Well who are we fooling, you're awesome regardless! :thumbsup:

### Packages Used
1. dplyr - Used for data manipulation, organization, and cleaning.
2. ggplot2 - Used to plot your data!
3. gganimate - Used to animate your data!
4. gifski - You need to have this package installed for your animations to work.
5. lubridate - Used to manipulate times and dates.
<br></br>

### Things Worth Noting
For this tutorial, I will not explain things line by line because this tutorial is more focused on learning by doing. Make sure to be mindful of the directory paths your stuff is saved at. I would just set your working directory to wherever all your stuff is located at to make it easier. For me, I just created a project and set my working directory to where my project is at. Now that I got that out of the way....let's start!


### Hercules vs Xena: Which TV show had higher ratings?
When I was a kid, one of the highlights of my weekend was Saturday afternoon because of 
[Hercules: The Legendary Journeys](https://en.wikipedia.org/wiki/Hercules:_The_Legendary_Journeys) and [Xena: Warrior Princess](https://en.wikipedia.org/wiki/Xena:_Warrior_Princess). It was two hours of action-packed Saturday afternoon fun since these shows aired back to back in my town. If you didn't know, Hercules aired first in 1995 and Xena was a spinoff of Hercules. Xena first aired in 1996. Each of these series were six seasons long. This makes it convenient to see which show had higher ratings each season. Let's make a simple plot with ggplot2 and animate it with gganimate. I have to give a shoutout to [Sean Meling Murray](https://smu095.github.io/2019/01/22/2019-01-22-first-foray-into-tidytuesday/) for posting his code so I can better figure out how to use gganimate.

Before we start doing anything, we need to load the libraries we need into R. We also need to load the data as well. I saved the data to a variable called TVShows. If you don't have the below packages installed, then you need to run ```install.packages("packagename")```. After that, run the glimpse function to take a look at your dataset.


```r
library(dplyr)
library(ggplot2)
library(gganimate)
library(gifski)
library(lubridate)

TVshows <- read.csv("IMDb_Economist_tv_ratings.csv")

glimpse(TVshows)
```

First, I need to get the name of the shows I want to look at.
```r
HercXena <- TVshows %>%
  filter(title == "Hercules: The Legendary Journeys" | title == "Xena: Warrior Princess")
```

And now it's time to plot and animate it. I tweaked the plot to put the legend on the bottom and I removed the title of the legend. I didn't like the default x and y axis labels, so I tweaked that as well. I also made sure that the legend elements were arranged vertically. I also centered the title of the plot. I will keep these settings for the rest of my plots in this tutorial.

```r
p<-ggplot(HercXena, aes(x = seasonNumber, y = av_rating, color = title)) +
  geom_line() +
  geom_point() +
  labs(title = "Ratings per season for Hercules and Xena", x = "Season", 
       y = "Average Rating") +
  theme(plot.title = element_text(hjust = 0.5), 
        legend.title = element_blank(), 
        legend.position = "bottom",
        legend.direction = "vertical") +
  transition_reveal(seasonNumber)
```
<p align = "center">
<img src = "img/HercXena_Season.gif">
</p>

So it's quite clear that Xena had higher ratings overall per season.

<p align = "center">
<img src = "https://media.giphy.com/media/MC2CX6rHTU2Zy/giphy.gif">
<p></p>

I'm not too surprised with these ratings. No offense to Hercules, but I did like Xena better overall. That being said, I felt that Hercules did get it together for the final season and felt that the final episode of Hercules had more closure than Xena. The final episode of Xena just went off the rails in my opinion.

### Round 2: Babylon 5 vs Star Trek: Deep Space Nine

Let's explore two other shows that I rather liked during my childhood, which is [Star Trek:
Deep Space Nine(DS9)](https://en.wikipedia.org/wiki/Star_Trek:_Deep_Space_Nine) and [Babylon 5](https://en.wikipedia.org/wiki/Babylon_5). Now there was a bit of [controversy](https://en.wikipedia.org/wiki/Babylon_5#Star_Trek:_Deep_Space_Nine_and_Paramount_plagiarism_controversy) with DS9 because J. Michael Straczynski, the creator of Babylon 5, tried to sell the show to Paramount and they didn't bite. But lo and behold, in 1993, DS9 came out. Babylon 5 came out a year later. Being the young pup that I was, I thought that Babylon 5 was a rip off of DS9, but a long time later, I learned that's not the case. Also being a young pup, a lot of Babylon 5 went over my head but everything makes sense now and I have a deeper appreciation for this show.

Babylon 5 was five seasons long and DS9 was seven seasons long. Let's see how the average ratings compare per year. Now how are we going to do that? With the lubridate package!

Let's single out these shows. DS9 and Babylon 5 were both on air from 1994 to 1998. Let's create a new column that stores the years of the seasons. This is done by using the filter and mutate function in dplyr and the year function in lubridate. After that, let's just keep the seasons that aired between 1994 and 1998. 

```r
ST_B5 <- TVshows %>% 
  filter(title == "Star Trek: Deep Space Nine" |  title == "Babylon 5") %>%
  mutate(year = year(date)) %>%
  filter(year >= 1994 & year <= 1998) 

```

Alright, ready to do some plotting and animating!

```r
q <- ggplot(ST_B5, aes(x = year, y = av_rating, color = title))+
  geom_line()+
  geom_point()+
  labs(title = "Ratings per year for Star Trek: DS9 and Babylon 5", x = "year", 
       y = "Average Rating") +
  theme(plot.title = element_text(hjust = 0.5), 
        legend.title = element_blank(), 
        legend.position = "bottom",
        legend.direction = "vertical") +
  transition_reveal(year)
```

<p align = "center">
<img src = "img/B5_DS9_Year.gif">
</p>

![](https://media.giphy.com/media/AE6PiglUBmXB2BbKa1/giphy.gif)

Now this is interesting. Based on my data wrangling, Babylon 5 has higher ratings than
DS9. The younger version of myself felt that DS9 had more fans than Babylon 5. But maybe we should explore this more. How many people were actually tuning into these shows during their regular air time? We can plot the [share](https://www.thebalancecareers.com/how-to-understand-nielsen-tv-ratings-2315476) variable to figure this out.

```r
r <- ggplot(ST_B5, aes(x = year, y = share, color = title))+
  geom_line()+
  geom_point()+
  labs(title = "Share per year for Star Trek: DS9 and Babylon 5", x = "Year", 
       y = "Share") +
  theme(plot.title = element_text(hjust = 0.5), 
        legend.title = element_blank(), 
        legend.position = "bottom",
        legend.direction = "vertical") +
        transition_reveal(year)
```

<p align = "center">
<img src = "img/B5_DS9_Share.gif">
</p>

Even though while DS9 had lower ratings, it had a larger share of people watching the show when it actually aired. Since I started down this route, let's go ahead and go back to our first example and compare the share between Xena and Hercules...

```r
s<- ggplot(HercXena, aes(x = seasonNumber, y = share, color = title))+
  geom_line()+
  geom_point()+
  labs(title = "Share per season for Hercules and Xena", x = "Season Number", y = "Share")+
   theme(plot.title = element_text(hjust = 0.5), 
        legend.title = element_blank(), 
        legend.position = "bottom",
        legend.direction = "vertical") +
        transition_reveal(seasonNumber)
        
```

<p align = "center">
<img src = "img/HercXena_Share.gif">
</p>

Well, it seems that there were a higher number of people who watched Xena at its 
regularly aired timeslot than Hercules with the exception of season 2. It's important
to note that I'm looking at the share per season and not per year.

I will totally admit that I didn't know much about how ratings and shares were calculated
until now, so my bad if I messed something up. Also, my bad if there's a mistake in my code. It worked when I ran it, but please let me know if there's something up! I plan on dropping a TidyTuesday tutorial once every month!