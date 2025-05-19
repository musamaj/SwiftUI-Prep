In Swift's structured concurrency (introduced in Swift 5.5), you can implement serial and concurrent tasks in a clear and maintainable way using async let, Task, TaskGroup, and await. Here's how to handle both:

### 🔁 Serial Tasks
Serial tasks are executed one after another, where each task waits for the previous to complete.

### ✅ Using await sequentially

```swift
func performSerialTasks() async {
    let result1 = await task1()
    let result2 = await task2(input: result1)
    let result3 = await task3(input: result2)
    print("Final result: \(result3)")
}
```

Each await pauses until the task finishes.
Ensures strict order of execution.

### 🔀 Concurrent Tasks
Concurrent tasks are executed in parallel (as much as the system allows), improving performance when tasks are independent.

✅ Using async let
Use when tasks are independent and return values.

```swift
func performConcurrentTasks() async {
    async let result1 = task1()
    async let result2 = task2()
    async let result3 = task3()

    // Await all results
    let results = await (result1, result2, result3)
    print("All results: \(results)")
}
```

Tasks start immediately and run concurrently.
Only await when results are needed.

### ✅ Using TaskGroup
Use when you need dynamic parallelism or want to spawn multiple tasks in a loop.

```swift
func performConcurrentTasksWithGroup() async {
    await withTaskGroup(of: String.self) { group in
        for i in 1...3 {
            group.addTask {
                await performTask(id: i)
            }
        }

        for await result in group {
            print("Task result: \(result)")
        }
    }
}
```

Tasks run concurrently and are managed automatically.
Handles cancellation and child task lifetimes safely.

### Summary
Pattern	Use Case
Sequential await	When order matters
async let	Few known concurrent tasks
TaskGroup	Many or dynamic concurrent tasks
