# Swift — Calculated Property

## Objectives

1. Learn the calculated property syntax.
2. Identify when to utilize a calculated property.
3. Create a non-numerical calculated property.


Before getting started, create a new Swift-based Xcode project.

## Calculated Properties

Calculated properties can be used for properties that rely on the state of other properties. Instead of constantly updating the value of the property that depends on the state of other properties whenever those states change, we can write a calculated property which only determines its present value at the moment that it is accessed. The syntax for a calculated property generally follows:

```swift
    var propertyName: Type { return <#calculation#> }
```

**Objective-C:** *This is generally equivalent to writing a read-only property with a custom getter that returns a value based on the values of other properties.*

Consider a `Circle` class that has a `diameter` property:

```swift
class Circle {
    var diameter: Double = 1
    
    init(diameter: Double) {
        self.diameter = diameter
    }
}
```
The `diameter` property holds state, the width of the circle. If we wish to add another property that returns the `circumference`, it would be appropriate to create this as a calculated property that determines its value based on the current state of the circle's `diameter`. Since the mathematical definition of a circle's circumference is its diameter multiplied by *pi* (≈ 3.141592654), we can program this calculation into the `circumference` property:

```swift
class Circle {
    var diameter: Double = 1
    
    var circumference: Double { return diameter * M_PI }
    
    init(diameter: Double) {
        self.diameter = diameter
    }
}
```
From the AppDelegate we can create an instance of our class and print the `circumference`:

```swift
let circle = Circle(diameter: 1.0)
let bigCircle = Circle(diameter: 2.0)
        
print(circle.circumference)
print(bigCircle.circumference)
```

This will print:

```
3.14159265358979
6.28318530717959
```

### Non-numerical Calculated Properties

Calculated properties don't have to be mathematical calculations. They can return strings, booleans, or any other type as well. 

Consider our `Student` class from previous examples:

```swift
//  Student.swift

import Foundation

class Student {
    let username: String
    var firstName: String
    var lastName: String
    var email: String = ""
    var phone: String = ""
    
    init(username: String, firstName: String, lastName: String) {
        self.username = username
        self.firstName = firstName
        self.lastName = lastName
    }
}
```

If we want to add a `fullName` property, it would be appropriate to include it as a calculated property since the `firstName` and `lastName` properties are already holding the values we need. To avoid any potential that these properties could get desynchronized, we should set up `fullName` as a calculated property that interpolates `firstName` and `lastName` into a single string:

```swift
    var fullName: String { return "\(firstName) \(lastName)" }
```
Including it in the class's definition might look like this:

```swift
//  Student.swift

import Foundation

class Student {
    let username: String
    var firstName: String
    var lastName: String
    var email: String = ""
    var phone: String = ""
    
    var fullName: String { return "\(firstName) \(lastName)" }
    
    init(username: String, firstName: String, lastName: String) {
        self.username = username
        self.firstName = firstName
        self.lastName = lastName
    }
}
```
Now from the AppDelegate we can initialize an instance of our `Student` class with the necessary values and access (print, in this case) the `fullName` property just like the class's other properties:

```swift
let mark = Student(username: "markedwardmurray", firstName: "Mark", lastName: "Murray")
        
print(mark.fullName)
```
This will print: `Mark Murray`

If we change the state of the underlying properties that are used for the calculated property, the changes will be automatically reflected in the result of accessing the calculated property:

```swift
let mark = Student(username: "markedwardmurray", firstName: "Mark", lastName: "Murray")
        
print(mark.fullName)

mark.firstName = "Marky Mark"
mark.lastName = "and the Funky Bunch"

print(mark.fullName)
```
This will print:

```
Mark Murray
Marky Mark and the Funky Bunch
```
![](https://curriculum-content.s3.amazonaws.com/swift/swift-calculated-property/Wildside.jpg)

<a href='https://learn.co/lessons/swift-calculated-property' data-visibility='hidden'>View this lesson on Learn.co</a>
