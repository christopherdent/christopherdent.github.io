---
layout: post
title:      "My JS/Rails Project Battle, Ships!"
date:       2020-10-06 02:25:34 +0000
permalink:  my_js_rails_project_battle_ships
---

## Overview 
This one has been quite a journey.  When I started the JS section, COVID wasn’t really a thing yet.  I entered the section riding on a wave of overconfidence after what I think was a very good Rails app, following my earlier successes with Sinatra and Ruby CLI.  I came out of that third project with a solid understanding of Ruby, Sinatra and Rails and was very happy with my progress.  

Then I got to JS.  The first thing I noticed was the topic-centric study groups which had been so prevalent in those past three sections just didn’t seem to be happening very much.  Further, the 1:1 pre-project reviews that were part of the program when I started no longer happen.  So, going into this project, I felt very much alone, and stuck.  It took me several run throughs of the Javascript section in the curriculum, lots of Googling, trial and error, and support from other students via Slack, and some great suggestions from the instructors during office hours, and I believe my MVP is now ready.  

This project and this section as a whole were one of the biggest challenges I've faced in my software engineering experiment.  Many times I thought I should just give up and focus on the career I've already got.  But I persisted, and now I've got a working app.  As simple as it is, I am pretty proud of it.  

## The Game 

Battle, Ships! I was inspired by a few labs from the section, Pokemon Team Challenge and Toy Tale.  I didn’t really want to do something with a login and forms etc. as I’ve already done that for my last two projects.  I’ve been wanting to develop a game of sorts since I started the program and I thought this could be a good opportunity.  Instead of Pokemon cards or Toy Story cards, what kind of cards do I want?  Well I had recently been brushing up on my WWII history (I was a European History major and still geek about about it from time to time) and thought it would be fun to develop a simple div card game based on some of the more prompnent naval vessels of ths time.  And Battle, Ships! was born.  

## Concept & Definition

I started sketching out ideas on a notepad and eventually developed a board with three main DIVs:  a left hand div which would display the cards for all of the available ships, and a right hand div to add the player fleet, the computer fleet, and a ‘battle’ button.  I had absolutley no idea how I was going to do much of this beyond creating the cards at that time, but I told my self to just take it piece by piece, function by function, heck, line by line if that’s what it takes.  My early idea is sketched out (poorly) below:
 
