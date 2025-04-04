# 🔗 Handling Deep Linking in SwiftUI

Deep linking allows your SwiftUI app to navigate directly to specific content or views based on a URL, such as `myapp://profile/123`.

---

## ✅ Why Use Deep Linking?

- Open specific screens from outside the app
- Handle push notifications and redirect users
- Integrate with other apps or websites
- Support Universal Links for web-to-app routing

---

## 🧱 Basic Setup with `.onOpenURL`

```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .onOpenURL { url in
                    DeepLinkManager.shared.handle(url: url)
                }
        }
    }
}
```

### Deeplink manager
```swift
class DeepLinkManager: ObservableObject {
    static let shared = DeepLinkManager()
    
    @Published var path = NavigationPath()
    
    func handle(url: URL) {
        guard url.scheme == "myapp" else { return }

        if url.host == "profile", let id = url.pathComponents.last {
            path.append(DeepLink.profile(userID: id))
        }
    }
}

enum DeepLink: Hashable {
    case profile(userID: String)
    case settings
}
```

### Navigation using NavigationStack
```swift
struct ContentView: View {
    @ObservedObject var deeplinkManager = DeepLinkManager.shared
    
    var body: some View {
        NavigationStack(path: $deeplinkManager.path) {
            HomeView()
                .navigationDestination(for: DeepLink.self) { link in
                    switch link {
                    case .profile(let userID):
                        ProfileView(userID: userID)
                    case .settings:
                        SettingsView()
                    }
                }
        }
    }
}
```

### 🌐 Universal Links Support
Enable Associated Domains
Go to Xcode > Signing & Capabilities

Add: applinks:myapp.com

Host the apple-app-site-association file
At:

arduino
Copy
Edit
https://myapp.com/.well-known/apple-app-site-association
Handle URL Routing
For full universal link handling, integrate with UIApplicationDelegate or SceneDelegate.
