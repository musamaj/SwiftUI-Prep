# 🚀 SwiftUI & UIKit Interoperability

SwiftUI provides powerful declarative UI-building capabilities, but sometimes you need to integrate existing UIKit components. Apple offers `UIViewRepresentable` and `UIViewControllerRepresentable` to bridge the gap between SwiftUI and UIKit.

---

## 📌 Wrapping a UIKit View with `UIViewRepresentable`
Use `UIViewRepresentable` when you need to embed a UIKit view (e.g., `UILabel`, `UIButton`, `MKMapView`) into SwiftUI.

### 🔹 Example: Embedding `UILabel` in SwiftUI
```swift
import SwiftUI
import UIKit

struct CustomUIKitLabel: UIViewRepresentable {
    func makeUIView(context: Context) -> UILabel {
        let label = UILabel()
        label.text = "Hello from UIKit!"
        label.textAlignment = .center
        label.textColor = .blue
        return label
    }
    
    func updateUIView(_ uiView: UILabel, context: Context) {
        // Update the view if needed
    }
}

struct ContentView: View {
    var body: some View {
        CustomUIKitLabel()
            .frame(height: 50)
    }
}
```
✅ This allows you to use UIKit components seamlessly within SwiftUI.

---

## 📌 Wrapping a UIKit ViewController with `UIViewControllerRepresentable`
Use `UIViewControllerRepresentable` when you need to embed a UIKit ViewController inside SwiftUI (e.g., `UIImagePickerController`, `UIPageViewController`).

### 🔹 Example: Embedding a Custom `UIViewController`
```swift
import SwiftUI
import UIKit

struct CustomViewController: UIViewControllerRepresentable {
    func makeUIViewController(context: Context) -> UIViewController {
        let viewController = UIViewController()
        viewController.view.backgroundColor = .blue
        return viewController
    }
    
    func updateUIViewController(_ uiViewController: UIViewController, context: Context) {}
}

struct ContentView: View {
    var body: some View {
        CustomViewController()
            .edgesIgnoringSafeArea(.all)
    }
}
```
✅ This is useful for integrating UIKit-based flows in SwiftUI apps.

---

## 📌 Handling Delegates & Events with Coordinators
UIKit components often require delegation. SwiftUI provides `Coordinator` to handle these cases.

### 🔹 Example: Integrating `MKMapView` with Delegate
```swift
import SwiftUI
import MapKit

struct MapView: UIViewRepresentable {
    class Coordinator: NSObject, MKMapViewDelegate {}

    func makeCoordinator() -> Coordinator {
        return Coordinator()
    }
    
    func makeUIView(context: Context) -> MKMapView {
        let mapView = MKMapView()
        mapView.delegate = context.coordinator
        return mapView
    }
    
    func updateUIView(_ uiView: MKMapView, context: Context) {}
}
```
✅ `Coordinator` allows handling interactions like region changes, pin selection, etc.

---

## 📌 Embedding SwiftUI Views Inside UIKit
Sometimes, you need to use SwiftUI views inside UIKit-based applications. This is possible using `UIHostingController`.

### 🔹 Example: Embedding a SwiftUI View in a UIKit ViewController
```swift
import SwiftUI
import UIKit

struct SwiftUIView: View {
    var body: some View {
        Text("Hello from SwiftUI!")
            .font(.largeTitle)
            .foregroundColor(.blue)
            .padding()
    }
}

class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let swiftUIView = SwiftUIView()
        let hostingController = UIHostingController(rootView: swiftUIView)
        
        addChild(hostingController)
        hostingController.view.frame = view.bounds
        view.addSubview(hostingController.view)
        hostingController.didMove(toParent: self)
    }
}
```
✅ This allows integrating SwiftUI seamlessly within existing UIKit-based projects.

---

## 📌 Best Practices for UIKit-SwiftUI Interoperability
✔ Use `UIViewRepresentable` for simple UIKit views (e.g., labels, buttons).  
✔ Use `UIViewControllerRepresentable` for full ViewControllers (e.g., modals, media pickers).  
✔ Utilize `Coordinator` for delegation & event handling.  
✔ Use `UIHostingController` to embed SwiftUI views in UIKit.  
✔ Ensure UIKit components remain lightweight to avoid performance issues.  

---

## 🚀 Summary
SwiftUI makes it possible to integrate UIKit components using `UIViewRepresentable`, `UIViewControllerRepresentable`, and `UIHostingController`. Whether you're embedding UIKit views, handling complex interactions, or integrating legacy code, these tools allow smooth interoperability.

---

### 💡 Want to contribute?
Feel free to open an issue or submit a pull request with improvements! 🚀

