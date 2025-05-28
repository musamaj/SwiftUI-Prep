In SwiftUI, the App Lifecycle was redesigned starting in iOS 14 with the introduction of the @main App protocol, replacing the traditional AppDelegate + SceneDelegate pattern from UIKit.

### 🚀 SwiftUI App Lifecycle Overview
At its core, SwiftUI uses a declarative and scene-based model.

✅ Entry Point:
```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
@main defines the entry point.

App protocol replaces UIApplicationDelegate.

WindowGroup is where the UI starts (like a window/scene container).

### 📦 App Lifecycle States
Even though SwiftUI is declarative, it still reflects key app lifecycle states:

State	Description
onAppear	Called when a view appears on screen.
onDisappear	Called when a view disappears.
onChange(of scenePhase)	Called when app enters background, foreground, inactive, etc.

### 🧠 Managing App Lifecycle
Use the environment value scenePhase to track app state transitions:

```swift
@main
struct MyApp: App {
    @Environment(\.scenePhase) private var scenePhase

    var body: some Scene {
        WindowGroup {
            ContentView()
        }
        .onChange(of: scenePhase) { newPhase in
            switch newPhase {
            case .active:
                print("App is active")
            case .inactive:
                print("App is inactive")
            case .background:
                print("App is in background")
            @unknown default:
                break
            }
        }
    }
}
```
📘 Traditional AppDelegate Support
If needed, you can still use UIApplicationDelegate inside SwiftUI:

```swift
@main
struct MyApp: App {
    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate

    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

```swift
class AppDelegate: NSObject, UIApplicationDelegate {
    func application(_ application: UIApplication,
                     didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
        // Setup code here
        return true
    }
}
```

### 💡 Summary
Feature	SwiftUI Lifecycle
Entry Point	@main struct MyApp: App
Main UI Container	WindowGroup
App State Tracking	@Environment(\.scenePhase)
Side Effects	.onAppear, .onChange, .task
UIKit Compatibility	@UIApplicationDelegateAdaptor
