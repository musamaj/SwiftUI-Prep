# 🚀 SwiftUI Performance Optimization: Avoiding Unnecessary Re-renders

SwiftUI efficiently updates views when state changes, but unnecessary re-renders can degrade performance. This guide explores best practices for optimizing SwiftUI apps, focusing on `@StateObject` vs. `@ObservedObject`.

---

## 📌 When Does SwiftUI Re-render?

A **view re-renders** when:
✅ `@State`, `@Binding`, or `@EnvironmentObject` changes  
✅ `@Published` property in `@ObservedObject` or `@StateObject` updates  
✅ The entire parent view updates (causing unnecessary child updates)  

**❌ Problem: Too Many Re-renders**  
- Re-initializing objects incorrectly can cause unwanted re-renders.
- Parent state changes may unnecessarily affect child views.

---

## 🔹 `@StateObject` vs `@ObservedObject`

| Property Wrapper  | When to Use | Re-initialization? | Who Owns the Object? |
|------------------|------------|-------------------|----------------------|
| `@StateObject`  | When **creating** the object inside the view | ❌ No (Persists) | The **view** owns it |
| `@ObservedObject` | When **passing** an object from parent to child | ✅ Yes (Re-initializes) | The **parent** owns it |

---

## 🔥 Optimized Usage: `@StateObject` vs `@ObservedObject`

### ❌ **Inefficient: Using `@ObservedObject` in Parent View**
```swift
class CounterViewModel: ObservableObject {
    @Published var count = 0
}

struct ParentView: View {
    var body: some View {
        ChildView(viewModel: CounterViewModel()) // ❌ New instance every render!
    }
}

struct ChildView: View {
    @ObservedObject var viewModel: CounterViewModel
    
    var body: some View {
        VStack {
            Text("\(viewModel.count)")
            Button("Increment") { viewModel.count += 1 }
        }
    }
}
```
**❌ Issue:** Each time `ParentView` re-renders, `CounterViewModel` is re-initialized, resetting `count`.

---

### ✅ **Optimized: Using `@StateObject` in Parent View**
```swift
struct ParentView: View {
    @StateObject private var viewModel = CounterViewModel()  // ✅ Persisted instance

    var body: some View {
        ChildView(viewModel: viewModel)
    }
}
```
✅ `@StateObject` ensures the instance **persists** across re-renders.

---

## 📌 Avoiding Re-renders with `EquatableView`

```swift
struct UserView: View, Equatable {
    let name: String

    var body: some View {
        Text(name)
    }

    static func == (lhs: UserView, rhs: UserView) -> Bool {
        lhs.name == rhs.name  // ✅ View only updates if `name` changes
    }
}
```
✅ **Prevents re-rendering** unless `name` actually changes.

---

## 🔹 Optimizing Lists with `@StateObject`

### ❌ **Inefficient: Entire List Re-renders**
```swift
struct ContentView: View {
    @ObservedObject var viewModel = UserListViewModel()

    var body: some View {
        List(viewModel.users) { user in
            UserRow(user: user)  // ❌ Full list re-renders
        }
    }
}
```

### ✅ **Optimized: `@StateObject` Per Row**
```swift
struct UserRow: View {
    @StateObject var user: UserModel  // ✅ Each row manages its state
    var body: some View {
        Text(user.name)
    }
}
```
✅ **Only the modified row updates, not the entire list**.

---

## 📌 Using `@EnvironmentObject` for Global State

```swift
class AppSettings: ObservableObject {
    @Published var isDarkMode = false
}

struct ParentView: View {
    @StateObject private var settings = AppSettings()

    var body: some View {
        ChildView()
            .environmentObject(settings)  // ✅ Inject globally
    }
}

struct ChildView: View {
    @EnvironmentObject var settings: AppSettings  // ✅ Accessible anywhere
    var body: some View {
        Toggle("Dark Mode", isOn: $settings.isDarkMode)
    }
}
```
✅ Prevents unnecessary re-renders caused by manual state passing.

---

## 🚀 Summary: Best Practices to Avoid Re-renders

| Optimization | Benefit |
|-------------|---------|
| ✅ Use `@StateObject` for objects **owned by a view** | Prevents re-initialization |
| ✅ Use `@ObservedObject` for objects **passed from parent** | Ensures state consistency |
| ✅ Use `EquatableView` for expensive view updates | Prevents unnecessary UI updates |
| ✅ Use `@EnvironmentObject` for global state | Reduces parameter passing |
| ✅ Optimize lists with `@StateObject` per row | Avoids full list re-renders |

---

## 🚀 Final Thoughts
Optimizing SwiftUI performance requires **controlling state changes** and **minimizing unnecessary view updates**. Proper use of `@StateObject`, `@ObservedObject`, and `EquatableView` will improve efficiency and responsiveness.

---

### 💡 Want to contribute?
Feel free to open an issue or submit a pull request if you have improvements or optimizations to share! 🚀

