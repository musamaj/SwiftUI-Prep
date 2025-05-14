Investigating EXC_BAD_ACCESS related crashes in iOS is challenging but crucial. These crashes are typically due to dereferencing a deallocated or corrupted memory address — often caused by:

Dangling pointers

Double frees

Incorrect unowned/unmanaged references

Low-level C/Objective-C interop issues

Here’s a step-by-step strategy to diagnose and fix them:

🔍 1. Understand EXC_BAD_ACCESS
This exception occurs when your app accesses memory it shouldn't, like:

An object that has been deallocated

A nil pointer treated as valid

Unsafe memory mismanagement

🛠️ 2. Enable Zombies
This is your first line of defense.

🧟 Enable "Zombies":
Go to Edit Scheme → Run → Diagnostics → Enable Zombie Objects
This replaces deallocated objects with a zombie placeholder, which throws an error if accessed.

This helps identify messages sent to deallocated instances.

shell
Copy
Edit
UserDefaults.standard.set(true, forKey: "NSZombieEnabled")
🧪 3. Use Address Sanitizer (ASan)
This tool detects memory errors like:

Use-after-free

Buffer overflows

Stack memory issues

✅ How to enable:
Edit Scheme → Run → Diagnostics → Enable Address Sanitizer

Run your app. ASan gives detailed stack traces for illegal memory accesses.

🐛 4. Symbolicate the Crash Log
If this is a crash from production:

Use Xcode’s Organizer or symbolicatecrash tool.

Get the .dSYM file and app version that caused the crash.

Look at the backtrace for the thread that crashed (usually Thread 0).

It might look like:

plaintext
Copy
Edit
EXC_BAD_ACCESS (code=1, address=0x... )
Thread 0 Crashed:
0  libobjc.A.dylib objc_msgSend + 20
1  MyApp       ViewController.swift:45
Use this to locate the exact line that caused the issue.

📊 5. Use Crash Reporting Tools
Tools like:
Firebase Crashlytics

Sentry

Instabug

…can capture:

Breadcrumbs (what happened before crash)

Stack traces

App state

Device info

This helps correlate crashes with specific user actions or devices.

📌 6. Audit for Common Causes
Swift:
Force-unwrapping optionals (!)

Using unowned instead of weak in closures

Using UnsafePointer, UnsafeRawPointer, etc.

Bridging to/from C APIs

Objective-C:
Calling methods on nil/deallocated pointers

Misuse of __unsafe_unretained

Manual memory management (retain/release/autorelease errors)

🧹 7. Instruments: Leaks and Allocations
Open Instruments → Leaks or Allocations to:

Detect memory leaks

See object lifecycle

Verify if an object was deallocated unexpectedly

📚 8. Consider Static Analysis & Code Review
Use:

Xcode’s Static Analyzer: Shift + Cmd + B

Clang’s static analyzer

Review unowned/weak references in closures carefully

🚨 Bonus Tips:
Avoid unowned unless you’re 100% sure the object will always outlive the closure.

When using C APIs or Unsafe* types, validate lifetimes rigorously.

Add logs or breakpoints in deinit to understand object lifecycles.

Summary
Tool	Purpose
🧟 Enable Zombies	Catch use-after-free by raising exceptions
🧪 Address Sanitizer	Detects memory violations during runtime
🛠 Crash Symbolication	Identify crash cause in release builds
🧠 Instruments	Track object allocations/deallocations
🧰 Static Analyzer	Prevent common memory pitfalls

