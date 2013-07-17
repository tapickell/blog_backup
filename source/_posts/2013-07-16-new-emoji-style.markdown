---
layout: post
title: "New Emoji Style"
date: 2013-07-16 18:49
comments: true
categories: 
---

I have been spending a lot of time writing code in Vim recently as I have been delving deeper into the wonderful world of Ruby development. I have also been using Tmux for window and pane managment and have found that the two programs go together quite nicely. Usually when I am coding in Vim and Tmux, I am running iTerm in full screen. On my MBP I need to keep an eye on the battery level but I don't want to break my workflow to mouse to the top of the screen to peek at the battery percentage on the menu bar. So, I set out to create a Ruby app to display my battery level right in the Tmux status bar.<!-- more -->  

No need to stop coding or even move my fingers away from the home row. Now at a glance I can tell what my battery level is and I will receive a message in notification center when it starts getting low. This setup uses the BSD Power Managment Settings API by calling `pmset -g batt` which outputs a snapshot of battery/power source state and then piping that data to the application. Inside the app the required data is parsed from the arguments passed in and used to determine what to display. Now if I wanted to keep it simple I could have just had it output the current percentage, but that is too easy. While I am taking the time to create this I might as well bake in some extra functionality and make it look cool as well.  

I have been using [Aaron Kromer's](http://aaronkromer.com "Aaron Kromer's Blog") [emoji-rspec](https://github.com/cupakromer/emoji-rspec "emoji-rspec formatter") gem with some of my projects. Yes I know using emoji for you rspec formatters is not necessary for TDD but I think it looks cool. So when I was thinking about how I want to represent the battery level in the Tmux status bar I thought how cool would it be to use hearts just like the health meter in Zelda. So the app uses emoji characters to display the battery level. Unfortnatley the red hearts didn't work out as originally hoped to give it that classic Zelda look, so I used green, blue, pink and gold to denote diferent states of the battery and charging system on the MBP.  

The lighting bolt and gold full hearts are used to denote when the laptop is plugged into a/c and charging, the battery and green full hearts when it is not plugged in.  

{% img /images/post_pics/charging_battery.png %}   
{% img /images/post_pics/discharging_battery.png %}  
{% img /images/post_pics/charged_battery.png %}  
{% img /images/post_pics/charged_ac.png %}  
{% img /images/post_pics/dead_battery.png %}  

If you like this then clone it from [Github](https://github.com/myappleguy/tmux_battery_display) and use it in your Tmux status bar.