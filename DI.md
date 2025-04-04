# 🧩 Dependency Injection (DI) in SwiftUI

Dependency Injection (DI) is a design pattern used to make components loosely coupled, testable, and modular. SwiftUI supports several DI patterns that help manage dependencies cleanly.

---

## ✅ Why Use DI in SwiftUI?

- Promotes testability
- Avoids tight coupling
- Enables easy mocking
- Improves modular design

---

## 📦 Common DI Methods

| DI Method                   | Description                                       | Best Use Case                        |
|-----------------------------|---------------------------------------------------|--------------------------------------|
| Initializer Injection       | Inject via constructor                           | ViewModels, small apps               |
| `@EnvironmentObject`        | App-wide dependency injection                    | Global/shared state like auth/theme  |
| DI Container (Environment)  | Centralized management via custom EnvironmentKey | Scalable & modular apps              |
| Singleton                   | Global shared access                             | Quick prototyping or legacy support  |

---

## 1️⃣ Initializer Injection

```swift
class MyViewModel: ObservableObject {
    let networkService: NetworkService

    init(networkService: NetworkService) {
        self.networkService = networkService
    }
}
```

### Injecting in View

```swift
struct ContentView: View {
    @StateObject private var viewModel: MyViewModel

    init() {
        _viewModel = StateObject(wrappedValue: MyViewModel(networkService: RealNetworkService()))
    }
}
```

### @EnvironmentObject (App-Wide Shared Instance)

```swift
class UserSession: ObservableObject {
    @Published var isLoggedIn = false
}

@main
struct MyApp: App {
    let session = UserSession()

    var body: some Scene {
        WindowGroup {
            ContentView()
                .environmentObject(session)
        }
    }
}
```

### DI Container with Custom Environment Key
```swift
class DIContainer {
    let networkService: NetworkService
    let analyticsService: AnalyticsService

    init(network: NetworkService, analytics: AnalyticsService) {
        self.networkService = network
        self.analyticsService = analytics
    }
}
```

### Custom Environment Key:
```swift
private struct DIContainerKey: EnvironmentKey {
    static let defaultValue: DIContainer = DIContainer(
        network: RealNetworkService(),
        analytics: FirebaseAnalytics()
    )
}

extension EnvironmentValues {
    var diContainer: DIContainer {
        get { self[DIContainerKey.self] }
        set { self[DIContainerKey.self] = newValue }
    }
}
```

### Inject into app entry point
```swift
@main
struct MyApp: App {
    let container = DIContainer(network: RealNetworkService(), analytics: FirebaseAnalytics())

    var body: some Scene {
        WindowGroup {
            ContentView()
                .environment(\.diContainer, container)
        }
    }
}
```

### Summary
Technique	Pros	When to Use
Initializer Injection	Simple, testable	ViewModels or isolated components
@EnvironmentObject	Global, easy	App-wide shared state
DI Container	Scalable, centralized	Modular & larger apps
Singleton	Quick	Avoid long term in production
