Combine’s reactive framework to observe, transform, and react to state changes, so UI updates automatically and cleanly reflect those changes.

### 🛠️ Scenario: Counter App with Combine
Let’s build a small example with:

@Published → to bind state from ViewModel to View

Just → to emit a single value (like a mock API)

PassthroughSubject → to manually trigger events

Combine pipelines → to transform and debounce data

### 📦 Model + ViewModel

```swift
import Foundation
import Combine

class CounterViewModel: ObservableObject {
    // Emits changes to SwiftUI
    @Published var count: Int = 0
    @Published var message: String = ""

    private var cancellables = Set<AnyCancellable>()
    
    // Subject for handling button tap events
    let incrementTapped = PassthroughSubject<Void, Never>()

    init() {
        setupBindings()
    }

    private func setupBindings() {
        // Increment logic: when tapped, increment count
        incrementTapped
            .map { [weak self] _ in (self?.count ?? 0) + 1 }
            .assign(to: &$count)
        
        // Transform count into a string message using Combine pipeline
        $count
            .map { "You clicked \($0) times!" }
            .debounce(for: .milliseconds(200), scheduler: RunLoop.main)
            .assign(to: &$message)
        
        // Simulate loading from API using Just
        Just(5)
            .delay(for: .seconds(1), scheduler: RunLoop.main)
            .sink { [weak self] initialValue in
                self?.count = initialValue
            }
            .store(in: &cancellables)
    }
}
```

### 🎨 SwiftUI View

```swift
import SwiftUI

struct CounterView: View {
    @StateObject private var viewModel = CounterViewModel()

    var body: some View {
        VStack(spacing: 20) {
            Text(viewModel.message)
                .font(.headline)
            
            Button("Increment") {
                viewModel.incrementTapped.send()
            }
        }
        .padding()
    }
}
```

### 🔍 What's Used Here

Combine Tool	Purpose
@Published	Publishes changes to count and message to update SwiftUI automatically.
PassthroughSubject	Sends manual events like button taps into the Combine pipeline.
Just	Emits a mock value (5) once, simulating API or default value.
Combine pipelines	Used .map, .debounce, .assign, .sink to transform and bind data.
