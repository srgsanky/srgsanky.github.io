---
layout: default
title:  "Swift programming language"
date:   2016-11-24
categories: ios swift
---
* TOC
{:toc}

# xcode keyboard shortcuts

* ``⌥ Click`` - View documentation of a method
* ``⌃ a k k`` - to delete a line

# Buzz words
Automatic Reference Counting (ARC)
Protocol - similar to interface concept

## Basics

* constant - ``let``
* variable - ``var``

type if provided explicitly is written after the variable name separated by a colon

```swift
let height: Double = 70
```

There is no implicit type conversion. Type conversion must be done explicitly.

To include values in a string use the syntax
``\(value)``

### Arrays
Create arrays and dictionaries using brackets ``[]``. Access their elements by writing the index or key in the brackets.

To create an empty array, use the initializer syntax

```swift
let emptyArray = [String]()

let emptyArray = Array<String>()

let animals = ["Giraffe", "Crow", "Doggie", "Bird"]
```

How to enumerate an Array?

```swift
for animal in animals {
  println("\(animal)")
}
```

How to filter undesirables from the array?

```swift
let bigNumbers = [2, 47, 118, 5, 9].filter({$0 > 20})
```
Anything that returns ``true`` from the predicate will be retained. Anything that returns ``false`` will be filtered out.

How to map/transform each element of an array to something different?

```swift
let stringified: [String] = [1, 2, 3].map({String($0)})
```

How to reduce an entire array to a single value?

```swift
let sum: Int = [1, 2, 3].reduce(0, {$0 + $1})
```

### Dictionaries
To create a dictionary

```swift
let emptyDictionary = [String: Float]()

let emptyDictionary = Dictionary<String, Float>()
```

When you try to get the value of a key from the ``Dictionary``, it returns an ``Optional`` since they value may be present or may be not.

How to enumerate a Dictionary?

```swift
for (key, value) in someDictionary {
  print("\(key) = \(value)")
}
```

### String

How to get the characters in a string?

```swift

```

Characters in a String are represented using ``String.CharacterView``. They are not exactly ``[Character]`` (Character array) but behave similar to one.

```
startIndex -> String.Index
endIndex -> String.Index
hasPrefix(String) -> Bool
hasSuffix(String) -> Bool
capitalizedString -> String
lowercaseString -> String
uppercaseString -> String
componentsSeparatedByString(String) -> [String] // Similar to split in Java
```

### NSObject
Base class for all Objective-C classes
You need to subclass ``NSObject`` to use some advanced features.

### NSNumber
Used to wrap primitive numbers like ``int`` and ``double`` in Objective-C to an object. This object can then be stored in an Array or other collections that can only hold objects.

### NSDate
Find out the date and time right now or to store past or future dates.

Related - ``NSCalendar``, ``NSDateFormatter``, ``NSDateComponents``.

### NSData
A "bag o' bits". Used to save/restore/transmit raw data throughout the iOS SDK.


### Tuples
It is a grouping of values and  can be used anywhere a type can be used.

```swift
let x: (String, Int, Double) = ("Hello", 5, 0.85)
let (word, number, value) = x
print(word) // prints Hello
print(number) // prints 5
print(value) // prints 0.85
```

or

```swift
let x: (w: String, i: Int, v:Double) = ("Hello", 5, 0.85)
print(x.w) // prints Hello
print(x.i) // prints 5
print(x.v) // prints 0.85
```

### Ranges

Inclusive

```swift
let array = ["a", "b", "c", "d"]
let subArray1 = array[2...3] // ["c", "d"]
```

Exclusive

```swift
let array = ["a", "b", "c", "d"]
let subArray2 = array[2..<3] // ["c"]
```

Ranges in enumeratable like Array, String, Dictionary

```swift
for i in 27...104 {
  
}
```

### AnyObject
``AnyObject`` is a special type (a protocol).

Used to be commonly used for compatibility with old Objective-C APIs but not anymore in iOS9 since those old Objective-C APIs have been updated.

1. A variable of type ``AnyObject`` can point to any **Class** (not a struct or enum), but you don't know which.
1. Type ``Any`` can be used to hold anything but is rarely used.

The following is an example usage of ``AnyObject``

```swift
func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject)
```

How to use ``AnyObject``?
We must convert it to another, known class or protocol. This conversion might not be possible, so the conversion generates an Optional.

Conversion is done using
``as?`` or ``as!``.

```swift
let ao: AnyObject = ...
if let foo = ao as? SomeClass {
  // we can use foo and know that it is of type SomeClass
}
```

You can check to see if something can be converted with the ``is`` keyword. It returns ``true``/``false``.

#### Property list

