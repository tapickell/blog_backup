---
layout: post
title: "New Android Project Under Way"
date: 2013-03-11 23:43
comments: true
categories:
---

I am starting week 2 of development for my next Android App, BaristaLog.
This is a productivity app that will be useful for barista’s working at
coffee shops and for coffee enthusiasts that like to make specialty
coffee at home as well. It will have timers for different types of
coffee devices and a log for barsita’s to capture notes for times and
settings that produce the best cup. The log notes will be sortable by
best rating for device and best rating for coffee used. I have been
working on creating this app using a BDD/TDD style workflow. It seems to
take a bit longer to get features implemented but it is nice to have the
tests in place when I start refactoring parts of the program.<!-- more
-->

So far I have implemented a feature that allows the barista to have a
countdown style timer for 5 devices, Aeropress, Chemex, Clever, French
Press and single serving pour over. The countdown timer has a sub timer
that counts down times for specific parts of the coffee making process
and a total timer that counts down the entire time. It also has a count
up timer for timing shots of espresso. It uses audio cues to signal the
end of countdowns. I implemented a feature that allows the barista to
edit the times for each device and to restore the edited times back to
the defaults. This allows the user to set the times according to what
works best for the coffee they are using.

The log feature will take the times from the last cup and combine them
with notes entered by the barista, date, time, optional picture and
possibly other data from device sensors and then save it to the sqlite
database on the device. It will then present all of the logs in a list
view that can display logs per device or logs per coffee used. The list
view will be sorted by the best rating or most recent. I am unsure of
the rating scale yet, thinking about using a 1-5 with either stars,
coffee cups or coffee beans to represent the ratings. Another feature I
would like to implement is the ability for a barista to share a log
entry or note with other app users.

I feel like I am working backwards on this app when compared to my last
apps. The Giddy Goat apps I started with developing for iOS and began
with creating the graphics and the views using storyboards then moved on
to adding functionality and then adding OCUnit tests. This time I am
starting with Cucumber tests, then adding basic views, then adding Junit
tests, then adding functionality. I will be adding graphics after all
the functionality is implemented and tested. After the Android version
is available from the Google Play store then I will start on the iOS
version.
<p><img src="https://dl.dropbox.com/u/21041180/Screenshot_2013-03-11-07-34-36.png" alt="Timer Screen" style="width: 200px;"/><img src="https://dl.dropbox.com/u/21041180/Screenshot_2013-03-11-07-34-51.png" alt="Settings Screen" style="width: 200px;"/></p>
