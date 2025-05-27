
App states

https://www.linkedin.com/pulse/states-app-ios-understanding-lifecycle-application-hector-carmona-pjelc/

Apple’s iOS Human Interface Guidelines (HIG) are a comprehensive set of best practices and design principles that help developers and designers create intuitive, consistent, and user-friendly apps. Here's a breakdown of the key takeaways and best practices from the HIG:

### 🧠 Core Principles
### 1. Clarity
Text should be legible at all sizes.

Icons should be precise and universally understood.

Functionality should be obvious and intuitive.

### 2. Deference
The UI should help people understand and interact with content without competing with it.

Use motion, depth, and subtle effects to clarify hierarchy and functionality.

### 3. Depth
Visual layers and realistic motion convey hierarchy, provide feedback, and facilitate understanding.

### 🧩 Key Best Practices by Area
🔘 Navigation
Use standard navigation patterns like tab bars, navigation stacks, modals, or sheets.

Support back navigation.

Use gestures (like swipe to go back) appropriately.

### 🔤 Typography
Use the built-in San Francisco font (Dynamic Type) for consistency and accessibility.

Support font scaling to accommodate users with visual impairments.

### 🎨 Color
Use system colors for elements like buttons and text to support dark mode automatically.

Ensure sufficient contrast between text and background.

Avoid using color as the only indicator of information.

### ⚙️ Controls & Interactions
Use native controls like Button, TextField, Toggle, etc.

Ensure tappable areas are at least 44x44 points.

Provide feedback for every interaction (e.g., animations, haptics).

### 🧭 Adaptivity & Layout
Support multiple screen sizes using Auto Layout or SwiftUI’s layout system.

Make use of SafeArea and avoid hardcoding dimensions.

Support both portrait and landscape orientations when appropriate.

### 🌓 Dark Mode
Use system colors and materials (.background, .label, .secondarySystemBackground) to support dark mode automatically.

### ♿️ Accessibility
Label interactive elements with accessibilityLabel.

Use VoiceOver and Dynamic Type for inclusive design.

Support larger text sizes and contrast options.

### 💡 SwiftUI-Specific Best Practices (aligned with HIG)
Use SwiftUI’s system components (NavigationStack, Form, List) to automatically align with iOS aesthetics and behavior.

Take advantage of SF Symbols for consistent iconography.

Embrace @Environment, @State, and @Binding for managing view state.

### Things to follow

When developing an iOS app, following Apple’s Human Interface Guidelines is key to creating a product that feels native, intuitive, and delightful. Here are the main things I focus on:"

### 1. Platform Consistency
Use standard UIKit or SwiftUI components wherever appropriate.

Respect platform conventions for navigation, gestures, controls, and transitions.

Example: Use tab bars for top-level navigation and swipe-to-delete for list items.

### 2. User-Centric Design
Prioritize usability and accessibility.

Ensure content is legible with appropriate contrast, font sizes, and dynamic type support.

Support VoiceOver, Reduce Motion, and other accessibility settings.

### 3. Adaptive Layouts
Design for all screen sizes using Auto Layout or SwiftUI’s responsive tools.

Support Light and Dark Mode.

Use safe area insets to avoid UI clipping on devices like iPhone X and later.

### 4. Minimal and Focused UI
Avoid clutter—display only what's needed and let content shine.

Use clear hierarchy and affordances for interaction.

Example: Use SF Symbols and native buttons to convey purpose clearly.

### 5. Touch Interactions
Ensure tap targets are at least 44x44 points.

Provide responsive feedback—e.g., animations, haptics, or visual state changes.

Avoid blocking interactions with unnecessary alerts or modals.

### 6. Performance and Battery Efficiency
Minimize unnecessary background activity.

Use lazy loading, efficient animations, and Combine/async-await properly.

### 7. Privacy and Permissions
Only request necessary permissions and explain the need clearly.

Respect the user’s data and comply with App Store Review Guidelines.

### 8. Native Feel and Motion
Use system animations and transitions (e.g., navigation push/pop).

Follow Apple's gesture system unless there’s a strong reason to deviate.
