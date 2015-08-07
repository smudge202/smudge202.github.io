---
layout: post
title: Abstracting
hidden: true
---

The term _"abstract"_ is thrown around often within the realms of programming. There are numerous resources that explain the various interpretations of "abstract" as well as usages thereof. It's a term that I too commonly refer to and therefore would like to ellaborate upon. Not only to better explain the principle and usage, but also to offer tips on how to introduce _abstractions_ into your otherwise, rotting products, and later demonstrate the benefits afforded.

## Definition

A [quick google](https://www.google.co.uk/#q=define%20abstract) reveals that the word "abstract" can be used in adjective, verb, and noun forms, each with more than one meaning. No wonder there's some confusion!

So what is it we mean when we're talking about _abstractions_?

## `abstract` class

Of course, we have the `abstract` keyword which can be defined on a class declaration and the methods of such a class. When declared against the class itself we are stating that the class **must be inherited** and subsequently, instances cannot be constructed for the `abstract class` itself.

Similarly, the `abstract` keyword when used with a method means that the method signature must be fulfilled by derivatives (classes inheriting from the abstract class that are not themselves abstract).

Although there is some relation, this `abstract` keyword is typically not what is being referred to when people mention _abstractions_.

## So what is it?

To understand this, it may be best to examine Object-Orientated Programming. A term I'm sure you're familar with, but in itself hosts a great deal of confusion as interpretations have emerged over the years. One of, if not _the_ father of OOP, is [Alan Kay](https://en.wikipedia.org/wiki/Alan_Kay) whom [described the term](http://userpage.fu-berlin.de/~ram/pub/pub_jf47ht81Ht/doc_kay_oop_en) as follows:

> OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things.

## Messaging

Let's examine the first of Alan's points, _messaging_. I am in agreement with [Jörg W Mittag's excellent interpretation](http://programmers.stackexchange.com/a/253121) and research.

> Implementation-wise, messaging is a late-bound procedure call, and if procedure calls are late-bound, then you cannot know at design time what you are going to call

> Messaging is fundamental to OO, both as metaphor and as a mechanism.

> If you send someone a message, you don't know what they do with it. The only thing you can observe, is their response

For me, this is the premise of _""abstraction""_; it should be the goal of you and the code you write. I find it fascinating that the father of OOP had this amazing vision for what he later regrets not having called "message-orientated programming", but now so many developers barely understand the core principles.

## Re-envisioned & Re-visited

Much later in the relatively short lifespan of software development, a group of engineers got together to discuss and ellaborate upon a number of concepts, and to put together guidance for both their peers and future generations. This [gang of four](https://en.wikipedia.org/wiki/Design_Patterns) produced what has now been refined to the 24 Creational, Behavioural, and Structural [design patterns of OOP](http://www.oodesign.com/).

Then came along everyone's favourite, [Uncle Bob](https://en.wikipedia.org/wiki/Robert_Cecil_Martin), delivering to us the 5 core principles in the shape of [SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)). **All** of these principles, relate back to _abstraction_, if not as predecessor to a principle, then as proponent and guidance.

It beggars belief at times just how few within our field are aware of these things. But that is not the fault of our juniors and newcomers, **it is the fault of our seniors** and their lack of mentoring, collaboration, and propagation of knowledge.

## Abstraction

We now have all these little pieces of information, but still no firm grasp of what is is abstraction *means*. Of the many well structured definitions there are out there which describe abstractions, I'll go with my own account:

> Abstraction is the technique employed to remove unnecessary implementation detail in order to provide a relevant, clean context for consumers.

The fact that abstractions can afford us [Inversion of Control](https://en.wikipedia.org/wiki/Inversion_of_control) we'll ignore for now and just concentrate on the basics. How do we remove unnecessary implementation detail? Simple, we abstract the implementation. Be that by interface or by class, the process remains the same, so let's examine this by example.

Here is an example of what is known as _"tightly coupled"_ code:

```c#
static void Main(string[] args)
{
  var order = new CreateOrder();
  order.Create(args);
}

sealed class CreateOrder
{
  internal void Create(string[] args)
  {
    var parser = new ArgumentParser();
    var order = parser.ParseOrder(args);
    
    using (var database = new DatabaseContext())
      database.Store(order);
    
    Console.WriteLine($"Order with ID {order.OrderId} has been created");
  }
}
```

I've intentionally not included details about the `ArgumentParser` and `DatabaseContext` classes, but the intent is that the former parses the `string[]` passed into our console application, and the latter stores the order into our database. Great, right? The code is simple to read (I hope), and ignoring skipped exception handling, would probably work perfectly fine.

However, it isn't right. It isn't great. As you learn more about C#, .Net, and development as a whole, you'll find there's a million different ways you can accomplish a given task and _make it work_. I _could_ have written all the code in the `Main` method, but does it mean I _should_? (If you answer `yes` to that, you can leave now and go back to playing flappy-birds). If you consider the above to be fairly clean, then that's good. Not because you're right, but because **you are my target audience**.

## Requirements

We try to keep our code clean because we typically add new features and maintain our applications. We want to be able to understand our code when we go back to it, we want to be able to find the right place to make changes, and we want the new features and maintenance to be _easy_.

So how do we abstract the above example?