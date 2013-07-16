---
layout: post
title: "Maven My Robotium With Cucumbers Please."
date: 2013-03-1 21:18
comments: true
categories:
---
##My attempt to use Maven, Cucumber, Robotium, JUnit and Jenkins together with Android.

As i grow as a developer I am always trying to learn new techniques and
tools, and I try to implement them into the projects I am currently
working on. I have recentley benn on a TDD/BDD/QA kick and have been
looking for an excuse to use a bunch of these tools together without too
much disaster. I am starting another Android app to be released to the
Google Play market. This seems like a good oppurtunity to add TDD/BDD to
my Android work-flow.<!-- more -->

My last Android app I used Robotium and JUnit for
unit and functional testing. This was not a test driven wokflow though,
only testing after the fact. Time to step it up a notch. I have read on
some other blogs that Test Driven Development in for mobile applications
can be a daunting task at times. This could prove to be challenging.

+    First Try

First I created a blank Android project and a Maven project for the
test suite. I found with Android it can be better to create a test suite
project outside of your actual Android project. This keeps your package
smaller and the project cleaner. I have added a `project.features` file
and a `projectstepdefs.java` file to the test suite. This is using
cucumber-jvm which I had some success testing web apps with. I included
the dependencies in the `pom.xml` file for cucumber, junit and robotium.

{% codeblock lang:xml %}
     <dependencies>
       <dependency>
         <groupId>info.cukes</groupId>
         <artifactId>cucumber-jvm</artifactId>
         <version>1.1.1</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>info.cukes</groupId>
         <artifactId>cucumber-junit</artifactId>
         <version>1.1.1</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.10</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>com.jayway.android.robotium</groupId>
         <artifactId>robotium-solo</artifactId>
         <version>3.6</version>
       </dependency>
     </dependencies>
{% endcodeblock %}

I ran mvn test and got back there are no tests to run and build success!
So exciting I know. I added the `RunCukesTest.java` to get cucumber to
start working, added a simple feature and matching step-def, then ran
mvn test again. “NoClassDefFoundError:”

I figured out that because I created a Maven project instead of an
Android Test project I didn’t have any thing for Android so Robotium was
getting an error. Next I tried adding Android to the dependencies in the
pom file, that should fix it right? Not so fast grasshopper! That just
created more issues with dependencies in my pom file. It was telling me
it couldn’t get the Android files basically. I tried mvn install:file to
install them from my local (android-sdk/…). No fix.

+   Abstract it out

OK, what if I abstract away my problems? Next I tried putting the calls
to Robotium into the test file in the app (the app is an Android project
anyways) and calling that class from my step def file. This should
totally work, right? No Robotium calls in my test suite only step-defs
that call other functions in the test located within the app. Swing and
a miss. Ok, maybe I just need to take a step back and think about this
for a moment. I think I am trying to do too much at once, Maven,
Cucumber, Robotium, Junit and I haven’t even tossed Jenkins into the mix
yet. Let’s simplify this. I gave Maven a toss over the shoulder and put
Jenkins aside for now. Now I have Cucumber, Robotium and Junit left. I
know I can run Junit tests on my own for unit testing and such. I also
know I can use Robotium to do testing in Junit. But what about the whole
BDD/TDD approach and Cucumber?

+    Ruby to the rescue

Well actually calabash-android, Ruby and Pragmatic Programmer to the
rescue. The recipe was very straight forward.

{% codeblock lang:bash %}
$ gem install calabash-android

$ calabash-android gen

$ cd features

$ rm my_first.feature

$ touch my_demo.feature

$ vim .
{% endcodeblock %}

Added a simple feature and scenario to the feature file.

{% codeblock lang:ruby %}
  Feature: Launch

    Scenario: See text on launch
        When I launch the app
            Then I should see "Hello world!"
{% endcodeblock %}

Then I ran calabash-android run ../bin/myDemoApp.apk and got success in
the form of a failing test!

{% codeblock lang:ruby %}
Feature: Launch

  Scenario: See text on launch       # features/launch.feature:3
5231 KB/s (528123 bytes in 0.098s)
4611 KB/s (181624 bytes in 0.038s)
      When I launch the app            # features/launch.feature:4
          Then I should see "Hello world!" #

1 scenario (1 undefined)
2 steps (1 skipped, 1 undefined)
0m21.567s

You can implement step definitions for undefined steps with these snippets:

      When /^I launch the app$/ do
        pending # express the regexp above with the code you wish you had
      end
{% endcodeblock %}

Awesome! Next I copy/paste the suggested step def into
features/step_definitions/app_steps.rb and ran it again. Legen- wait for
it…

{% codeblock lang:ruby %}
Feature: Launch

  Scenario: See text on launch       # features/launch.feature:3
  2622 KB/s (528123 bytes in 0.196s)
  3484 KB/s (181624 bytes in 0.050s)
      When I launch the app   #features/step_definitions/launch_steps.rb:2
            TODO (Cucumber::Pending)
            ./features/step_definitions/launch_steps.rb:3:in `/^I launch the app$/'
            features/launch.feature:4:in `When I launch the app'
      Then I should see "Hello world!" #calabash-android-0.4.2/lib/calabash-android/steps/assert_steps.rb:9

1 scenario (1 pending)
2 steps (1 skipped, 1 pending)
0m21.872s
{% endcodeblock %}

-dairy!, It was actually working. I was so stoked. Now to actually get
it to pass I just removed the pending from the step def, because it
isn’t really testing anything. On the next run I get cool green cukes.
It passes because calabash has a canned step def for `Then /^I should see
"([^\"]*)"$/` so I didn’t have to write a step def for that one.

{% codeblock lang:ruby %}
Feature: Launch

  Scenario: See text on launch       # features/launch.feature:3
  2225 KB/s (528123 bytes in 0.231s)
  3375 KB/s (181624 bytes in 0.052s)
      When I launch the app            #features/step_definitions/launch_steps.rb:2
      Then I should see "Hello world!" #calabash-android-0.4.2/lib/calabash-android/steps/assert_steps.rb:9

1 scenario (1 passed)
2 steps (2 passed)
0m24.688s
{% endcodeblock %}
