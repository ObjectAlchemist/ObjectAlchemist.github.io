---
layout: post
title: Swifty protocols
description: Using protocols in object oriented programming.
modified: 2017-05-06
tags: [PROTOCOL, SWIFT]
comments: false
---

Today I read some questions and answers at Stackoverflow. And there was one question about composition over inheritance. The question wasn't really interesting for me, more one of the answers. The writer described the typically inheritance problem with a simple example, the classic composition alternative and the swifty "Protocol Oriented Programming".
I think it is a good example to show you how to code object oriented.

<!--break-->

Let's start with the simple code example:

``` swift

class Robot {
    var battery = 0
    
    func charge() {
        print("⚡️")
        battery += 1
    }
}

class Human {
    func eat() {
        print("🍽")
    }
}

class RobotCleaner: Robot {
    func clean() {
        print("💧")
    }
}

class HumanCleaner: Human {
    func clean() {
        print("💧")
    }
}

```

It's obvious that the clean() function is implemented twice. So the writer told us how it should be done in the swifty "Protocol Oriented Programming" way. The solution looks like that:

``` swift

protocol Robot {
    var battery: Int { get set }
}
extension Robot {
    mutating func charge() {
        print("⚡️")
        battery += 1
    }
}

protocol Human { }
extension Human {
    func eat() {
        print("🍽")
    }
}

protocol Cleaner { }
extension Cleaner {
    func clean() {
        print("💧")
    }
}

struct HumanCleaner: Human, Cleaner { }

struct RobotCleaner: Robot, Cleaner {
    var battery: Int = 0
}

let humanCleaner = HumanCleaner()
let robotCleaner = RobotCleaner()

```

This looks really nice. The three functions charge(), eat() and clean() are implemented only once. There is no more code duplication. We all should code like that, or?

Ok that sounds hard, but I would never code it like that. Yes I know, it's exactly the way that apple told us in the WWDC talk about "Protocol Oriented Programming" and it even may be some kind of protocol oriented, but it's definetly not object oriented. Let us think about this code. 

The Protocol definitions
----------

A protocol describes an ability the implementing object has. In our example there is a protocol named Human. If you think about it you should ask yourself the question "What does it mean to be Human?". 

Ok that's a kind of philosophical question, but we are developer. So let us change it into "What is an object able to do if it is Human?". It's not obvious because the protocol is empty at first look, but the function in the extension tell it to us "eat". It seems a little bit weird to reduce a human to the function "eat" but ok. And now the key question: "How many different implementations of human eaters do you expect?". Don't we eat all the same way? That's why it's a bad protocol for me.

The definition of the protocol Human makes only sense for that kind of usage. It's not driven by need, it's driven by the style of coding. 

The Extensibility
----------

The struct HumanCleaner shows us how we use the functionality implemented by the extensions. 

``` swift

struct HumanCleaner: Human, Cleaner { }

```

We select functions and attach them to one large struct/class. In that way it's possible to create abominations like HumanMusicanLoverDeveloperSleeperDriverCleaner. Naming them will become very interesting. 
It may not be exactly that abomination but I ensure you that someday you will find something like that in your team code. And if it's deeply coupled to other parts of your code you have to refactor very soon.

That abomination shows us what happens when our functionality base grows. But thats only one kind of extensibility. It's also possible to combine existing functionalities that shouldn't be combined. Our simple example above already has that ability. How about that:

``` swift

struct HumanRoboter: Human, Roboter { }
let humanRoboter = HumanRoboter()

```

I would say: "We are doomed!!!" 😳

And there is another issue we have to handle. Lets define that:

``` swift

extension Human {
    func clean() {
        print("🛀")
    }
}

```

Now you get an error "Ambigous use of clean()" at every code line that already used the function clean() on a HumanCleaner instance. Let me say it again "Your existing code failed because of new code"! This should never happen. 
Just think about it. What happens if the Human and Cleaner implementations are located in a framework/library and you use them in that way? Would you expect failures when updatung the framework 1.0.0 to 1.1.0? There is no api change only a new feature told by semantic versioning. How can it breach existing code?

Mutability
----------

Lets take a look at the Robot again. 

