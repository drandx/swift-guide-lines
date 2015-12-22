##Code Guidelines
###Naming
Use descriptive names with camel case for classes, methods, variables, etc. Class names should be capitalized, while method names and variables should start with a lower case letter.
#####Prefered:
```Swift
private let maximumWidgetCount = 100

class WidgetContainer {
  var widgetButton: UIButton
  let widgetHeightPercentage = 0.85
}
```
#####Not Predered:
```Swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
  var wBut: UIButton
  let wHeightPct = 0.85
}
```
###Enumerations
Use UpperCamelCase for enumeration values:
```Swift
enum Shape {
  case Rectangle
  case Square
  case Triangle
  case Circle
}
```
####Spaces
* Method braces and other braces (if/else/switch/while etc.) always open on the same line as the statement but close on a new line.
* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu). Some of the Xcode template code will have 4-space tabs hard coded, so this is a good way to fix that.
```Swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```
#####Whitespace
* Tabs, not spaces.
* End files with a newline.
* Make liberal use of vertical whitespace to divide code into logical chunks.
* Don’t leave trailing whitespace.
* Not even leading indentation on blank lines.

###Optionals
####Avoid Using Force-Unwrapping of Optionals
If you have an identifier foo of type `FooType?` or `FooType!`, don't force-unwrap it to get to the underlying value (foo!) if possible.

Instead, prefer this:
```Swift
if let foo = foo {
    // Use unwrapped `foo` value in here
} else {
    // If appropriate, handle the case where the optional is nil
}
```
Alternatively, you might want to use Swift's Optional Chaining in some of these cases, such as:
```Swift
// Call the function if `foo` is not nil. If `foo` is nil, ignore we ever tried to make the call
foo?.callSomethingIfFooIsNotNil()
```
*Rationale:* Explicit if let-binding of optionals results in safer code. Force unwrapping is more prone to lead to runtime crashes.

####Avoid Using Implicitly Unwrapped Optionals
Where possible, use let foo: `FooType?` instead of `let foo: FooType!` if foo may be nil (Note that in general, ? can be used instead of !).

*Rationale:* Explicit optionals result in safer code. Implicitly unwrapped optionals have the potential of crashing at runtime.

###Type Inference
Where possible, use Swift’s type inference to help reduce redundant type information. For example, prefer:
#####Prefered:
```Swift
var currentLocation = Location()
```
#####Not Prefered:
```Swift
var currentLocation: Location = Location()
```
###Self Inference
Let the compiler infer self in all cases where it is able to. Areas where self should be explicitly used includes setting parameters in init, and non-escaping closures. For example:
```Swift
struct Example {
    let name: String

    init(name: String) {
        self.name = name
    }
}
```
##Programming Best Practices
####Prefer structs over classes
Unless you require functionality that can only be provided by a class (like identity or deinitializers), implement a struct instead.

Note that inheritance is (by itself) usually not a good reason to use classes, because polymorphism can be provided by protocols, and implementation reuse can be provided through composition.

For example, this class hierarchy:
```Swift
class Vehicle {
    let numberOfWheels: Int

    init(numberOfWheels: Int) {
        self.numberOfWheels = numberOfWheels
    }

    func maximumTotalTirePressure(pressurePerWheel: Float) -> Float {
        return pressurePerWheel * Float(numberOfWheels)
    }
}

class Bicycle: Vehicle {
    init() {
        super.init(numberOfWheels: 2)
    }
}

class Car: Vehicle {
    init() {
        super.init(numberOfWheels: 4)
    }
}
```
could be refactored into these definitions:
```Swift
protocol Vehicle {
    var numberOfWheels: Int { get }
}

func maximumTotalTirePressure(vehicle: Vehicle, pressurePerWheel: Float) -> Float {
    return pressurePerWheel * Float(vehicle.numberOfWheels)
}

struct Bicycle: Vehicle {
    let numberOfWheels = 2
}

struct Car: Vehicle {
    let numberOfWheels = 4
}
```
*Rationale:* Value types are simpler, easier to reason about, and behave as expected with the let keyword.









