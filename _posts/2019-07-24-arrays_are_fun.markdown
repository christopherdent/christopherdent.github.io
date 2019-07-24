---
layout: post
title:      "arrays are fun "
date:       2019-07-24 22:07:26 +0000
permalink:  arrays_are_fun
---


Every day I'm in it I am just more and more excited to be doing this.  I love the feeling when I get my code, an otherwise seemingly nonsensical mess of ASCII, to actually execute as expected after tweaking it for hours and hours on end.  

I've noticed a recurring theme, it's almost always way easier to do than I make it out to be.  Take the Deli Counter lab for example.   

```
def take_a_number(deli, name)
    if deli = []
    deli << name
    puts "Welcome #{name}.  You are number 1 in line."
    
  else 
    deli.each.with_index(1) do |name, index|
      puts "Hi #{name} you are number #{index}."
    end
    
  end
       
end 

```


Ater spending over a DAY trying to figure out why this code wouldn't pass the test (despite working fine in IRB) I finally hit "ask a question."  had some great help from the coaches basically telling me I've overcomplicated this WAY too much and to look again at what the test is actually asking --- which was something way more simple!

```
def take_a_number(array, name)

  array.push(name)
  puts "Welcome, #{name}. You are number #{array.count} in line."
      
end 

```

Was all I needed to get it passing!  I'll keep this in mind going forward!  

But the important thing here is seeing how arrays are so powerfull - those two lines of code are all that was needed to pull of what seemed at first like a pretty complicated task!  I learned alot about arrays today and can't wait to keep working with them.   


