---
layout: post
title:  "iOS/Swift: Testing Swift Apps - Lessons Learned"
date:   2016-09-11 22:00:00 +0100
categories: Swift,iOS
permalink: "/testing-swift-apps-lessons-learned"
---
At Mozio, we're always trying to deliver the highest quality software we can. A big part of achieving that is having a solid testing framework. In this post, I'm going to share a few lessons learned along the way in that area, specifically for Swift projects.

## Lesson one: Dependency inject all the things!
Seriously, testing logic that depends on other units of your project will be easy if you follow that.

Here's an example situation:
you have a class that does a complex calculation using a `Calculator` object. So you define your class like that:

```swift
class SomeClass {
  let calculator: Calculator = Calculator()

  func calculateSomething(leftValue: Int, rightValue: Int) -> Int {
    return self.calculator.doMath(leftValue, rightValue)
  }
}
```

### Question: how do you test `SomeClass.calculateSomething(..)` separately from `Calculator`?
Answer: With this implementation, you can't! Well that's not entirely true, but the methods used to make this possible are not pretty at all. Read on for a much better solution.

### Solution: Initializer injection
Sure, there are other types of injection. But this one is probably the easiest. The concept of initializer injection is simple: an object doesn't create its own dependencies. They get handed to it in the initializer. See below how that looks for the `Calculator` example:

```swift
class SomeClass {
  // We're not initializing `calculator` below anymore:
  let calculator: Calculator // How about making Calculator a protocol? Even cleaner!

  // We receive a `Calculator` instance in the initializer and use that to do the calculations:
  init(calculator: Calculator) {
    self.calculator = calculator
  }

  func calculateSomething(leftValue: Int, rightValue: Int) -> Int {
    return self.calculator.doMath(leftValue, rightValue)
  }
}
```

The benefits here are huge: when you're testing `SomeClass`, you can pass in a mocked `Calculator` object to the initializer, made just for testing purposes. That means when you're testing the `calculateSomething(..)` method, you have full control over what that `self.calculator.doMath(..)` call will return, and you don't have to worry about the internals of `Calculator`. After all, you're testing `SomeClass`, not `Calculator`, right?

## Lesson two: Handling big initializers
Here's the scenario: the class you are testing has a lot of dependencies. You've nailed it by injecting them all instead of creating them internally. Good! But you ended up with a huge initializer. That's no good! Here's how to solve that:

### Solution: Using a configuration object
Super simple: instead of receiving all of your dependencies directly in the constructor, you can receive a configuration object. Structs have implicit initializers, so if you don't need any specific case (like handling optionals), it's very clean. Here's an example:

```swift
struct ClassWithLotsOfDependenciesConfig {
  let someDep: SomeDependency
  let anotherDep: FunDependency
  let yetAnotherDep: NotSoFunDependency
  //...
}
class ClassWithLotsOfDependencies {
  let someDep: SomeDependency
  let anotherDep: FunDependency
  let yetAnotherDep: NotSoFunDependency

  init(configuration: ClassWithLotsOfDependenciesConfig) {
    // Here you populate all the dependencies from the config object
  }
}
```

## Lesson three: Keep things simple
This is not important only for testing, it's a good practice to follow even if you're not testing (why are you not testing by the way!?). The idea here is to keep your classes/structs/whatever focused. It's all about the `S` in <a href="https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)">SOLID</a>.

The reason behind that is: it's much easier to test specific parts of your code if they are very objective. If you have a method that does 10 things at once, you'll need to include assertions for all that when writing tests. And that means checking all the possible input/output combinations of all 10 things. This will not only make tests huge and complex, it can also mean you're missing a few cases.

### Solution: Single responsibility
A good rule of thumb here: _if you can logically split a method into multiple methods, do it_. It will make testing them a lot simpler.

Similarly, if you can logically split a class into multiple classes, do it. For the same reason: testing them in isolation will be much easier.

## Comments
Testing is extremely important on any software project. Making the right architectural decisions makes testing much easier and efficient. In this post I shared just a small number of lessons learned from working with tests in Swift. I plan on writing more posts like this one, with some other information we found useful.
