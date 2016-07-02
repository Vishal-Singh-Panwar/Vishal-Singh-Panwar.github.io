---
layout: post
title: Swift protocols with 'get' and 'get set' properties.
---

`Protocols` in swift defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.

This is how we can decalre and conform to a simple protocol in Swift.

```swift
protocol Fooable {
    
}

struct Bar: Fooable {
    
}

```

## Properties in protocols

A protocol is set of rules. Any conforming type has to follow all the rules. As a part of rules, a protocol can require any conforming type to provide a property with a particular name and type. The conforming type can fulfill this requirement either by using a stored property or a computed property. To give some clarity, conforming type means the types (structs, classes, enums) which are conforing to a protocol. In the above code sample, `Fooable` is a protocol and `Bar` is the conforming type. Lets add some property requirement in `Fooable`.

**NOTE: Property requirements in a protocol are always declared as variable properties.**

```swift
protocol Fooable {
    var name: String {get}
}

```
There are different ways by which the conforming types can fulfill the property requirement of a protocol.

```swift
struct Bar: Fooable {
    let name = "Bar"
}

struct Buz: Fooable {
    var name = "Buz"
}

struct Qux: Fooable {
    let quxName = "Qux"
    
    var name: String {
        get {
            return quxName
        }
    }
}

```

Notice that even though inside protocols, properties are declared with var keyword only, we can use `let` for fulfilling `get` property requiremnet inside the conforming types. The only requirement of Foo is to provide a name, and all conforming types are free to provide it using a `let` or a `var`. 

Below is an example of get set property requirement in a protocol.

```swift
protocol Fooable {
    var name: String {get}
    var age: Int {get set}
}

// Fulfilling get set propety requirement:
struct Bar: Fooable {
    let name = "Bar"
    var age = 27 
}

struct Qux: Fooable {
    let quxName = "Qux"
    var quxAge = 20
    var name: String {
        get {
            return quxName
        }
    }

    var age: Int {
        get {
            return quxAge
        }
        set {
        	quxAge = newValue
        }
    }
}

```

If a protocol requires a property to be gettable and settable, that property requirement **cannot** be fulfilled by a constant (let) stored property or a read-only computed property. 

```swift
protocol Fooable {
    var name: String {get}
    var age: Int {get set}
}

struct Bar: Fooable {
    let name = "Bar"
    let age = 27 // Compilation Error: Candidate is not settable, but protocol requires it.
}

```

## How *get* propeties are different from *get set* properties in a protocol?

In this section, I will try to explain the difference between get propeties and get set properties.

A get property does not mean that the conforming type has to declare that propety as get only. Take below example:


```swift
protocol Fooable {
    var name: String {get}
    var age: Int {get set}
}

struct Bar: Fooable {
    var name = "Bar" // Conforming to `Fooable`
    var age = 27 
}

var someFooable = Bar()
someFooable.age = 20
someFooable.name = "Bar2"

```

As we can see, even though `name` is a get property requirement in the protocol, we are stil able to perform both get and set operations on `name` of `someFooable`. 

![Image alt]({% asset_path 984.jpg %} "confused")

## Explaination
By making `name` as get property in `Fooable`, we are asking the conforming types to provide a getter method with name `name`. Fooable is not stopping the conforming types to set the property `name`. 

The reason we are able to set `name` of `someFooable` is that since we have not explicity specified the type of `someFooable`, its type is being inferred as `Bar`. So we are actually setting `name` of `Bar` and not of `Fooable`. `name` is decalred as `var` in `Bar`, so we can set `name` in `someFooable`.

If we explicitly set the type of `someFooable` to be `Fooable`, then compiler wont allow us to set the `name`, as `Foobale` only gurantees that a getter with `name` exists and has no knowledge about the setter. Its totally upto the conforming types to provide a setter or not for a get-only property requirement of a protocol.  


```swift
protocol Fooable {
    var name: String {get}
    var age: Int {get set}
}

struct Bar: Fooable {
    var name = "Bar"
    var age = 27   // can only be var
}

var someFooable: Fooable = Bar()
someFooable.age = 20
someFooable.name = "Bar2" // Compilation Error: cannot assign to property: 'name' is a get-only property

```

I was always confused in the beginning that if we can set the properties which are marked as get in protocol, then how is it different from get set property. Later I realised that we are actualy using setter given by the conforming types. And I find this behaviour very useful in encapsulation. If you have also had this confusion, then I hope that this post might have made your head feel little lighter by now.



