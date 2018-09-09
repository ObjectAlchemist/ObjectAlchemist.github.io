---
layout: post
title: Lego Development
description: Writing code like playing Lego
modified: 2017-10-25
tags: [SWIFT, OO-CONCEPT]
comments: false
---

Do you know Lego? Of course you do. Everybody know it. 3.4 billion euro turnover per year speaks for itself. The idea behind it can't be so wrong, or? 

What is it that makes it successful? I think it's the simplicity. A child can understand it without a manual. It's easy to learn and you can build really complex structures with simple pieces. Wouldn't it be nice to code like Lego? Let's think about the Lego principles and how we can adapt them for software development.

<!--break-->

![A complex Lego castle]({{ site.url }}/images/LegoCastle.jpg)

To code like Lego we need some simple code-stones. Stones we do not have yet. Thats a small difference to Lego. As long as we do not have the basic stones we are the stone creators. So our task is to create usable stones, else nobody wan't to use them. So how do we create such stones?

Let's learn from Lego. A stone will be used more often if it has a small shape. A stone with only one pin is much more flexible usable than a stone with 50 pins. In swift development the shape of a stone we call protocol. The pins of the stone are therefore the protocol functions. A protocol with only one functionality is much more flexible than a protocol with 50. The code-stones that fullfill our protocol we call objects. Voila.

Lets see an example:

``` swift

public protocol OOExecutable: class {

    func execute()
    
}

public final class DoNothing: OOExecutable {
    
    public init() {}
    
    public func execute() {}
    
}

```

Now let us start building our castle with it:

``` swift

let button = ViewTextButton(title: "Update UI", action: DoNothing())
let screen = ScreenSimple(content: button)
window?.rootViewController = screen.ui

```

Like connecting two stones in Lego, we connected a ViewTextButton with our object DoNothing. And because action is defined as OOExecutable we can easily exchange the object with another one like a red stone with a green stone of same shape in Lego. And we can also do this with views in our Lego-coding world. 

``` swift

let label = ViewLabel(title: "Label")
let button = ViewTextButton(title: "Update UI", action: DoUpdateView(label))
let screen = ScreenSimple(content: 
    ViewStackVertical(content: [
        (height: 50, view: ViewSpace()),
        (height: 50, view: label),
        (height: 10, view: ViewSpace()),
        (height: 50, view: button),
        (height: VerticalStretched, view: ViewSpace())
    ])
)
window?.rootViewController = screen.ui

```

In the Lego world it's really hard to create 50 pin stones from one pin stones, so you don't like to do it. There might be a 50 pin stone there, but in our Lego-coding world we do not need such a stone if we already have the components to build one. Our advantage against the real world is that a once written object can be used several times. We can create more complex objects by wrapping them into an own world like complex Lego objects on a ground plate. This encapsulating subworld we call a Wrap.

``` swift

public final class ViewBorderedButton: OOViewWrap {
    
    public init(title: OOString, action: OOExecutable) {
        super.init(origin:
            ViewColored(color: ColorDefault(.black), content:
                ViewBordered(top: 1, bottom: 1, left: 1, right: 1, content:
                    ViewColored(color: ColorDefault(.green), content:
                        ViewBordered(top: 5, bottom: 5, left: 5, right: 5, content:
                            ViewTextButton(title: title, action: action)
                        )
                    )
                )
            )
        )
    }
    
}

```

``` swift

let label = ViewLabel(title: "Label")
let buttonUpdate = ViewBorderedButton(title: "Update UI", action: DoUpdateView(label))
let buttonLogin = ViewBorderedButton(title: "Logout", action: DoNothing())
let screen = ScreenSimple(content: 
    ViewStackVertical(content: [
        (height: 50, view: ViewSpace()),
        (height: 50, view: label),
        (height: 10, view: ViewSpace()),
        (height: 50, view: buttonUpdate),
        (height: 10, view: ViewSpace()),
        (height: 50, view: buttonLogin),
        (height: VerticalStretched, view: ViewSpace())
    ])
)
window?.rootViewController = screen.ui

```

As you see our castle grows and grows. And each simple object and each complex wrap is reusable by design. Follow these path and you will create marvelous castles created from simple objects like Lego. And if you need to build another castle you will see that you may build it from the same objects and wraps in less than half the time you need for the first one. 

That's what we call ~~Lego-Development~~ Object Oriented Programming.

Wanna see such code in action? See ExampleLegoWorld target on [GitHub](https://github.com/ObjectAlchemist/OOExamples)