It means an ``AnyObject`` which is known to be a collection of objects which are **only** one of ``String``, ``Array``, ``Dictionary``, a number (``Double``, ``Int``, etc), ``NSData``, ``NSDate``.

``NSUserDefaults`` is a storage mechanism for property list data. It is essentially a very tiny database that stores Property List data. It persists between launchings of your application!

```swift
let defaults = NSUserDefaults.standardUserDefaults()
defaults.setObject(plist, forKey: "foo")

if !defaults.synchronize() {
  
}
```

## Control flow
Conditionals:

* if
* switch

Loops:

* for-in
* for
* while
* repeat-while

Braces around the body are required.

### Optionals
An optional value either contains a value or contains ``nil`` to indicate that a value is missing. It is implemented using an enum.

#### Explicitly unwrapped optionals
Write a question mark(``?``) after the type to indicate (explicitly unwrapped) optional.

```swift
var optionalString: String? = "Hello"
```

Combine let with Optionals

```swift
if let myString = optionalString {
  // optionalString is not nil
  // do something with myString
  print(myString.characters.count)
}
```

#### Implicitly unwrapped optionals

```swift
var optionalString: String! = "Hello"
```

You don't have to unwrap implicitly unwrapped optionals. They will be unwrapped by the language for you. Use it when you are sure that the Optional won't be ``nil``. If you try to use an implicitly unwrapped Optional and it is ``nil``, your program will crash.

```swift
print(optionalString.characters.count)
```

You can also unwrap an Optional defined using ``?`` by using ``!``. For example

```swift
var optionalString: String? = "hello"

print(optionalString!.characters.count)
```
Once again, when you use the above format, you have to be sure that the Optional won't be ``nil``.

#### Optional "defaulting" operator
Use a default value when optional is ``nil`` using ``??``

```swift
let informalGreeting = "Hi \(nickName ?? fullName)"
```


### Switch
Switch statements must be exhaustive i.e. you must include have a clause for all possible values. So a default might be needed.
Switch can work with any data type and can do more than equality check. You can even do tuple comparison for e.g.

```swift
switch (yourMove, myMove) {
  case (.rock, .scissors):
    // some statement
}
```

There is no need to explicitly break out of the switch at the end of each case's code.


### Loops

for-in

You can keep an index in the loop. Use ``..<`` or ``...``

```swift
var total = 0

for i in 0..<4 {
  total += i
}

print(total)
```

while

repeat-while


## Functions and closures

### Functions
Functions are declared using ``func``

```swift
func greet(preson: String, day: String) -> String {
  return "Hello \(person), today is \(day)."
}

greet(person: "Bob", day: "Tuesday")
```

Returning compound values using tuples

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
  
}
```

Functions can take variable number of arguments

```swift
func sumOf(numbers: Int...) -> Int {
  
}
```

Functions can be nested.

Functions are a first-class type. So, one function can return another function and take another function as its argument.

```swift
func makeIncrementer() -> ((Int) -> Int) {
  
}
```

#### Internal vs external parameter names

All parameters can have an ``internal`` name and an ``external`` name.

```swift
func foo(externalFirst first: Int, externalSecond second: Double) {
  var sum = 0.0
  for _ in 0..<first {sum += second}
}

foo (externalFirst: 5, externalSecond: 5.5)
```

You can put ``_`` if you don't want callers to use an external name at all for a given parameter.

```swift
func foo(_ first: Int, externalSecond second: Double) {
  var sum = 0.0
  for _ in 0..<first {sum += second}
}

