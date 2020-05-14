---
layout: post
title: The Reason for Delegates
description: Why Delegates exist and what they really are for.
modified: 2018-09-09
tags: [SWIFT, PROTOCOL, OO-CONCEPT]
comments: false
---

Starting with the UIApplicationDelegate, the UITextfieldDelegate and finally the UITableViewDelegate, you can't avoid Delegates when working as a developer in the Apple world. Every new developer who wants to develop applications for iOS or macOS has to learn when to expect them and how to handle them correctly. 

Some developers I met called them a good practice or a common pattern. They added own Delegate types to their code base or developed clever solutions to make them easier to handle or to hide them from the user. These people mostly did not understand the reasons behind them.

So lets take a look into Delegates, see why they exist and what they really are for.

<!--break-->

![Delegate it]({{ site.url }}/images/Delegate.jpg)

When writing a server or terminal program there is exactly one interaction between a user and your program. In pure object oriented programming thats the only place where you are allowed to call a method of one of your protocols. In most cases it should be something like `Application(arguments).execute()` or `Application(arguments).run()`. Most developers call this a touch point.

Frontend development is a little bit different, because the user interacts with your code not only by starting the application. There are more touch points like text input, button presses, scrolling content, and so on. But there are also indirect touch points like system events, ble device notifications and memory warnings. When developing for iOS or macOS these touchpoints are mostly provided by a Delegate.

Why Delegates?
----------

For an object oriented programmer Delegates are an antipattern. Instead we typically prefer dependency injection and object composition. So why is Apple using them? The answer is simple: Security reasons. Apple do not want to give us access to hardware and the so called private API's to protect the customer.

Let us play with a simplified example, a text input in a UITextField. Text is inserted by a keyboard that may be a touch display or a keyboard hardware. Adding keystrokes will fill a keyboard buffer. In code it may look like in the next picture.

![Keystroke without UI]({{ site.url }}/images/Keystroke1.jpg)

Typically the keyboard will not be visible on iOS without a first responder, in our example a UITextField. Without limitations an object oriented solution will add functionality without change the existing. So in our case we would design our UITextField by injecting the DataBuffer object as a datasource. To receive updates we create a decorator for the Buffer protocol and insert it between the Keyboard and the DataBuffer. We dependency inject the UITextField as an implementer of the Updatable protocol that implements the update method. Please note that in this implementation the UITextField do not need more api than this update method to work. The result is visible in the next picture.

![Keystroke with UI with full access to hardware and data]({{ site.url }}/images/Keystroke2.jpg)

Note: In addition I've added a BufferToText object to separate the knowledge of mapping ASCII to String. Our UITextField do not have to implement it than.

Now we run into a problem (the red lightnings). To implement it like that we have to know the DataBuffer and it's api protocol (to implement/use a Decorator and a BufferToText object) and we have to manipulate the object stack between the Keyboard and the DataBuffer. Both is not in the interest of Apple. Implementing a UITextField class is therefore not possible for us. 

To make this functionality available Apple implemented and inserted the necessary parts in the object stack and provided us a class UITextField. That class lives in both worlds. We are able to instantiate it and it has limited access to the internal implementations. It works as a fascade to provide access to internal knowledge like the text coming from the DataBuffer. But the events are asynchrone. They only occure when the user interacts with the device. In early iOS/macOS versions the block/closure language feature wasn't available yet. Thats why Apple provided the UITextfieldDelegate.

![Keystroke with UI and Delegate]({{ site.url }}/images/Keystroke3.jpg)

The result is a ***highly flexible customizable UITextField instead of several use case specific objects***. For the object consumers the possibilities are limited by the object api but combinations of the api features are possible. If the implementation isn't a fascade that splits the functionalities behind the api these are mostly classes of 1000 and more lines of code that grow infinitely. These classes are easy to use but mostly a pain to maintain.

What happens if you define own Delegates? 
----------

At first the user of your delegate has to handle the changes over time. Take a look into the UITableViewDelegate, it started in iOS 2 with already 15 methods. Over time it has grown until it reaches the currently 40 methods. ***Fourty functionalities in one protocol!*** And there might be more in the future! Who knows how far it will go?
Even ***the documentation split the functionalities into eleven categories*** to handle them. If you have to do something like that you have to recognize that you are far away from object oriented thinking. Hopefully the apple developers are aware of it and do it only for compatibility reasons.

These growing over time has some impacts. When implementing a complex tableview that uses most of the functionalities you end up with classes of hundreds of code lines automatically. It may be less functionality at the beginning but over time when adding more and more functionality it will be more and more difficult to maintain. In addition most developers are tempted to code procedural instead of object oriented in these classes and every depending class. That's understandable because Apple and Stackoverflow shows it in every example implementation.

In addition, if new mandatory functionalities has been added by the provider you have to extend every current implementation with the new functionality. In worst case you have to touch several files of your code depending on your project size and scope.

So Delegates are in conflict with the oop idea of coding an object once and handle it as readonly then (except for bugfixes).

Conclusion
----------

Delegates are touch points, triggered by users of your app or device hardware. They provide a bridge between hidden encapsulated functionalities and your software. 

Only if you provide closed source software components, you may need to define new Delegate types. In any other case there are better ways. 

If you need to implement provided Delegates try to design them as single objects and change them to fascades early when the complexity grows to make them maintainable. Try to avoid the specialized UITableViewController to avoid a class of hundreds of code lines. Better use a UIViewController with a UITableView and two objects for the delegate and datasource from the beginning.
