---
layout: post
title: "For your coding enjoyment"
date: 2013-07-18 08:52
comments: true
categories:
---

For this post, I decided to do a little comparison of solving a common programming problem in three languages that I have experience writing in and one language that is new to me. I want to compare the approach to solving the problem for each language and go over some of the differences. I also want to go over some of the obstacles I had to overcome with each choosen language.<!-- more -->  

I choose the FizzBuzz problem, as a good problem to tackle and use for comparison. It uses a bit of math, string manipulation, interation and outputing to the console. If you haven't heard of this problem yet [Jeff Atwood](https://twitter.com/codinghorror) has a nice [blog post](http://www.codinghorror.com/blog/2007/02/why-cant-programmers-program.html) about it. It is typically used as an interview question for developers to ensure that they can solve simple programming problems.

_With these examples I wanted to use classes whenever possible to use more of an object oriented design, even though I know the problem can easily be solved with a procedural design as well._  

###FizzBuzz  
Write a program that prints the numbers from 1 to N. But for multiples of three print "Fizz" instead of the number and for the multiples of five print "Buzz". For numbers which are multiples of both three and five print "Fizz Buzz".  

###Objective-C
I thought it would be a good exercise, to write a FizzBuzz in Objective-C, since I typically only code in Objective-C when I am working on something for iOS.  
  
The most noticable difference for Objective-C is the seperation of the interface and implementation of the class with the `FizzBuzz.h` and `FizzBuzz.m` files and then the `main.m` file that instantiates an instance of the FizzBuzz class and sends the `output` message to it passing an argument of `42`.  


{% codeblock lang:objc FizzBuzz.h %}
#import <Foundation/Foundation.h>

@interface FizzBuzz : NSObject
- (void) output:(int)number;
@end
{% endcodeblock %}

I originally wanted to use enumeration with a block on a range of numbers in a set, but hit some snags on converting variables back and forth that made a mess of the code. So for readablilty and simplicity I refactored everything down to a basic for loop to iterate through the range of 1 to N and an NSMutableString to append strings onto.  

{% codeblock lang:objc FizzBuzz.m %}
#import "FizzBuzz.h"

@implementation FizzBuzz
- (void) output:(int)number
{
    for (int i = 1; i <= number; i++) {
        NSMutableString *output = [NSMutableString stringWithString: @""];
        if (i % 3 == 0) { [output appendString: @"Fizz "]; }
        if (i % 5 == 0) { [output appendString: @"Buzz"]; }
        if (output.length == 0) { [output appendString: [NSString stringWithFormat:@"%d", i]]; }
        printf("%s\n", [output UTF8String]);
    } 
}
@end
{% endcodeblock %}

If you are unfamiliar with Objective-C and are curious about the strange syntax used to new up a class, in Objective-C you instantiate a class by sending the alloc message that allocates the space in memory for the instance and then sending the init message that creates the object. This is the same effect as calling FizzBuzz.new in Ruby. It's a bit strange at first but after you've used the syntax for a while you just get used to it.
{% codeblock lang:objc main.m %}
#import <Foundation/Foundation.h>
#import "FizzBuzz.h"

int main(int argc, const char * argv[])
{
    @autoreleasepool {
        FizzBuzz *fizzy = [[FizzBuzz alloc] init];
        [fizzy output: 42];
    }
    return 0;
}
{% endcodeblock %}

###Java

Just like in the Objective-C example where I used a main method to instantiate the FizzBuzz class and send the `output` message to it, in Java I used a similar technique. The main differences are that the interface and implementation
are not split into different files and that the FizzBuzz class is a private class within the Examples class. It is the run method that is called from main that creates an instance of FizzBuzz and calls the `output` method on it. The implementation of the `output` method is similar to that of the Objective-C example. It uses a standard for loop to iterate from 1 to N and then a StringBuilder to append strings together.  

One thing that I think would imporve readability in Java is a bit of syntatic sugar for classes like `Integer` so that you could call an instance level `variable.toString()` instead of calling the class level `Integer.toString(variable)`.  

{% codeblock lang:java FizzBuzz.java %}
import java.lang.*;

public class Examples {
    public static void main (String[] args) {
        Examples examples = new Examples();
        examples.run();
    }

    public void run() {
        FizzBuzz fizzy = new FizzBuzz();
        fizzy.output(42);
    }

    private class FizzBuzz {

        public void output(Integer number) {
            for (int i = 1; i <= number; i++) {
                StringBuilder output = new StringBuilder();
                if (i % 3 == 0) {
                    output.append("Fizz ");
                }
                if (i % 5 == 0) {
                    output.append("Buzz");
                }
                if (output.length() == 0) {
                    output.append(Integer.toString(i));
                }
                System.out.println(output);
            }
        }
    }
}
{% endcodeblock %}

###Ruby   
The Integer class in Ruby has a really nice `upto` instance method that replaces the for loop seen in the other two examples. This method itereates the following block passing it the current integer into the num variable. This greatly simplifies the code and makes it more readable. If you are unfamiliar with blocks in Ruby the block that the `upto` method is iterating through is the code from the following `do` till the next `end` keyword. If you come from a C-ish background and prefer curley brackets you can replace the `do` and `end` keywords with curley brackets as well. Personally I love Ruby's lack of curleys in comparison to some other languages.  

The same approach is used with building an ouput string up to be printed out at the end of the block. Ruby has a nice syntax for appending to strings by using `<<`. One thing that is different is the if statements in this part of the method. In Ruby, when there is only a single line of code to be executed you can place the if statement after the code. This allows for a "do this, if that is true" type of syntax. You can still do it the other way as well, `if num % 3 == 0 output << "Fizz "`. It is up to the developers preference and what makes the code readable.  
{% codeblock lang:ruby FizzBuzz.rb %}
#!/usr/bin/env ruby

class FizzBuzz
  def output(number)
    1.upto(number) do |num|
      output = ""
      output << "Fizz " if num % 3 == 0
      output << "Buzz" if num % 5 == 0
      output << num.to_s if output == ""
      puts output
    end
  end
end

fizzy = FizzBuzz.new
fizzy.output(42)
{% endcodeblock %}

Groovy
{% codeblock lang:groovy FizzBuzz.groovy %}
def output(Integer number) {
  1.upto(number) { num ->
    def output = ""
    if (num % 3 == 0) { output = output + "Fizz " }
    if (num % 5 == 0) { output = output + "Buzz" }
    if (output == "") { output = num.toString() }
    println output
  }
}
output(42)
{% endcodeblock %}