foo (5, externalSecond: 5.5)
```

Swift 3 makes all external names required unless you specify otherwise (using ``_``).

#### Overrides

You can override methods/properties in your superclass using the keyword ``override``.

#### Finals
You can mark a method/property as ``final`` which prevents subclasses from being able to override them. Classes can also be marked as ``final``.

#### Type vs Instance methods/properties
  Type methods/properties are similar to ``static`` in Java - they belong to a type rather than a specific instance of the type. You declare a type method or property with a ``static`` prefix.
  
```swift
static func abs(d: Double) -> Double {
  
}
```

### Closures
block of code that can be called later. The code in a closure has access to things like variables and functions that were in the scope where the closure was created, even if the closure is in a different scope when it is executed.
Functions are a special case of closures.

You can write a closure without a name by surrounding code with braces ({})
Use ``in`` keyword to separate the return type from the body of the closure.
  
Single statement closures implicitly return the value of their only statement.

You can refer to parameters by numbers instead of names (similar to bash syntax)

When a closure is the only argument to a function, you can omit the parentheses.

```swift
let sortedNumbers = numbers.sorted { $0 < $1}
```

## Objects and Classes
Use ``class`` followed by the class name to create a class. Properties are created using ``var`` or ``let``.
Methods are created using ``func``.

To create an instance of the class, use parentheses after the class name. Use dot syntax to access properties and methods.

### initializer and deinitializer
Use ``init`` to create an initializer. Initializer can be ``designated`` or ``convenience``. See [Designated and convenience initializer](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html)

Use ``self`` to access the current object.

Every property must be assigned a value - either in the declaration or in the initializer. If a property is not assigned a value, it must be declared as optional. Properties can also be initialized "lazily" or using a closure.

You get "free" init when all the properties have defaults. You also get "free" init with a struct with all properties as arguments.

1. You can call on other ``init`` method in your class using ``self.init``
1. You can call ``super.init``

What are you **required** to do inside ``init``?
1. By the time any ``init`` is done, all properties must have values (optionals can have the value ``nil``)
1. **convenience** initializers and **designated** initializers. Convenience initializers are marked with the keyword ``convenience``.
1. A **designated** init must (and can only) call a designated init that is in its immediate superclass.
1. You must initialize all properties introduced by your class before calling a superclass's ``init``. (Note: this is different from Java. In Java, you call the superclass constructor before you work with the properties introduced by your class)
1. You must call superclass's init before you assign a value to an inherited property
1. A ``convenience`` init must call (and can only) call an ``init`` in its own class.
1. A ``convenience`` init must call that ``init`` before it can set any property values. It is just for convenience - you will be resetting values set by your class's designated ``init`` and the superclass's ``init``.

Inheriting inits

1. If you do not implement any designated inits, you will inherit all of your superclass's designated inits.
1. If you override all of your superclass's designated inits, you'll inherit all of its convenience inits.
1. If you implement no inits, you'll inherit all of your superclass's inits.

Required init

1. You can mark an ``init`` as ``required``.
1. Any subclass **implement** ``init``s marked as ``required``.

Failable init

If an ``init`` is declared with a ``?`` (or ``!``) after the word ``init``, it returns an Optional.

```swift
init? (arg1: Type1, ...) {
  // might return nil in here
}
```
For e.g. Double(String) -> Optional<Double>

Use ``deinit`` to create a deinitializer in order to perform any cleanup before the object is deallocated.

### inheritance
Subclasses include their superclass name after their class name, separated by a colon.

```swift
class Square: NamedShape {
  
}
```

Methods that override the superclass's implementation are marked with ``override``. Overriding a method by accident without ``override`` is flagged as an error by the compiler.

```swift
class Square: NamedShape {
  var sideLength: Double = 4
  
  override func simpleDescription() -> String {
    return "A square with sides of length \(sideLength)"
  }
}
```

### Computed properties
You can provide optional setters and getters for a property. These are called **computed properties** because they are computed on the fly as opposed to **stored properties**.

```swift
var length: Double {
  get {
    return length * 10
  }
  set {
    length = newValue / 10
  }
}
```

### properties

* stored properties - must be set when an instance is initialized
* type properties - these are class properties (like static in Java). It has the same value for every instance of the class.

``` swift
static let premittedRatings = ["G", "PG"]
```
* computed properties - a property that is computed based on other fields of the class

#### willSet and didSet
You can observe changes to any property with ``willSet`` and ``didSet``

```swift
var someStoredProperty: Int = 42 {
  willSet {
    // Code executed before someStoredProperty is set to the value newValue
  }
  didSet {
    // Code executed after someStoredProperty is set to the newValue. oldValue refers to the previous value.
  }
}
```

#### Lazy Initialization
A ``lazy`` property does not get initialized until someone accesses it.

You can allocate an object, execute a closure or call a method.

```swift
lazy var brain = CalculatorBrain()

lazy var someProperty: Type = {
  return <the constructed value>
}()

