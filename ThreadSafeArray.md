To make an Array thread-safe in Swift, you need to synchronize access to it — both reads and writes — since Swift collections are not thread-safe by default.

Here are the most common and effective approaches:

### ✅ 1. Using a Serial DispatchQueue (Recommended)

```swift
final class ThreadSafeArray<Element> {
    private var array: [Element] = []
    private let queue = DispatchQueue(label: "com.example.ThreadSafeArray")

    func append(_ item: Element) {
        queue.async {
            self.array.append(item)
        }
    }

    func get(at index: Int) -> Element? {
        queue.sync {
            guard array.indices.contains(index) else { return nil }
            return array[index]
        }
    }

    var count: Int {
        queue.sync {
            array.count
        }
    }

    func getAll() -> [Element] {
        queue.sync {
            array
        }
    }
}
```

### ✅ Pros:
Safe and easy to use

No data races

Efficient for most use cases

### ✅ 2. Using a Concurrent Queue + Barrier
Allows concurrent reads and serializes writes (better for read-heavy workloads).

```swift
final class ThreadSafeArray<Element> {
    private var array: [Element] = []
    private let queue = DispatchQueue(label: "com.example.ThreadSafeArray", attributes: .concurrent)

    func append(_ item: Element) {
        queue.async(flags: .barrier) {
            self.array.append(item)
        }
    }

    func get(at index: Int) -> Element? {
        queue.sync {
            guard array.indices.contains(index) else { return nil }
            return array[index]
        }
    }

    var count: Int {
        queue.sync {
            array.count
        }
    }
}
```

### ✅ Pros:
High concurrency performance

Thread-safe reads + writes

### ⚠️ Cons:
Slightly more complex

Always use .barrier for writes!

### ❌ What NOT to do
Avoid DispatchSemaphore, NSLock, or objc_sync_enter unless you have a very specific reason. They are more error-prone and lower-level compared to GCD.

### 🧠 Tip: For SwiftUI or Combine Apps
Use @MainActor or @Published and ensure UI state is modified only on the main thread.

