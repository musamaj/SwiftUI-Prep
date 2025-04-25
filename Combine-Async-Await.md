### 🛠️ Example: Fetching a List of Users from a Network Service

Here’s a hands-on example showing how you can combine Combine and async/await in a modern iOS app:

### 🔹 Step 1: Service Layer Using async/await
This uses URLSession natively with async/await.

```swift
struct User: Decodable {
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

### 🔹 Step 2: ViewModel Using Combine + Async/Await Interop
Here we wrap async/await inside a Combine publisher so we can still use Combine’s reactivity in the UI.

```swift
import Combine

final class UserViewModel: ObservableObject {
    @Published var users: [User] = []
    @Published var errorMessage: String?

    private var cancellables = Set<AnyCancellable>()
    private let userService: UserService

    init(userService: UserService) {
        self.userService = userService
    }

    func loadUsers() {
        // Use Combine to bridge into async/await
        Future { [weak self] promise in
            Task {
                do {
                    let users = try await self?.userService.fetchUsers() ?? []
                    promise(.success(users))
                } catch {
                    promise(.failure(error))
                }
            }
        }
        .receive(on: DispatchQueue.main)
        .sink { completion in
            if case let .failure(error) = completion {
                self.errorMessage = error.localizedDescription
            }
        } receiveValue: { users in
            self.users = users
        }
        .store(in: &cancellables)
    }
}
```

###  🔹 Step 3: SwiftUI View Consuming the ViewModel

```swift
struct UserListView: View {
    @StateObject private var viewModel = UserViewModel(userService: NetworkUserService())

    var body: some View {
        List(viewModel.users, id: \.id) { user in
            Text(user.name)
        }
        .onAppear {
            viewModel.loadUsers()
        }
        .alert(item: $viewModel.errorMessage) { error in
            Alert(title: Text("Error"), message: Text(error), dismissButton: .default(Text("OK")))
        }
    }
}
```

### ✅ Summary:
Async/await was used where it made sense—clean and linear in service layer.

Combine was used in the ViewModel/UI layer for its @Published, pipeline structure, and cancellation management.

The two played nicely together via Future.
