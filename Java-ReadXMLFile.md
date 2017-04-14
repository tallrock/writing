wrap:space
# Writing code

Writing code to parse an XML file - a journey

(This is here as a reminder of an idea that might become an article somewhere).

I needed to parse an XML file using Java. In a well-managed corporate development environment, I would have access to a set of debugged and packaged utility classes including a class called something like "ReadXMLFile" which would contain all the plumbing I needed. But I didn't have that, so I had a journey ahead of me ...

Step 1: Find a model
The easiest way to get going on a project is to steal code from a working source. One way is to find a library of code that does everything you want and import it whole. Bolt-on functionality. Another way is to find a chunk of source code that does most of what you want and copy / modify until you have your own custom solution. I am starting this with small ambitions so I hit up the Internet and found Java source code that already has most of the plumbing I'll need. Thank you anonymous programmer who originally created the progenitor code from which everyone has copy / modified.

Step 2: Wireframe
I stripped out the code I won't need and commented out the rest. I now have a basic XML File Parser. It will (a) read an XML file, (b) validate it, and (c) print the name of the top XML element. Nothing more. Compile and run. It works. This means I have all the dependencies required and the basic plumbing works.

Step 3: My public static void main
Java has a thing about class versus instance references. I deftly avoid this by getting out of the "main" method as fast as possible. I now change the "main" method to instantiate the class and call an instance method, for now called "go" as a placeholder. And for the moment I set it up so the "main" method calls the "go" method with the name of the XML file I'm testing with. Another quick run to see that I haven't broken anything.

Step 4: First level parsing
I write the code necessary to traverse the top level of my XML file. I copy liberally from the already existing commented out code. I stick some print statements in so I can see XML data elements and run it. It works.

Step 5: Second level parsing
I write the code to loop through the repeating elements of the XML file and parse both attributes and text content. This requires me to look at the XML file in one editor as a guide while write the navigation and extraction code in another editor window. Run again. It works. I'm on a roll.

Step 6: Introduce an error condition
What happens if I go for an element and it isn't there or go for an attribute that isn't there. Time to test that condition. I change the name of some element and attribute tags on purpose and run the program. Splat. An exception gets thrown. OK - I've learned something. I need some defensive code. All XML values are string values. If you want them in another format (integer, floating, date, whatever) you have to convert it yourself. For now I'm dealing with everything as string values. Later I'll start doing type conversions. But I have the "null" problem to deal with. A string can be "aaa", or "" or null. I write so helper methods. Both grab data from an XML element and return a non-null value. I rewrite the existing extraction code to use these helper methods. Compile, run. It works. It deftly handles the condition where I ask for something that isn't in the XML file. Good.

Step 7: First clean-up
I now have a class that calls a method that reads my XML file, teases out the data I want and prints that data on the console. It handles a basic error condition. There are more error conditions to be dealt with in the future. but we're alright for now. I sweep through the source code and remove unused code, neaten working code and add comments. 

Step 8: More error conditions
I add code to handle what happens if the XML file doesn't exist, or isn't valid or can't be read. I add the code to convert numeric values in my XML file from string into numeric formats. I add code to handle if there is a problem with those conversions. Run and test. There's a logic bug. Fix it, run again. Back on track.

What's next?

Future 1: What to do with the extracted data
At this point there are a couple options. I have these data elements. What am I going to do with them? And how do I get them smoothly to that destination? It makes sense to create a class that will hold these elements so I'm dealing with one entity (one object), not a pile of variables. (a) So I
could create that class, create an array of objects and return that array upstairs or pass it off to another method that further processes it. (b) I could pass down a "callback" method which will get called after each set of elements has been created and packaged into an object. (c) I could write a method within this class which further processes the data (with or without marshaling it into a convenience object).

Future 2: Generalization
Now that I have a working class that extracts data from an XML file, perhaps I should refactor it into a general ReadXMLFile class which I can extend for each practical application. The general class would contain all the plumbing needed to read and traverse an XML structure and examples of how to navigate and extract data so future programmers (or me a month from now) have clues allowing us to re-use the class.

Future 3: Different XML Parsers
There are a number of different parsers for XML files. They each have their best and worst uses. For what I was doing today,speed didn't matter. I was trying to get from "a" to "b" quickly (in programmer time, the CPU can fend for itself). But this may not always be the case. I have to know that this cute class might be a performance drain with really large XML files. Before packaging this class for re-use by other programmers in the development group, I should spend some time performance testing it against other XML Parsers for files of various sizes. (Or I should find somewhere in the Internet where someone has already done this). It may turn out that my efforts work for me today, but this class shouldn't be propagated to others in the development group.

Future 4: Packaging 
The relatively easy part is done. I can suck the data I need out of an XML file. But I now have a bunch of other things I'll need to do to create the application I am writing. That work will start now.


