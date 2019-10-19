---
layout: post
title:      "CLI Project Development"
date:       2019-10-19 16:46:35 -0400
permalink:  cli_project_development
---

Well, I think I am finally ready to submit my project.  I've had a great time developing this little app and come a very long way with my coding knowledge while doing so.

Even as I was finishing up the OO Ruby section of the curriculum I was still pretty terrified at the idea of writing a program from scratch - all this still seemed very new to me and I wasn't sure I was ready to do something without any templates or tests .  I really thought this might be where my coding course ends!  But through persistence and leaning heavily on the resources Flatiron has introduced me to so far, I've got my program up and running and I learned more than I could have imagined.  

Those concepts that still felt new to me at the beginning, like scraping and object associations?  I feel like I know them well now.  A project like this forces you to review, review, review and eventually it all starts to connect.

One of the most interesting parts of working on this project has been to get a better understanding of abstraction.  After completing this first part of the course, I understood what abstraction was and how it could increase efficiency but I still thought of it as a "nice to have" part of code rather than a fundamental necessity.  With that in mind, my first version had a lot of information hard coded.  In fact, I had the program up and running over a week ago and it provided basically the same user experience it does now and I thought I was just about done.  After meeting with our friendly section coach Beth and also re-reading the instructions (keep it DRY; save your scraped data, etc.) I realized I needed to do some refactoring.  Some of these requirements just couldn't have been met without significant abstraction.  It took me a while - days really - to convince myself that deleting huge swaths of my working code and replacing them with something else was the way to go but eventually I just went for it – deleting three methods at a time to replace them with one better method; removing hardcoded data and replacing with passed arguments, and, most importantly, making heavy use of OBJECTS and their ATTRIBUTES instead of simple strings.  Now that I've done all that, even though my user experience is much the same as it was a week ago, the program itself is much more efficient and more powerful, with plenty of room for further development using the "period" and "creature" objects, each of which have their own attributes that are defined as the program is launched.  

It was a lot of work!  But the feeling of accomplishment I got every time I got a feature to work as I wanted it to was awesome.  

**About the Program:**

**Concept:**

In one of the earlier lessons there was a video demonstrating scraping using animals’ genus and species in Wikipedia as examples.  Being a nature/science lover, that one example really stuck with me and when it came time to develop an idea I started thinking about that.  And what’s more fun than living animals?  Dinosaurs of course.  Early on I knew I wanted to:

1)	Have a program that could dive into a specific geological time period.
2)	Give some examples of life from that period
3)	Go a level deeper and give us a fun fact about that life

First Attempt:
I browsed the web and decided to break it up by periods of the Mesozoic Era – Triassic, Jurassic and Cretaceous.  My first instinct was to use Wikipedia but I was concerned that there could be user changes to the pages I was scraping so I looked for an alternate.  I did end up using a Wiki, but a much less used, older, dinosaur specific wiki.  I looked at the edit history for most of the pages I’d be scraping and couldn’t find anything that had been edited even within the last few years… probably safe to use!

As I said I had the program up and running pretty quickly.  Early version worked like this:

1. Program initializes and presents you with the three time periods.

2. Selecting a time period triggered a period-specific scraper, which would scrape the “trending dinosaurs” section of that era’s page and create and output array with them.  

3.  Array would display as a list, user could pick their animal and get a “fun fact”, which was scraped from the animal’s      page.  The animal name from the array was interpolated into a URL that led to the animal’s page.

4. Fun fact would display and program closed.  


**Room for Improvement:**

That’s pretty much where things were when I met with Beth to discuss the project.  I got some great feedback out of that meeting which put me back to work for another week or so:

1)	Keep my classes straight.  Scrapers are for scraping, not puts-ing anything.  Creature class needs to know facts about each creature instance.  And the period class, which wasn’t doing much at the time, should contain all the facts about the period instance.  

2)	CLI is like a waiter – it asks the user what they want and brings it to them.  I had a lot of the action happening in the CLI class which was more like having the waiter ask them what they want and then bring them all of the ingredients to the table.  Breaking this out into the more appropriate classes helped me make the code look better and much more abstract.    

3)	DRY – use abstraction wherever I can.  For example, the period specific scrapers were almost identical except for the period name, surely there was a better way to do that?

4)	Repeat experience – program was running great if someone wants one fact about one animal from one period, but what if the user really wants to learn about these periods?  I needed a way to have the program repeat itself . That sounded easy at first until I realized doing so was expanding my array of creature every time the user asked to learn more.  Which ties into the last thing:

5)	Scrape the data once and then use the saved data.  Doing this wasn’t as easy as I expected it to be but once I got it done it solved a lot of the above problems.  

**The Program Now:**

I made these changes and I’m pretty happy with where things are now: 

1)	Program initializes and runs the creature scraper, saving each creature as an object and adding each creature as an attribute to the period object.    

2)	Program presents you with some awesome ASCII dinosaur art and three time periods, each of which are instances of the period class.

3)	Selecting a time period triggers a simple “list_creatures(period)” class method, which uses the saved array to list the creatures of that period (this replaced three methods, “list_triassic_creatures” etc.)

4)	A “get_creatures(period”) class method is triggered.  Based on the input, this method makes the dinosaur object roar and triggers a fun fact scraper.  The fun fact scraper still uses interpolation in the scraper’s URL to access the correct wiki page.  

5)	A “learn_more” method is then triggered, allowing the user to go back to the period selection and start fresh.  Alternatively the user can say “no” to terminate the program.  

So that’s where I am now and I think I'm in a good place to submit this.  I’ll keep the program around and work on it for fun when I have time as I really get a lot out of making little changes and improvements to it.  


Thanks for taking the time to read about my little journey developing this program!  




