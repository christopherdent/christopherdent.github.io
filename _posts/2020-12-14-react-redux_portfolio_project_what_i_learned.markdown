---
layout: post
title:      "React-Redux Portfolio Project: What I Learned"
date:       2020-12-14 22:22:31 -0500
permalink:  react-redux_portfolio_project_what_i_learned
---


I entered the React/Redux section after a taking real bruising in the JavaScript section, but was happy to see that React was bringing me back to a more declarative type of programming, which focuses on what the program should accomplish without specfying how the program should do that, whereas with Javascript you needed to tell the thing every little detail on how you want something done (imperative programming).  Yay, back to software that is smarter than I am! Shouldn’t be too bad!

And it wasn’t – until Redux came on in to muddy the waters for me! It wasn’t easy and I had to do a ton of research outside of the curriculum alone, but after reading blogs, docs, watching YouTube tutorials, and, importantly, watching the recorded project builds in Learn instruct, I started to get the hang of it and began to understand the differences between component state and store state, and when to use which (I kept high level data that I might want accessed at any point in the app in the store, but data that would only be used once like an edit form kept its own component state).  

So on to my app.  I was also entering this project with the clock very much ticking on how much time I have to complete the program.  I have faced a number of challenges during 2020 (aside from COVID)  that have taken precious time away from coding.  So I wanted to do something simple – and when I started watching the ‘expense tracker’ project build I realized that simple is possible for this final project.  For a while I had wanted to build out on the concept I started in my very first project, the Ruby CLI project, Prehistoric Life: Creatures of the Mesozoic.  Could I do a graphical interpretation of this my gem by using React?  A spiritual sequel?  Well I set off to find out and Dinofinder 2020 was born.

