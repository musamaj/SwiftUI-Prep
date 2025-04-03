# Custom Transitions & Animations in SwiftUI

SwiftUI makes it easy to add smooth and interactive animations using built-in transitions and custom-defined animations. You can animate views when they appear, disappear, or change state.

## Basic Animations in SwiftUI

The `.animation(_:)` modifier allows you to animate property changes.

### Example: Basic Animation

```swift
struct SimpleAnimationView: View {
    @State private var isExpanded = false

    var body: some View {
        VStack {
            Button("Tap to Animate") {
                isExpanded.toggle()
            }

            Rectangle()
                .fill(Color.blue)
                .frame(width: isExpanded ? 200 : 100, height: isExpanded ? 200 : 100)
                .cornerRadius(20)
                .animation(.easeInOut(duration: 0.5), value: isExpanded)
        }
    }
}
```

## Implicit vs. Explicit Animations

### Implicit Animation
SwiftUI automatically animates a view when a state change occurs with `.animation(_:)`.

```swift
Text("Hello, SwiftUI!")
    .font(.largeTitle)
    .scaleEffect(isScaled ? 1.5 : 1)
    .animation(.spring(), value: isScaled)
```

### Explicit Animation (with `withAnimation`)
Use `withAnimation {}` when you want more control over when the animation runs.

```swift
withAnimation(.easeInOut(duration: 0.5)) {
    isExpanded.toggle()
}
```

## Custom Transitions in SwiftUI

Transitions define how a view appears and disappears. SwiftUI provides built-in transitions like `.slide`, `.opacity`, and `.scale`, but you can also create **custom transitions**.

### Example: Slide & Fade Transition

```swift
struct TransitionExample: View {
    @State private var isVisible = false

    var body: some View {
        VStack {
            Button("Toggle View") {
                withAnimation {
                    isVisible.toggle()
                }
            }

            if isVisible {
                RoundedRectangle(cornerRadius: 20)
                    .fill(Color.green)
                    .frame(width: 200, height: 200)
                    .transition(.slide.combined(with: .opacity))
            }
        }
    }
}
```

## Custom Transitions Using `AnyTransition`

You can create fully custom transitions using `modifier(active:identity:)`.

### Example: Scaling & Rotating Transition

```swift
extension AnyTransition {
    static var scaleRotate: AnyTransition {
        AnyTransition.scale(scale: 0.1).combined(with: .rotationEffect(.degrees(180)))
    }
}

struct CustomTransitionView: View {
    @State private var showView = false

    var body: some View {
        VStack {
            Button("Toggle") {
                withAnimation {
                    showView.toggle()
                }
            }

            if showView {
                Circle()
                    .fill(Color.purple)
                    .frame(width: 150, height: 150)
                    .transition(.scaleRotate)
            }
        }
    }
}
```

## Matched Geometry Effect for Smooth Transitions

The `@Namespace` property wrapper allows smooth transitions of a shared element across views.

### Example: Matched Geometry Effect

```swift
struct MatchedGeometryExample: View {
    @Namespace private var animationNamespace
    @State private var isExpanded = false

    var body: some View {
        VStack {
            if isExpanded {
                RoundedRectangle(cornerRadius: 20)
                    .fill(Color.orange)
                    .frame(width: 300, height: 300)
                    .matchedGeometryEffect(id: "shape", in: animationNamespace)
            } else {
                RoundedRectangle(cornerRadius: 20)
                    .fill(Color.orange)
                    .frame(width: 100, height: 100)
                    .matchedGeometryEffect(id: "shape", in: animationNamespace)
            }
        }
        .onTapGesture {
            withAnimation(.spring()) {
                isExpanded.toggle()
            }
        }
    }
}
```

## Summary

✔ **Use `.animation(_:)`** for implicit animations.  
✔ **Use `withAnimation {}`** for explicit animations.  
✔ **Transitions (`.slide`, `.opacity`, `.scale`)** define how views appear/disappear.  
✔ **Custom Transitions (`AnyTransition`)** allow advanced effects.  
✔ **`@Namespace` + `matchedGeometryEffect`** creates smooth view morphing animations.

