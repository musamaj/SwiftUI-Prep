# State Management in SwiftUI

SwiftUI provides multiple property wrappers to manage state efficiently within an app. Understanding the differences between `@State`, `@Binding`, `@ObservedObject`, and `@EnvironmentObject` is crucial for building reactive UIs.

---

## **1️⃣ @State** – Local State

`@State` is used to store **local mutable state** within a single view. SwiftUI monitors state changes and automatically re-renders the view when the state updates.

### **Example:**
```swift
struct CounterView: View {
    @State private var count = 0  // Local state

    var body: some View {
        VStack {
            Text("Count: \(count)")
                .font(.largeTitle)
            
            Button("Increment") {
                count += 1
            }
        }
    }
}
```
✅ `count` is private to `CounterView`, and changes trigger a re-render.

**Key Points:**
- `@State` **should only be used inside a view**.
- SwiftUI stores `@State` **outside of the view struct** to persist state when the view re-renders.

---

## **2️⃣ @Binding** – Passing State to Child Views

`@Binding` allows child views to **access and modify state from a parent view**.

### **Example:**
```swift
struct ParentView: View {
    @State private var isOn = false

    var body: some View {
        ToggleView(isOn: $isOn) // Pass binding
    }
}

struct ToggleView: View {
    @Binding var isOn: Bool  // Bound state

    var body: some View {
        Toggle("Toggle", isOn: $isOn)
    }
}
```
✅ `ToggleView` can modify `isOn`, and the change reflects in `ParentView`.

**Key Points:**
- `@Binding` is used **in child views** to mutate a parent’s `@State`.
- A **two-way connection** is established between parent and child.

---

## **3️⃣ @ObservedObject** – External Data Model

`@ObservedObject` is used for referencing a **separate model** that conforms to `ObservableObject` and can be shared between multiple views.

### **Example:**
```swift
class CounterModel: ObservableObject {
    @Published var count = 0  // Publishes changes
}

struct CounterView: View {
    @ObservedObject var model = CounterModel()

    var body: some View {
        VStack {
            Text("Count: \(model.count)")
                .font(.largeTitle)
            
            Button("Increment") {
                model.count += 1
            }
        }
    }
}
```
✅ The `CounterModel` instance stores state **outside the view**, ensuring persistence even when the view reloads.

**Key Points:**
- `@ObservedObject` is for **external data models**.
- The model must conform to `ObservableObject` and use `@Published` for properties that change.
- The view **re-renders** when `@Published` properties update.

---

## **4️⃣ @EnvironmentObject** – App-Wide State

`@EnvironmentObject` is used to inject an `ObservableObject` **throughout the app hierarchy** without explicitly passing it.

### **Example:**
```swift
class UserSettings: ObservableObject {
    @Published var username = "Guest"
}

struct ParentView: View {
    @StateObject private var settings = UserSettings()

    var body: some View {
        ChildView().environmentObject(settings)  // Inject
    }
}

struct ChildView: View {
    @EnvironmentObject var settings: UserSettings  // Access global state

    var body: some View {
        Text("Username: \(settings.username)")
    }
}
```
✅ The `UserSettings` instance is **available across views** without manually passing it.

**Key Points:**
- `@EnvironmentObject` is useful for **global state** across many views.
- Must be **injected using `.environmentObject()`**.
- Avoid using it for small, localized state; prefer `@State` or `@ObservedObject` instead.

---

## **Choosing the Right Property Wrapper**
| Property Wrapper   | Scope | Usage |
|--------------------|----------------------------|--------------------------------|
| `@State`          | **Single view** | Local state inside a view |
| `@Binding`        | **Child views** | Passes state from parent to child |
| `@ObservedObject` | **Multiple views** | References an external observable model |
| `@EnvironmentObject` | **App-wide** | Global shared state across the app |

---

## **Conclusion**
State management in SwiftUI is crucial for building dynamic apps. Understanding when to use `@State`, `@Binding`, `@ObservedObject`, and `@EnvironmentObject` helps create clean, efficient, and maintainable code.

🚀 **Use `@State` for local changes, `@Binding` for child views, `@ObservedObject` for external models, and `@EnvironmentObject` for global state.**

