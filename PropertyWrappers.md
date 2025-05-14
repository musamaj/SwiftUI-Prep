### 🧾 What Are Property Wrappers in Swift?
Property wrappers in Swift are a feature that allows you to define custom logic for getting and setting a property’s value, without repeating that logic for each property.

They’re essentially a way to encapsulate behavior (e.g. validation, transformation, caching) that you'd otherwise write in every property's didSet/willSet or computed property.

### 🔧 Basic Syntax
You define a property wrapper by creating a struct (or class) annotated with @propertyWrapper:

```swift
@propertyWrapper
struct Capitalized {
    private var value: String = ""

    var wrappedValue: String {
        get { value }
        set { value = newValue.capitalized }
    }

    init(wrappedValue: String) {
        self.wrappedValue = wrappedValue
    }
}
```

Now you can use @Capitalized on any property:

```swift
struct User {
    @Capitalized var name: String
}

var user = User(name: "john doe")
print(user.name) // Output: "John Doe"

```

### 🧰 Common Use Cases
UserDefaults binding

```swift
@propertyWrapper
struct UserDefault<T> {
    let key: String
    let defaultValue: T

    var wrappedValue: T {
        get { UserDefaults.standard.object(forKey: key) as? T ?? defaultValue }
        set { UserDefaults.standard.set(newValue, forKey: key) }
    }
}

struct Settings {
    @UserDefault(key: "isDarkMode", defaultValue: false)
    static var isDarkMode: Bool
}
```

### Value clamping / validation

```swift
@propertyWrapper
struct Clamped<Value: Comparable> {
    var value: Value
    let range: ClosedRange<Value>

    var wrappedValue: Value {
        get { value }
        set { value = min(max(newValue, range.lowerBound), range.upperBound) }
    }

    init(wrappedValue: Value, _ range: ClosedRange<Value>) {
        self.range = range
        self.value = min(max(wrappedValue, range.lowerBound), range.upperBound)
    }
}

struct Volume {
    @Clamped(0...100) var level: Int = 50
}
```

### 🧠 Notes
You can access the wrapper instance itself via $propertyName.

You can define projected values using projectedValue to provide extra behavior.

var projectedValue: SomeType {
    // accessed with $propertyName
}


### 🔍 Summary
Feature	Purpose
@propertyWrapper	Marks a struct/class as a wrapper
wrappedValue	Logic to get/set the underlying value
projectedValue	Optional — used when accessing $property
Use case	DRY principle for repetitive logic around property behavior
