In SwiftUI, NavigationStack and NavigationView are both used to build navigation-based interfaces, but they differ significantly in functionality, architecture, and future support. Here's a comparison to help you understand when and why to use each:

### 🔄 Summary Comparison
| Feature                          | `NavigationView`        | `NavigationStack`                       |
| -------------------------------- | ----------------------- | --------------------------------------- |
| Introduced                       | iOS 13                  | iOS 16                                  |
| Navigation Style                 | Stack (not explicit)    | Explicit Stack                          |
| Supports programmatic navigation | Limited and error-prone | Full support with `NavigationPath`      |
| Supports deep linking            | Difficult               | Built-in and powerful                   |
| Data-driven navigation           | No                      | Yes (`NavigationPath`, codable support) |
| State Restoration                | Manual                  | Automatic with `NavigationPath`         |
| Future Proof                     | Deprecated (soon)       | Recommended moving forward              |

### 📌 NavigationView
Use case: Legacy projects targeting iOS 13–15.

```swift
NavigationView {
    VStack {
        NavigationLink("Go to Detail", destination: Text("Detail View"))
    }
    .navigationTitle("Home")
}
```

### Limitations:

Hard to manage programmatic navigation.
No native support for navigation state restoration.
Deep linking requires hacks.
Behaves differently on iPad/Mac due to automatic split view handling.

### 🚀 NavigationStack
Use case: All new development targeting iOS 16+.

```swift
NavigationStack {
    List(items) { item in
        NavigationLink(item.name, value: item)
    }
    .navigationDestination(for: Item.self) { item in
        Text("Details for \(item.name)")
    }
}
```

### Advantages:

NavigationPath: Allows pushing/popping views using values or types.
Programmatic navigation: Push/pop views dynamically.
Better control: View hierarchy and transitions are clear.
Backed by value types: Easier state management and unit testing.
Supports Codable types for deep linking and state restoration.

### ✅ When to Use What
iOS 16+ app? Use NavigationStack.

Supporting iOS 13–15? Stick with NavigationView, but consider building your navigation layer to transition later.

Need programmatic navigation, deep linking, or path-based control? Definitely go with NavigationStack.

### 🧠 Pro Tip
You can migrate from NavigationView to NavigationStack incrementally if you're starting to drop support for older iOS versions. Wrap newer features behind #available(iOS 16, *).



In SwiftUI's NavigationStack (introduced in iOS 16), you can push, pop, present, and dismiss views using NavigationPath and SwiftUI’s new presentation modifiers.

Here's how to do each operation:

### 🧭 1. Pushing Views (Programmatic Navigation)

```swift
struct ContentView: View {
    @State private var path = NavigationPath()
    
    var body: some View {
        NavigationStack(path: $path) {
            VStack {
                Button("Go to Detail") {
                    path.append("detail") // or any Hashable type
                }
            }
            .navigationDestination(for: String.self) { value in
                if value == "detail" {
                    DetailView()
                }
            }
            .navigationTitle("Home")
        }
    }
}
```

### 🔙 2. Popping Views
Pop one view: Use .removeLast() on the NavigationPath
Pop to root: Use .removeLast(path.count) or .removeAll()

```swift
Button("Back") {
    path.removeLast()
}

Button("Pop to root") {
    path.removeAll()
}
```

### 🎭 3. Presenting a Modal (Sheet / FullScreenCover)

```swift
struct ContentView: View {
    @State private var showModal = false

    var body: some View {
        NavigationStack {
            Button("Present Modal") {
                showModal = true
            }
            .sheet(isPresented: $showModal) {
                ModalView()
            }
        }
    }
}
```

You can also use .fullScreenCover(isPresented:) if needed.

### ❌ 4. Dismissing a Presented Modal
Use the @Environment(\.dismiss) property inside the presented view.

```swift
struct ModalView: View {
    @Environment(\.dismiss) private var dismiss

    var body: some View {
        VStack {
            Text("This is a modal")
            Button("Dismiss") {
                dismiss()
            }
        }
    }
}
```

### ✅ Summary Cheat Sheet

| Action            | Method                                       |
| ----------------- | -------------------------------------------- |
| **Push**          | `path.append(item)`                          |
| **Pop one**       | `path.removeLast()`                          |
| **Pop to root**   | `path.removeAll()`                           |
| **Present modal** | `.sheet(isPresented:)` or `.fullScreenCover` |
| **Dismiss modal** | `@Environment(\.dismiss).wrappedValue()`     |


