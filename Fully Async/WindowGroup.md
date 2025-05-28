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