lazy var myProperty = self.initializeMyProperty()
```

The lazy initializations still satisfies the "you must initialize all of your properties" rule. Lazy cannot be applied to constants (``let``).

### Optionals
You can write ``?`` before

1. methods
```
optionalSquare?.getName()
```
2. properties
```
optionalSquare?.sideLength
```
3. subscripting

## Enumerations & structures

### Enumerations
Use ``enum`` to create an enumeration. Enumerations can have methods associated with them.

Enums have ``rawValue`` assigned. They start at zero by default. Enums can use raw type of Int, String or Float.

```swift
enum Finger: String {
  case index = "index"
  case middle = "middle"
  case ring = "ring"
  case pinky = "pinky"
  case thumb = "thumb"
}
```

You can omit the raw type and raw value when you want the case values of the enumeration to be actual values.

You can also have values associated with the case. This value will be determined when you make the instance and can be different for each instance of an enumeration case.

### Structures
Use ``struct`` to create a structure. The main difference between structures and classes is that structures are copied when they are passed around in code, but classes are passed by reference.

```swift
struct Card {
  var rank: rank
  var suit: Suit
}
```

Structs come with member-wise initializers by default. So you don't have to provide one.

## Classes, Structs, Enums

### Similarities

|                          | Classes                                          | Struct                                            | Enum                                                          |
|--------------------------|--------------------------------------------------|---------------------------------------------------|---------------------------------------------------------------|
| Declaration syntax       | ``class MyClass {}``                             | ``struct MyStruct {}``                            | ``enum MyEnum {}``                                            |
| Properties and functions | Stored properties. Computed properties. Methods. |  Stored properties. Computed properties. Methods. |  Computed properties. Methods. Cannot have stored properties. |
| Initializers             | Yes                                              | Yes (also has default initializers)               | No                                                            |


### Differences

|                                | Classes        | Struct     | Enum       |
|--------------------------------|----------------|------------|------------|
| Inheritance                    | Yes            | No         | No         |
| Value types vs Reference types | Reference type | Value type | Value type |

#### Value types
1. Copied when passed as an argument to a function
1. Copied when assigned to a different variable
1. Becomes immutable if assigned to a ``let`` variable.
1. Function parameters are constants. You cannot modify them.
1. You must note any ``method`` in a struct/enum that can mutate its struct/enum with the keyword ``mutating``. You cannot use ``mutating`` on a ``func`` outside a type.

#### Reference types
1. Stored in the heap and reference counted (automatically). There is no garbage collection - predictable removal of objects from heap when reference count goes to zero.
1. Constant pointers to a class (``let``) still can mutate by calling methods and changing properties.
1. When passed as an argument, does not make a copy.

## Protocols & Extensions

### Protocols
Protocols are similar to interfaces in Java.
Use ``protocol`` to declare a protocol. Classes, enumeration and structs can all adopt protocols.

```swift
protocol ExampleProtocol {
  var simpleDescription: String {get}
  mutating func adjust()
}
```

### Extensions
Use ``extensions`` to add functionality to an existing type. You can use it to add protocol conformance of a type (as an after thought).

```swift
extension Int {
  var anotherField: Int = 10
}
```

## Error handling

Represent errors using any type that adopts the ``Error`` protocol.
Use ``throw`` to throw an error and ``throws`` to mark a function that can throw an error.
The following can be used to handle errors:

* do-catch
* try?

```swift
do {
  let printerResponse = try send(job:1440, toPrinter: "Gutenberg")
  print(printerResponse)
} catch {
  print(error)
}
```

You can provide multiple catch blocks that handle specific errors.

``try?`` converts the result to an optional. If the function throws an error, the specific error is discarded and the result is nil. Otherwise, the result is an optional containing the value that the function returned.

## Generics
Write a name inside angle brackets to make a generic function or type.

Use ``where`` before the body to specify a list of requirements for the Generic types.


## Collections

* arrays
* set
* dictionary

# Presentation
Navigation views (messages app)
Modal views (Open image from photo library). Alert Modal





# Misc

## Math PI
PI is defined in the various number types itself. So, you can access PI using

```swift
Double.pi
Float.pi
CGFloat.pi
```

Swift can infer the type from the context, so you can even omit the type. For e.g.

```swift
func circumferenceOfCircle(radius: Double) -> Double {
  return 2 * .pi * radius;
}
```

## Find the type of a variable
To find the type of a variable use ``type``

```swift
type(of: variable)
```

## UIViewController
Properties of UIViewController

* parent
* tabBarController

## Size of array
Use ``count`` property to find the size of an array

## Use ~= to compare number with ranges
To see if a number is within a range, use ``~=``

## Access Control

* internal (similar to protected in Java). The world cannot access it
* public
* private 

## Documentation

option-click on a type to show help.

Quick help in the right side 
View -> Utilities -> Show Quick Help Inspector

Window -> Documentation and API Reference

## Assertions
Assertions can be used to intentionally crash your program if some condition is not true. They are used as debugging aids.

```swift
assert(() -> Bool, "message")
```

# Keywords

## mutating
``mutating`` keyword is used to mark a method that modifies the structure.

## internal

## defer
Use ``defer`` to write a block of code that is executed before the function returns. The code is executed regardless of whether the function throws an error.

## as
``as?`` and ``as!`` are type cast operator

## class
Use it before a func to mark it as a static function which the subclass can override

## static
Use it to mark a func or property as static

## reduce - unknown syntax

```swift
reviewScores.reduce(0) { $0 + $1 } / Double(reviewScores.count)
```

## Operator overloading
Operator are global function. You write it just like a function

```swift
func == (lhs: Roommate, rhs: Roommate) -> Bool {
  return lhs.name == rhs.name && lhs.hungry == rhs.hungry
}
```

## convenience
convenience initializer - just for the sake of convenience.


# References

* Global functions http://swiftdoc.org/v3.0/func/