### ⚡ Version 2: Fully Async/Await (No Combine)
Awesome! Let’s now rewrite the same flow using pure async/await with SwiftUI’s native concurrency support, introduced in iOS 15+. This approach is cleaner, simpler, and leverages Swift’s modern concurrency model end-to-end.

### 🔹 1. Service Layer

```swift
struct User: Decodable, Identifiable {
    let id: Int
    let name: String
}

protocol UserService {
    func fetchUsers() async throws -> [User]
}

final class NetworkUserService: UserService {
    func fetchUsers() async throws -> [User] {
        let url = URL(string: "https://example.com/api/users")!
        let (data, _) = try await URLSession.shared.data(from: url)
        return try JSONDecoder().decode([User].self, from: data)
    }
}
```

### 🔹 2. ViewModel Using @MainActor + Async/Await

```swift
import Foundation

@MainActor
final class UserViewModel: ObservableObject {
    @Published var users: [User] = []
    @Published var errorMessage: String?

    private let userService: UserService

    init(userService: UserService) {
        self.userService = userService
    }

    func loadUsers() async {
        do {
            users = try await userService.fetchUsers()
        } catch {
            errorMessage = error.localizedDescription
        }
    }
}
```

### 🔹 3. SwiftUI View Using .task {}

```swift
import SwiftUI

struct UserListView: View {
    @StateObject private var viewModel = UserViewModel(userService: NetworkUserService())

    var body: some View {
        List(viewModel.users) { user in
            Text(user.name)
        }
        .task {
            await viewModel.loadUsers()
        }
        .alert(isPresented: Binding<Bool>(
            get: { viewModel.errorMessage != nil },
            set: { _ in viewModel.errorMessage = nil }
        )) {
            Alert(
                title: Text("Error"),
                message: Text(viewModel.errorMessage ?? ""),
                dismissButton: .default(Text("OK"))
            )
        }
    }
}
```

### ✅ Benefits of This Version
Cleaner and more readable—no need to wrap in Future or manage AnyCancellables.

SwiftUI .task {} launches a structured task when the view appears.

State is managed using Swift’s concurrency model (@MainActor ensures thread safety).

Fully native approach for iOS 15+.

