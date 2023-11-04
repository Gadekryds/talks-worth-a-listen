
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


# Use `ConfigureAwait(false)`

If it isn't important, which thread is picking back up the task after it finishes, use ConfigureAwait(false).
Unless you use this flag, we'll await until the calling thread is free to pickup the awaited Task.

Note: this only works when the framework is using SynchrnoizationContext.
- [X] WPF
- [X] WinForms
- [X] Xamarin
- [X] .NET MAUI
- [X] WInUI
- [ ] ASP.NET Core   

# Always Use CancellationTokens

If the async method doesn't allow for CancellationTokens you can use
.WaitAsync(token); to attach a cancellationToken anyways.

# Never use `.Wait()` and `.Result` It's a trap!

The Wait method will hijack the calling thread, and still run the task on another thread.
This means we'll use an additional thread for no reason.
For UI thread applications, this will freeze the app, in other apps, this can lead to threadpool exhaustion.

Instead use `.GetAwaiter().GetResult()`;


# You can use await on foreach loops

Using IAsyncEnumerable interface, yield and await on the foreach loop, we can 
run the foreach loop and each Task separately, so we can start reacting on each of these as they return.


# Use the IAsyncEnumerable interface for streaming data

This allows us to update the UI as we get data in, instead of a big bang.
Which in turn gives us a better User Expereince.

Use `[EnumeratorCancellation]` for CancellationToken.
``` csharp 
async IAsyncEnumerable<StoryModel> GetTopStories(int storyCount, [EnumeratorCancellation] CancellationToken token) {

    var listOfTasks = GetListOfTasks(token).ConfigureAwait(false);

    await forach(var task in listOfTasks)
    {
        // ... 
    }

}
```

# Using WhenAny

WhenAny will return a completedTask from a list of tasks, as soon as its completed.
This means we can start reacting more quickly, to the completed tasks.

# Use ValueTasks when it's possible.

Value types is added to the stack, which is much quicker.
If your "hot path" of your method returns right wait, without using the await keyword. (i.e. you've cached a value)
then you can use ValueTask as the return type, to get a performance boost.

# Avoid `return await` 

If the only place in your method you use the await keyword, is in the return statement, we can instead just return the task.
This will allow the caller to set the await keyword, as they see fit.

Don't do this if:

* You're in a try/catch block
* In a using block
