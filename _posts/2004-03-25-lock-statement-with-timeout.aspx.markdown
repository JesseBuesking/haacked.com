---
layout: post
title: "A lock statement with timeout..."
date: 2004-03-25 -0800
comments: true
disqus_identifier: 270
categories: [code]
---
Found this on Eric Gunnerson's (PM for the C# compliler team) blog.
It's an interesting approach to get a lock statement with a time out. It
would be nice to perhaps add a timeout syntax to the lock statement in
C#. Maybe it would look like this:

```csharp
object obj = new object();
int milliseconds = 10000;
try
{
    lock(obj, milliseconds)
    {
    	//Do something with obj
    }
}
catch(LockTimeOutException exception)
{
  //Handle exception
}
```

One thought I had, and let me know if I'm off base, but it seems we
could add debug code to Ian Griffith's TimedLock class to "register"
locks on an object. This would only happen if you conditionally compiled
with #DEBUG, but the idea is that when a class gets a TimedLock on an
object, TimedLock would add information (such as the call stack and
thread id) to a hashtable with the object as a key. Thus, if another
class attempts to get a lock on the object and times out, the exception
could have information about who had a lock on the object. May be useful
for debugging deadlock situations.

> Ian Griffiths comes up with an [interesting
> way](http://www.interact-sw.co.uk/iangblog/2004/03/23/locking)to use
> IDisposable and the “using“ statement to get a very of lock with
> timeout.
>
> I like the approach, but there are two ways to improve it:
>
> ​1) Define TimedLock as a struct instead of a class, so that there's
> no heap allocation involved.
>
> ​2) Implement Dispose() with a public implementation rather than a
> private one. If that's the case, the compiler will call Dispose()
> directly, otherwise it will box to the IDisposable interface before
> calling Dispose().
>
>  
>
> ![](http://weblogs.asp.net/ericgu/aggbug/95743.aspx)

*[Via [Eric Gunnerson's C#
Compendium](http://weblogs.asp.net/ericgu/archive/2004/03/24/95743.aspx)]*

