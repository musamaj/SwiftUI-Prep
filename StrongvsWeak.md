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


### ✅ Structs don't increase reference count, but they can still impact memory usage.
Here’s how:

### 🔹 Structs = Value Types
No ARC involved: So, no reference count is increased when passing/assigning.

But each copy of a struct occupies memory.

🟡 So even though reference count isn’t used, memory usage can increase depending on the size of the struct and how often it's copied.

🔍 Example: Lightweight vs Heavyweight Struct
struct SmallStruct {
    var x: Int
    var y: Int
}

struct BigStruct {
    var data: [Int] // Reference-type under the hood, CoW
}
SmallStruct copies cheaply.

BigStruct seems large, but Swift optimizes it using copy-on-write (CoW) — so memory only increases when it’s mutated.

### 🧠 Copy-on-Write (CoW) Optimization
Swift uses CoW for types like Array, String, Data, and even your custom structs if you use reference types inside.

So:
var a = [1, 2, 3]
var b = a        // No new memory used yet

b.append(4)      // Now memory is copied
Until b mutates, a and b share the same buffer.

### 🧾 Summary
Aspect	Struct (Value Type)	Class (Reference Type)
ARC / Ref Count	❌ No	✅ Yes
Memory Usage on Assign	✅ Copies (can increase)	❌ Shares (ref count only)
Mutation Impact	🟡 CoW (efficient)	✅ Mutates shared instance
Performance	✅ Faster, safer	🟡 Can be slower, flexible

### 🟢 Bottom Line:
Structs don’t increase reference count, but they can increase memory usage depending on how you copy and use them. Fortunately, Swift helps you with optimizations like CoW in most real-world scenarios.

Let me know if you want a memory visualization or comparison of class vs struct in Instruments profiler.
