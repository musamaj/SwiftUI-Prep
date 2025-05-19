### 🧩 What is PassthroughSubject in Combine?
In Apple’s Combine framework, PassthroughSubject is a type of subject that allows manual publishing of values to downstream subscribers — think of it as a bridge between imperative code and the declarative Combine pipeline.

### ✅ Quick Summary
PassthroughSubject is a publisher.

It doesn’t hold any value, unlike CurrentValueSubject.

You can send values manually via .send(_:).

It's useful for emitting events (like user actions, network updates, etc.).

### 🧠 Syntax & Usage

```swift
import Combine

let subject = PassthroughSubject<String, Never>()

let subscription = subject.sink { value in
    print("Received value: \(value)")
}

subject.send("Hello")
subject.send("World")
```

### 🏗️ Key Components
```swift
PassthroughSubject<Output, Failure>
```

Output: The type of data emitted (e.g. String, Int, etc.).
Failure: The type of error it can emit (use Never if it won’t fail).

### 🧭 When to Use It
Use Case	Description 
🔁 User events	Emit button taps, gestures
📡 Network callbacks	Bridge non-Combine APIs (like delegate methods)
🚦 Manual control	Trigger actions without automatic state bindings
🧪 Testing	Simulate publisher behavior in unit tests


### 🧩 Example in ViewModel (SwiftUI)
```swift
class MyViewModel: ObservableObject {
    var didTapButton = PassthroughSubject<Void, Never>()
    private var cancellables = Set<AnyCancellable>()

    init() {
        didTapButton
            .sink {
                print("Button was tapped!")
            }
            .store(in: &cancellables)
    }
}

Button("Tap Me") {
    viewModel.didTapButton.send()
}
```

### 🔄 PassthroughSubject vs CurrentValueSubject
Feature	PassthroughSubject	CurrentValueSubject
Stores latest value?	❌ No	✅ Yes
Emits to late subscribers?	❌ No	✅ Yes (latest value)
Typical use	Events, actions	State propagation
