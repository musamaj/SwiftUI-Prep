In iOS development (and more broadly, in Swift and Objective-C), strong and weak refer to how references to objects are managed in memory—specifically in terms of ownership and ARC (Automatic Reference Counting). Understanding them is crucial to avoid retain cycles and memory leaks.

### 🔹 strong (Default Reference Type)
Ownership: Keeps a strong hold on the object.

Memory Impact: The object will not be deallocated as long as there is at least one strong reference pointing to it.

Use Case: Most commonly used when you want to own the object (e.g., properties in a typical model or view controller).

```swift
class Person {
    var name: String
    init(name: String) {
        self.name = name
    }
}

var person1: Person? = Person(name: "Alice")
var person2 = person1  // strong by default
person1 = nil          // person2 still holds a strong reference
```

### 🔸 weak
Ownership: Does not keep a strong hold on the object.

Memory Impact: The object can be deallocated even if weak references to it exist.

Always declared as optional (since it can become nil automatically).

Use Case: Commonly used to break retain cycles, especially in closures, delegates, and child-to-parent references.

```swift
class Person {
    var name: String
    weak var friend: Person?  // weak reference to avoid retain cycle
    init(name: String) {
        self.name = name
    }
}
```

### 🔁 Example: Avoiding a Retain Cycle

```swift
class ViewModel {
    var onUpdate: (() -> Void)?
}

class ViewController {
    let viewModel = ViewModel()

    func setup() {
        viewModel.onUpdate = { [weak self] in
            self?.updateUI()
        }
    }

    func updateUI() {
        // Update UI here
    }
}
```

Without [weak self], ViewController would strongly capture the closure, and the closure would strongly capture self, causing a retain cycle.

### 🧠 Summary
Keyword	Keeps object alive?	Can become nil automatically?	Common use cases
strong	✅ Yes	❌ No	Default references, ownership
weak	❌ No	✅ Yes	Delegates, closures, parent-child references
