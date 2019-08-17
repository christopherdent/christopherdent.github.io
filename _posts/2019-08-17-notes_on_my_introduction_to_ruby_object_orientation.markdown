---
layout: post
title:      "Notes on My Introduction to Ruby Object Orientation "
date:       2019-08-17 21:30:52 +0000
permalink:  notes_on_my_introduction_to_ruby_object_orientation
---

I'm late for this blog post but it's because I have been busy trying to wrap my head around understanding object oriented Ruby as opposed to plan old procedural Ruby - which I was just starting to feel comfortable with! :) 

The labs throughout the OO section actually weren't too bad, that is, up until the (seriously hard!) School Domain lab.

I've just wrapped up that lab and the Object Orientation Review video and thought this would be a good place to do a post about to help me review what I've learned before I go any further forward.  


1)  What are objects?  

OK it's easy for the curriculum to state that Ruby is a pure langauage and every single thing in Ruby must be an object; it's a bit harder to wrap your head around the actual significance of that!  For example, even if two objects have the same exact value ("cat" and "cat") they are still separate objects, and you can confirm that by looking up their object ID.  Ruby comes with some built in "primitave" objects, including things we've been using every day up to this point like strings, arrays and hashes.


2)  How do objects work?

Objects are created using the "class" keyword and they're constants - they should start with a capital letter.  Just like a method, a class needs to be closed with the "end" keyword.  I liked where in the review video the instructor said to let your imagination go... if you're creating a dog object, imagine little dogs running around inside the computer.  It sounds a little crazy but I found it really does help!  Makes it a lot easier to understand the object and its potential.  Classes work as factories AND as blueprints for objects.  Objects know stuff, and do things.  

3)  How do we add behaviors to them?

Here's where the concept of an object starts to get really useful, adding behaviors to them.  We do that by adding methods to them.  We can define methods within objects and then use them on the object's instances.  Learning that all we can do to objects is add methods to them helped me understand this.  It was also pretty interesting to learn that if we want to set variables using = we need to create a method that includes= and doing this creates a writer / setter method.  At the same time if we want to be able to call .name on something, we need a method for just name... a reader/getter method; separate from the writer method, and just used to read that property.  All of these can be bundled up into the attr_accessor, which I've been using throughout these labs but didn't fully understand until doing this review today.  

4)  What's the initialize method?

This is another thing I've been using in the last few labs but didn't really get until today so I figured it warranted its own section in the post.  Initialize is a lifecycle event - the class will always evoke this method automatically.  It's a special method with a special meaning to Ruby.  If you want your object to be initialized with an argument (like a name for example) you just need to make sure to give that initialize method an argument.  


So if you're reading this, sorry for a pretty boring post!  But I struggled with the School Domain lab so much I really needed to go back and break all this down for my own benefit.  I think I've got a better grasp on this now so I'm excited to get back into the curriculum and click "next lesson"!

