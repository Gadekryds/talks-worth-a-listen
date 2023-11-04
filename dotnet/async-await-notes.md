
# Await Every Task

Non-awaited Tasks Hide Exceptions

# Avoid using Async Void

It doesn't show intent.
Intellisense doesn't show, that it's an async void, just void.
This can result in inconsistent behavior, and without showing intent, one may 
try to modify the same resources, as the void method.

It's very hard to catch exceptions from an Async void method.

It may be valid, in some cases.
[AsyncAwaitBestPractices](https://github.com/brminnick/AsyncAwaitBestPractices) package, can help with these issues.
Using **SafeFireAndForget** method.

# Constructors and await-async

You cannot await an async method in a constructor.
To get around this, you can await an async void method.
Use with care.

