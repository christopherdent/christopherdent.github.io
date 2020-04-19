---
layout: post
title:      "My Rails App:  Association Volunteer Manager"
date:       2020-04-19 20:35:21 +0000
permalink:  my_rails_app_association_volunteer_manager
---

Well this has been quite the experience.  I’ve been working on my Rails project so long now that we still had restaurants and movie theatres when I started – seems like eternity!

I’ve taken a long time with this for a variety of reasons.  I finished the Rails curriculum and entered project mode the same week as my company’s second biggest conference of the year, meaning I had to leave my little Florida hideaway and fly to DC for that week.  Even in a normal week that would set me back but that conference ended with the country beginning to shut down so it was not a normal week.  I flew home to find out that my brother was evacuating New York and needed a place to stay (guess where my coding rig is? Yup, guest room!), that my wife was sick, shoot I was sick too, and my elderly mom was going to need help with groceries, etc. for the foreseeable future.  Did I mention my own chronic health issues flared up at this time too?  I won’t linger on this but suffice to say it was a tough month and I’m extremely grateful to Flatiron for extending the self-paced program due to the pandemic.

Now, on to the app!  As I said I started this when I was in DC for work (I am a remote worker for a DC based association).  My company is a scientific association that develops methods (not that kind!) and standards in conjunction with volunteer experts, who pitch in to help the scientific situation in their particular community.  

My boss had mentioned needing a new way to organize our volunteers and groups.  Company wide we use iMIS, which does far more than my department needs, and doesn’t do what we need it to right.  It’s not working for our team.  I thought back to my Sinatra project and realized it wasn’t a huge stretch of the imagination to go from something like that to something like my boss has described, so after careful consideration I pitched the idea to her that I could try to make my next school project to be something like she was describing – I mean I’m doing this program for me, not for my company, but if I could make something that would both fulfill the project requirements AND make my team’s life a little easier, why not try?  

So my app “Association Volunteer Manager”  (AVM) was born.  It’s got four models:

    • User (the users of the app)
    • Volunteer (the people the app will keep track of)
    • Group (the groups the app will keep track of)
    • GroupVolunteer (the join table)

User has many of all the others.  Volunteer and Group are in a many-to-many relationship, and GroupVolunteer belongs to all of the others.  

The point of the app is to allow team members to collaborate so I didn’t want to restrict CRUD functionality only to the user who created the record, it wouldn’t make sense for this app.  But I did need to restrict access for some people, so I made an admin function for the User model.  Admins can CRUD, but non-admins can just R.  Non-admin users would be for example one of the volunteers themselves, or any other non-staff entity who needs to see the data.  

One thing I did a little differently than what we learned in the curriculum was that I put the nested routes and forms directly into the objects’ show pages.  It just made more sense to me to be able to add a volunteer to the group directly from the group’s show page than going to another page to do that. You can add a volunteer to the group right there from the group show page, either an existing volunteer from a dropdown or a brand new one using a form.  Similarly, the volunteer show page lists all of the groups that volunteer is a member of right there on the same page rather than creating a new page like volunteers/1/groups.  

All this led to some interesting routing tactics, specifically member routes.  Member routes are defined for actions that are performed on a member of a resource and look something like this:

`resources :groups do
  resources :volunteers do
  member do
  delete :remove_from_group
	end
     end
end`

One thing I was dreading was the join table.  It was a concept I’ve had trouble grasping anyway, and now I needed one with a user-submittable attribute.  Well, we require our volunteers to submit statements of expertise when they’re joining new groups, and those statements are specific to that group and that volunteer, so there we go, “statement” would be the user submittable attribute for the GroupVolunteer model.  I had a tough time implementing this and didn’t think I’d keep it in after the project was over, but now I must say it’s working nicely and does add a good amount of information to the records.  

Another thing I really wanted to include here was a CV attribute for each volunteer.  I had no idea how to do this as it’s not really an attribute but an attached file (they need to submit C.V.s as well) but it would be really great to have the volunteer’s core data, statements of expertise, groups, AND CV all on the same page so I figured I’d try, and this ended up teaching me about ActiveStorage (https://edgeguides.rubyonrails.org/active_storage_overview.html).  This part actually ended up a lot easier than I thought.  I started by following the example in the guide, which was adding an image rather than a file.  Just as a test I decided I’d add avatars for the users.  It worked!  So from there it was very easy to add the CV (has_one_attached) to my volunteer model.  

Probably the biggest issue I had – and continue to have – is with strong params.  Strong params is constantly telling me that my required parameter is missing.  Even when I promise it’s not.  I’ve Googled this to infinity and back and also brought it up at study groups but I’ve never really gotten a clear understanding of why it’s happening.  I’ve found workarounds in each and every instance where I had a problem though….always more than one way to do something in Rails!

This is getting long so I’ll wrap it up.  AVM is not perfect, I am sure it could use a ton of refactoring, and there are features I’d like to implement before we use this in a production environment.  But the great thing about the software engineering program is that I WILL know how to do it, in time.  



