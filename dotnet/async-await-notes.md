
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


# Use ConfigureAwait(false)

If it isn't important, which thread is picking back up the task after it finishes, use ConfigureAwait(false).
Unless you use this flag, we'll await until the calling thread is free to pickup the awaited Task.

# Always Use CancellationTokens

If the async method doesn't allow for CancellationTokens you can use
.WaitAsync(token); to attach a cancellationToken anyways.

# Never use .Wait(). It's a trap!

The Wait method will hijack the calling thread, and still run the task on another thread.
This means we'll use an additional thread for no reason.
For UI thread applications, this will freeze the app, in other apps, this can lead to threadpool exhaustion.

Instead use .GetAwaiter().GetResult();

# Don't use .Result

Instead use .GetAwaiter().GetResult();
The alternative can lead to issues regarding Exception handling? 


# You can use await on foreach loops

Using IAsyncEnumerable interface, yield and await on the foreach loop, we can 
run the foreach loop and each Task separately, so we can start reacting on each of these as they return.