``` swift

protocol Robot {
    var battery: Int { get set }
}
extension Robot {
    mutating func charge() {
        print("⚡️")
        battery += 1
    }
}

````

The protocol provide the ability to read/write an integer. You may say that the Robot is reduced to a value. Thats how every object ends if you implement it like that. The Human without a base functionality will not be the final implementation. A print statement is only an example for some real code. And normally there is interaction with other objects needed, like with an Int in case of the Robot. So if your protocols provide access to that values they provide access to the internal state. That's what we call mutable objects.

The Object oriented approach
----------

The writer of the example above would call it the classical way. But in my opinion it's a far more better one than the apple solution. Lets take a look on my 'classical' solution.

``` swift

// datasource protocols

protocol EnergyStorage {
    func provide(_ energy: Int)
}

// functionality protocols

protocol Consumer {
    func charge()
}

protocol Cleaner {
    func clean()
}

// objects

final class LithiumBattery: EnergyStorage {
    private var value: Int = 0
    private let maximum: Int 

    init(maximum: Int = 100) {
        self.maximum = maximum
    }

    func provide(_ energy: Int) {
        value = min(value+energy, maximum)
    }
}

final class Robot: Consumer {
    private let storage: EnergyStorage
    
    init(storage: EnergyStorage) {
        self.storage = storage
    }
    
    func charge() {
        print("⚡️")
        storage.increase(1)
    }
}

final class Human: Consumer {
    func charge() {
        print("🍽")
    }
}

final class RoomCleaner: Consumer, Cleaner {
    private let decorated: Consumer

    init(_ decorated: Consumer) {
        self.decorated = decorated
    }
    
    func charge() {
        decorated.charge()
    }
    
    func clean() {
        print("💧")
    }
}

let robotCleaner = RoomCleaner(Robot(storage: LithiumBattery()))
let humanCleaner = RoomCleaner(Human())

```

It looks different. So let us discuss the changes.

At first: It's more code. Yes it is! Less code doesn't make it better maintainable or testable. Thats a typical missunderstanding of software development. The keys for good software are readability and reusability instead.

Second: I split the protocols and objects into datasources and functionality. In normal life there shouldn't be much datasource definitions. Theese objects are really rare, because theese are the low level access protocols to in memory bytes, database values or file bytes.

Third: Every object has a single responsibility. Even the powersource of the Robot is extracted as an EnergyStorage. The Robot has no knowledge about how the battery works, he only knows how to use it. And the battery is hidden by making it private. The Robot is an immutable object like any of my objects except the datasources. But even a datasource do not provide access to the internal state.

Forth: I make the Robot and the Human equal. Yes I know it's hard to accept, but both charge energy. It may be different, but it's true. That makes it possible to define a Consumer protocol. A protocol that is reusable for other implementations, much more than a Human or Robot protocol would be.

Fifth: I use decoration for functionality enhancement. By that I limit the possibility of cleaning to Consumers. That may be a little bit weird and normally I wouldn't concat these two different things, but it's only an example. Without another implementation of the Cleaner protocol only Consumer are extendible with the clean functionality, you may call it "learn". It isn't possible to combine functionalities in an unwanted way like a HumanRobot.
What about a cat? A cat is a consumer but can not be a cleaner. That shows us an architectural problem. 

Hint: If a decorator implementation only route the functions of the decorated object there is something wrong with the protocol or the decorator.

Sixth: The codebase is easy to extend. You can implement a consumer group as a decorator and use them without changing the existing (tested) functionality:

``` swift

let groupCleaner = RoomCleaner(ConsumerGroup([Human(), Human(), Human()]))

```

Conclusion
----------

Coding 'classical' protocol based object oriented style let your tested codebase grow day by day. You can reuse everything from small simple objects up to larger complex compositions without sideeffects. 

Only the business logic of your app will change depending on your needs. In my implementation above the business logic is reduced to two readable and easy maintanable lines:

```swift

let robotCleaner = RoomCleaner(Robot(storage: LithiumBattery()))
let humanCleaner = RoomCleaner(Human())

```

*Original example source: [Stackoverflow](http://stackoverflow.com/a/43820459/6595536)*