![bad-diagram](https://drive.google.com/uc?export=view&id=1Jf9twzTYDcy2XLEztgD8mRmOVwbFTe_e)
 

Note that I did forsee a “User” model being involved at that time, but I’ve nixed that from the app for now.  I believe I’m meeting the requirements as it as and due to my time crunch I’m just going to go for it without users for now. and consider that a potential future enhancement.  It wouldn't add very much to a game like this anyway except for a tad of personalization.  


## Implementation

After rewatching the Pokemon Teams video a few times, I was confident that I could do a “fetch” GET request to an API and manipulate that data within the DOM.  

It took me a while but I did it, I was able to get all of the ships I had put in my API to render on screen with a name, a picture (courtesy of Wikipedia), their country affiliation, and the type of ship.  Ship types largely coincide with the board game “Battleship” - carrier, cruiser, battleship, destroyer and submarine.  I also decided at this point that I’d need to assign point values to each class, but that these point values would be randomized over certain ranges so if, for example, two battleships were facing each other, it wouldn’t always be a tie.  Because that was part of the Rails model, I was comfortable enough to write that method early on:

```
  def points
    if self.kind == "Battleship"
      return rand(40..50)
    elsif self.kind == "Super Dreadnaught"
      return rand(55..65)
    elsif self.kind == "Carrier"
      return rand(50..60)
    elsif self.kind == "Destroyer"
      return rand(30..40)
    elsif self.kind == "Submarine"
      return rand(25..45)
    elsif self.kind == "Cruiser"
      return rand(15..30)
    end
	end
```
 

I haven’t included any “super dreadnaughts” yet but may in the future.  

From there I had to figure out how to split the screen in two since it’s a single page application (SPA) but I needed separate places for available ships and the battlefield.  This part came a bit easier than I’d expected thanks to plenty of examples on the internet, I tinkered with a few until I found one that worked (a combination of [this](https://www.w3schools.com/howto/howto_css_split_screen.asp) and some examples from Stack Exchange, etc. 

With that in place I moved to the next step, getting my ships to move to the battlefield as they were selected.  I knew fetch was going to be involved but could not figure out the best way to add the ship to each fleet using fetch.  I eventually decided to do this as a `PATCH` request, not to the fleets controller but to the ship, and have that ship’s fleet_id (which starts the game as “null”) assigned to `1`, because Fleet 1 is always the human fleet.  And it worked!  I could click on the “add to fleet” button on the ship card, trigger the `PATCH` request, which routed to the ship controller and set it’s fleet_id to 1.  

Great success!  

Then I had to take those ships who had been assigned to Fleet 1 (which is limited to five ships), render them on the right side and remove their card from the left.  DOM manipulation was one part of the JaveScript section I felt pretty comfortable with, so althought it took a few days and a lot of Googling, I got that to work and got the cards to shrink a little in the battlefield (the “add to fleet” button is removed in this function as well so the card needed to be shrunk so it didn’t look funny).

Then, I added an alert to state that the max fleet capacity has been reached.  Once that alert popped, I needed to trigger a couple other things:  
* Creation and rendering of a computer fleet (consisting of ships that are not in Fleet 1) 
* Rendering of “battle” button between the fleets.  

 This got a little trickier.  I was already using the “update” action in the ships controller to assign ships to fleet 1.  I needed another way to assign ships to fleet 2.  I was already using my fleet controller update  action for something else too, resetting the fleets to empty on a new game.  I tried many things what worked best for me was to use another method in the fleet controller to do the same thing.  Since the “Create” function was doing nothing, I added a method there that: 

1. Defines fleet variable as Fleet.find(2)  note there are only ever two fleets in this game, both generated with seed data
2. Sets an empty array for fleet.ships
3. Pushes five 5 ships into that array *if they did not already have a fleet ID*.  

Viola, the computer fleet was created.  I was then able to render it onto the screen much like I did the user fleet.  Note:  I know that the “create” action of the fleet controller is NOT the best place for this, but it had been so long since I was using Rails I hadn’t even thought to use a custom route, but I remedied that situation while finishing up the project.

Which leads me to the “battle” function.  This function sums the totals of each of the fleets and then compares them to each other.  Fleet with the highest overall score wins.  I added a “battles” route to my config/routes, added an `eventlistener` to the `battlebutton` which would fire that fetch request and return the result and it worked.  It was at this stage I went back and brushed up on Rails routing and realized it made more sense to create a 'battle' action in the controller than to co-opt one of the CRUD actions into doing something it shouldn't be doing - let's leave those open for future enhancements.

One thing to note here was that with many of my fetch requests before and up to that one, I was getting a syntax error in the browser console about an “unexpected end of JSON data.”  It hadn’t limited functionality up to that point so I mostly ignored it and added a .catch to the end of those, but this time, it was preventing the fetch request from working properly so the time had come to debug it.  The problem was actually simple.  It wasn’t my code, it was the API returning malformed JSON data.  I tweaked my seed file as well as few of my my “render JSON” lines to match some examples from the curriculum and the error message disappeared.  From what i've seen in study groups and around the web this is a pretty common problem, but the solution is almost always the same - make sure your JSON data is not malformed.

By then my minimum viable product (MVP) was in place, I had a single page app (SPA)  that would allow a user to:

1. Select ships to form a fleet
2. Battle the fleet against a randomly generated computer fleet.  

It has a has many relationship (fleet has many ships) and a belongs_to relationship (ship belongs to fleet).  I have numerous fetch requests with at least two of the requirements (I have PATCH and GET requests only, nothing new is created and no objects are deleted in this app – yet).  My app is an HTML, CSS and Javascript frontend with a Rails AP back-end and all interactions between the two are handled asynchronously.  At that point, about 2-3 weeks before this writing, I was back on my high horse thinking I’d finally gotten it and was very close to the end.  All I had to do was refactor to be object oriented.  No problem, right?  WRONG.  

### OO Refactor

This was another big struggle for me which again I think would have been remidied if we had more study groups on the topic.  But here is where I really thank other Flatiron students for providing me with a slew of resources when I asked the question in Slack.  Between those, the curriculum and some outside resources I gained an understanding of OOJS.  I am certain there is much more refactoring I can do in this respect, but the important part is that *I get it* when I really did not see the point of it before the refactor.  It has made my code much cleaner, easier to manage, and easier to expand on in the future.  Since my app doesn't require much creation of instances, I relied heavily on static methods.  There is one place where an instance is created and that's the "game" class - every time a game is started, a new instance of the "game" class is born.  The only other model I have right now is “ShipCard” which handles much of the HTML and CSS around the building and movements of the ship cards.  


## Conclusion 

And that’s about that.  You can view a quick demo here:

[![](http://img.youtube.com/vi/awzoW1nrSZM/0.jpg)](http://www.youtube.com/watch?v=awzoW1nrSZM "")

It’s not going to win any GOTY awards, and heck,  but for a history nerd like me it is fun to play around with the different combinations and see who can win what.  I hope this will be acceptable to the instructors in my assessment and look forward to hearing their feedback and suggestions for potential improvements.  Thanks for taking the time to read my post!

