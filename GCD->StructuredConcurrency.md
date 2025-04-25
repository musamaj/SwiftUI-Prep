let’s look at a real-world example where we replace GCD and completion handlers with Swift's modern async/await + Task. We'll use a simple network call use case for clarity.

### 🧵 Before: Using GCD + Completion Handler

```swift
func fetchUserProfile(completion: @escaping (Result<User, Error>) -> Void) {
    DispatchQueue.global().async {
        let url = URL(string: "https://example.com/user")!
        
        URLSession.shared.dataTask(with: url) { data, response, error in
            if let error = error {
                completion(.failure(error))
                return
            }

            guard let data = data,
                  let user = try? JSONDecoder().decode(User.self, from: data) else {
                completion(.failure(NSError(domain: "ParsingError", code: 0)))
                return
            }

            DispatchQueue.main.async {
                completion(.success(user))
            }
        }.resume()
    }
}
```

### ⚡ After: Using Async/Await + Task

```swift
func fetchUserProfile() async throws -> User {
    let url = URL(string: "https://example.com/user")!
    let (data, _) = try await URLSession.shared.data(from: url)
    return try JSONDecoder().decode(User.self, from: data)
}
```

### ✅ Usage in SwiftUI or ViewModel:

```swift
Task {
    do {
        let user = try await fetchUserProfile()
        print("User: \(user.name)")
    } catch {
        print("Error fetching user: \(error)")
    }
}
```

### 🎯 Key Improvements

Aspect	Old (GCD + Completion)	New (async/await + Task)
Readability	Nested, callback-heavy	Linear, synchronous-like
Error handling	Manual via Result or optionals	Native try/catch
Thread safety	Needs manual DispatchQueue.main.async	Automatically managed via @MainActor
Testability	Complex with expectation handling	Simple using async XCTest methods


### 🧵 Scenario: Legacy API with Completion Handler
Let’s look at how to bridge legacy completion-handler code into the new async/await world using withCheckedContinuation (or withUnsafeContinuation if you don’t want error validation).

```swift
func loadUserFromDisk(completion: @escaping (User?, Error?) -> Void) {
    DispatchQueue.global().asyncAfter(deadline: .now() + 1) {
        let user = User(id: 1, name: "John Appleseed")
        completion(user, nil)
    }
}
```
You want to use this like an async function so you can await it in modern code.


### 🔁 Convert it with withCheckedContinuation

```swift
func loadUserFromDiskAsync() async throws -> User {
    return try await withCheckedThrowingContinuation { continuation in
        loadUserFromDisk { user, error in
            if let error = error {
                continuation.resume(throwing: error)
            } else if let user = user {
                continuation.resume(returning: user)
            } else {
                continuation.resume(throwing: NSError(domain: "UnknownError", code: -1))
            }
        }
    }
}
```

### ✅ Now You Can Use It Like This:

```swift
Task {
    do {
        let user = try await loadUserFromDiskAsync()
        print("Loaded user: \(user.name)")
    } catch {
        print("Failed to load user: \(error.localizedDescription)")
    }
}
```

### 
🔍 When to Use This
✅ Use withCheckedThrowingContinuation when:

You’re migrating a legacy method that uses a completion handler with a Result, error, or optional-based pattern.

You want the compiler to help you detect if you forget to resume the continuation.

⚠️ Avoid this:

If the legacy method can call the completion more than once—only one resume() is allowed per continuation.

If the method is already async—you don’t need it.