I have branded Dinofinder 2020 as “the prehistoric card collection that anyone can edit.” Nothing at all influenced that tagline of course.  Just kidding – the app is actually intended to be both a fun little library of virtual dinosaur ‘cards’ but also a research tool that would encourage kids to learn the very basics of internet research, using web-based resources (some linked to in the app) to create their very own dino card collections.  The app begins with a presentation of three world maps – the Triassic World, the Jurassic World, and the Cretaceous World (just like my CLI app structured)
  
	![](https://dinofinder2020.s3.amazonaws.com/dinofinder+screen.png)

Clicking on one of the worlds will present you with different categories of creatures for each world, herbivores, carnivores, avians and marine life.  I’ll stop here to highlight one of the coolest parts of React, which was the React Router.  I loved using the Router to create my own custom routes to navigate through the app, all while being on the same single page.  I may be wrong but it seems to me like a lot of the hassles of connecting with the backend are removed by this – the backend routes really don’t matter at all except for your fetche requests, it was a concept I hadn’t encountered before and I just felt really good about the flexibility it provided.  

Anyway, on to the challenges:

Following the live builds, particularly the expense tracker build, I got the basic structure of my app up and running.  One big takeaway from the live build videos was the importance of getting your backend right the first time so you don’t have to worry much about it going forward, so I took a good bit of time to set up my backend.  

Within a couple of days I had my eras container, dino-type container (category), and dinosaur container all set up, as well as an action to ‘fetch’ whatever was in the database (seed data at that point, one creature for each category of each era).  It was then that I started edit and delete functions and that is where I realized I still hadn’t fully grasped the concept of state – both features were doing all sorts of strange things when triggered, sometimes displaying a card that simply said JSON, sometimes duplicating a card, sometimes doing nothing at all – all the while making every change I wanted it to make to the backend.  After a few more marathon React/Redux instructional video sessions (thanks YouTube) I got the idea of what was going on – my fetch requests were going fine but it was the next step, the reducer, that was giving me trouble.  You see, the reducer is what spits out the modified state based on the changes made in your actions, and you’ve got to return that state correctly.  

What I learned was if you’re mutating your state, you’re gonna have problems.  The curriculum did state this more than once but I didn’t really grasp the concept until I faced it in my own app.  

I could not for the life of me figure out why my edit and delete functions were both doing what needed to be done on the backend, but giving me strange, unexpected results in the page render.  This took me at least a week of debugging to figure out, but I am glad it did because I’m not sure I fully grasped Redux until I went through that.  What was happening was I was mutating my state.  

I didn’t know if it was an issue with my actions, form/button, my container, heck even my back-end (which WAS the problem for a much more easily solved issue with the “create” function).  Early on I realized that a quick reload (and hence re-fetch) would solve the problem – may be a lifecycle method could be the answer here?  One that will happen once the component updates, like componentDidUpdate?  I threw that in there and repeated my fetch inside this method and viola, problem solved.  

Not.  

![](https://womenwriteaboutcomics.com/wp-content/uploads/2015/11/Mutant-Menace-300x286.jpg)


While visually the desired effect was achieved with this approach, it resulted in an infinite loop of fetching which just wasn't right, something my poor laptop made clear to me when its fan started roaring like a jet engine.  I was happy to see the desired behavior, but I knew deep down that this wasn’t the right way to fix it and that I couldn’t submit my project like that.  
 
So, after watching another dozen or so hours of project builds, I started messing around with the React and then the Redux dev tools.  Redux tools are what helped me pinpoint the problem (insert redux screenshot here).



![](https://dinofinder2020.s3.amazonaws.com/redux+screen.png)
			What do we have here?  Objects nested differently – mutants! 

Looking at that diff I was immediately able to narrow down the problem to the reducer, and I could also see what was going wrong – the object that the state used to be was being changed – or MUTATED – into something new altogether, hence unexpected results (with edit, a duplicate card render; with delete, just nothing happening until reload).

So we went from this (do you still have the bad code from the reducers, maybe in the backup??)

```
case 'DELETE_DINOSAUR':
	const dinosaurs = state.dinosaurs.filter(dinosaur => dinosaur.id !== action.id); ///keep all the dinos except the one who's id matches action id
	return {...state, dinosaurs}

case 'EDIT_DINOSAUR':

	return {...state, dinosaurs: [...state.dinosaurs, action.payload]}
```


to this:


```
case 'DELETE_DINOSAUR':
			 return {
		...state, dinosaurs: [...state.dinosaurs.filter(dinosaur => dinosaur.id === action.dinosaurId ? false : true)]   ///returning a copy of the state as well as an array of all dinos other than the one that was just deleted.
	}

case 'EDIT_DINOSAUR':
let dinos = state.dinosaurs.map(dinosaur => {
	if (dinosaur.id === action.payload.id) {
		return action.payload  ///return the modified dinosaur card
	} else {
		return dinosaur
	}
})
return {...state, dinosaurs: dinos}  //return the state with the modified dinosaur
```


And the mutant menace was defeated.  It was hard to figure this one out, but as soon as I saw the diff in the above screenshot it because pretty clear what was wrong and from there it was just some trial and error to figure out.  

The final leg of this project for me was CSS.  I’d actually started pretty early on with vanilla bootstrap incorporated into this project and was moderately happy with the results, but they needed tweaking.  I re-read the project requirements and noticed they suggested using REACT bootstrap, not just plain old bootstrap.  Why?  That was the question I asked myself.  I like my vanilla bootstrap.  My previous projects all used it and I was getting comfortable with it.  So I did some research and realized that again, just because I was getting a desired effect on screen did not mean that I was doing it right.  I learned that one big benefit of using react bootstrap was animations – something that I wish I’d known earlier because the react-bootstrap accordion provided t he same effect I scrapped together with my “showHide” method (but damned if I was taking showHide() out of my project after the work I put into it!)  Aside from animation though, I learned that the syntax of the virtual DOM may not always be compatbile with the actual DOM so using a CSS library tailored to a traditional DOM might cause problems down the road, problems that I didn’t need.  I also realized that I’d already encountered some of these problems, with bootstrap styles not displaying as I’d expected.  So, off to learn react-bootstrap I went.  

From the react bootstrap docs:

`Methods and events using jQuery is done imperatively by directly manipulating the DOM. In contrast, React uses updates to the state to update the virtual DOM. In this way, React-Bootstrap provides a more reliable solution by incorporating Bootstrap functionality into React's virtual DOM. Below are a few examples of how React-Bootstrap components differ from Bootstrap.`

So I toyed with React-Bootstrap and was happy with how easy it was to work with, easier than vanilla Bootstrap really.  Everything is packaged up into neat little React components.  Aside from Accordion, and Card, I haven’t used too many of these components, but they leave so much room for improvement and future tinkering.  


