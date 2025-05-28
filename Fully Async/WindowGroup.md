In SwiftUI, WindowGroup is the entry point for your app’s UI in a SwiftUI lifecycle app (introduced in iOS 14). It's the SwiftUI equivalent of setting up your main window in UIKit.

### 🪟 What is WindowGroup?
A WindowGroup defines a scene that can manage multiple windows of your app’s content — especially on platforms like macOS and iPadOS (with multi-window support). On iPhone, it just shows one window at a time.

✅ Example:

@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
@main marks this as the app’s entry point.

WindowGroup creates the app’s main window.

ContentView() is the root view for this window.

### 🔁 Why “Group”?
Because WindowGroup:

Supports multiple windows (especially on iPad/macOS).

Maintains independent state for each window.

Automatically manages scene lifecycle (activation, backgrounding, etc.).

### 🧱 How it Works

var body: some Scene {
    WindowGroup {
        NavigationStack {
            HomeView()
        }
    }
}
You can wrap your navigation, tab view, or any other root component here.

SwiftUI will launch this UI inside the window scene of the app.

### 🧠 Related Scene Types
Scene Type	Purpose
WindowGroup	Main app window, supports multiple instances
DocumentGroup	For document-based apps
Settings	macOS settings window
MenuBarExtra	macOS menu bar items

### ❗️Platform Behavior
Platform	WindowGroup Behavior
iPhone	Single window only
iPadOS	Multi-window supported
macOS	Full multi-window app


### 🧠 What is SceneDelegate?

SceneDelegate is part of the UIKit app lifecycle introduced in iOS 13. It was designed to help support multiple windows (scenes) in apps — especially for iPadOS and macOS Catalyst — and works alongside the traditional AppDelegate.

SceneDelegate manages the UI lifecycle of a single scene (window) of your app.

It’s called for:

Setting up the window and root view controller

Responding to scene lifecycle events like becoming active/inactive

Managing multi-window UI on iPad/macOS

### 🏗️ When Is It Used?
In UIKit-based apps with a SceneDelegate.swift file (created by Xcode templates for iOS 13+).

Not used in SwiftUI App lifecycle (@main struct MyApp: App) — SwiftUI uses WindowGroup and Scene.

### 📦 Typical SceneDelegate Setup
```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession,
               options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = scene as? UIWindowScene else { return }

        let window = UIWindow(windowScene: windowScene)
        window.rootViewController = UIHostingController(rootView: ContentView())
        self.window = window
        window.makeKeyAndVisible()
    }
}
```
This code sets up a SwiftUI view (ContentView) as the root of a UIKit window.

Called when a new scene (window) is being connected.

### 🔁 SceneDelegate vs AppDelegate
Aspect	AppDelegate	SceneDelegate
Scope	Entire App	Individual Scene (Window)
Introduced in	iOS 2	iOS 13
Handles	Launch, background, notifications	UI lifecycle of a window (scene)
Required in SwiftUI	❌ No (SwiftUI uses App protocol)	❌ No (replaced by SwiftUI scene system)

### ✅ When to Use SceneDelegate
UIKit apps targeting iOS 13+ with multiple scenes

If you’re not using SwiftUI App lifecycle

Apps that support multi-window behavior (iPad, Mac Catalyst)

### ❌ When It's Not Used
In SwiftUI apps using the @main App protocol (WindowGroup, etc.), there’s no SceneDelegate.
SwiftUI uses a declarative Scene system instead.
