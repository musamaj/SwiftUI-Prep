Here's a simple explanation of the Redux pattern—especially useful if you're coming from an iOS/Swift background and preparing for interviews.

### 🧠 What is Redux?
Redux is a state management pattern originally from JavaScript (React), but its ideas have been adopted in many platforms, including iOS.

At its core, Redux is about centralizing all your app’s state in one place and updating it in a predictable way.

### 🧩 Key Concepts
### 1. Store
This holds the entire state of your app.

```swift
struct AppState {
    var counter: Int
}
```

### 2. Actions
These are just descriptions of what happened.

```swift
enum CounterAction {
    case increment
    case decrement
}
```

### 3. Reducer
A pure function that takes the current state and an action, and returns a new state.

```swift
func counterReducer(state: AppState, action: CounterAction) -> AppState {
    var newState = state
    switch action {
    case .increment:
        newState.counter += 1
    case .decrement:
        newState.counter -= 1
    }
    return newState
}
```

### 4. Dispatcher
This sends actions to the reducer and updates the store.

### ⚙️ Redux Flow
```sql
User Action → Dispatch → Reducer → New State → UI Update
```

So imagine a button press:

User taps “+”
Dispatch .increment action
Reducer handles it, updates state
UI reacts to new state (e.g. @Published or @StateObject)

### 🔄 Redux in SwiftUI (Example)
Using Combine:

```swift
class Store: ObservableObject {
    @Published private(set) var state: AppState
    private let reducer: (AppState, CounterAction) -> AppState

    init(initialState: AppState, reducer: @escaping (AppState, CounterAction) -> AppState) {
        self.state = initialState
        self.reducer = reducer
    }

    func dispatch(action: CounterAction) {
        state = reducer(state, action)
    }
}
```

### Usage in SwiftUI:

```swift
struct ContentView: View {
    @ObservedObject var store: Store

    var body: some View {
        VStack {
            Text("Count: \(store.state.counter)")
            HStack {
                Button("-") { store.dispatch(action: .decrement) }
                Button("+") { store.dispatch(action: .increment) }
            }
        }
    }
}
```

### ✅ Why Use Redux?

Predictable state updates (no surprises)
Single source of truth
Easier to debug & test
Scales well in complex apps
