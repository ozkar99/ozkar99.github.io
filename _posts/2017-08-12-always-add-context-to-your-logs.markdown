---
layout: post
title:  "Always add context to your logs"
date:   2017-08-12
comments: true
---

<p class="intro"><span class="dropcap">A</span>lways add context to your logs</p><br /><br />


Its a good rule of thumb to add context to your logs, if you dont believe lets see a simple case.  


### Before Context
Imagine you have a system in which you run code asynchronously on diferent places.  

You can have a webserver running, a page, if something goes wrong you log the exception:
```C#
  try {
    DoSomeWebServerStuff()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

Then you have on the same server notifications arriving asynchronously:
```C#
  try {
    HandleNotification()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

And also you have a service running somewhere, doing taks:  
```C#
  try {
    DoServiceTask()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

We can already see that this smells really bad.  

The problem with this approuch (appart from enclosing everything in a huge `try-catch` and catching the root exception object `Exception`)  

Is that when you want to actually debug something and see the logs, you will see something like this:  

Timestamp|Level|Message
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object

### The Solution:  
This is easily fixable by adding context to the log calls:  
```C#
  try {
    DoSomeWebServerStuff()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

Becomes:  

```C#
  try {
    DoSomeWebServerStuff()
  }
  catch (Exception e) {
    Log.ErrorFormat("DoSomeWebServerStuff Exception: {0}", e)
  }
```
<hr />

```C#
  try {
    HandleNotification()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

Becomes:  
```C#
  try {
    HandleNotification()
  }
  catch (Exception e) {
    Log.ErrorFormat("HandleNotification Exception: {0}", e)
  }
```
<hr />

And
```C#
  try {
    DoServiceTask()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

Becomes:  
```C#
  try {
    DoServiceTask()
  }
  catch (Exception e) {
    Log.ErrorFormat("DoServiceTask Exception: {0}", e)
  }
```

Now when you look at the logs you will see something like this:  

Timestamp|Level|Message
XXXX-XX-XX XX:XX:XX|ERROR|DoServiceTask Exception: System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|HandleNotification Exception: System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|DoWebServerStuff Exception: System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|DoServiceTask Exception: System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|HandleNotification Exception: System.NullReferenceException: Object reference not set to an instance of an object
XXXX-XX-XX XX:XX:XX|ERROR|HandleNotification Exception: System.NullReferenceException: Object reference not set to an instance of an object

### Conclusion:

This is a very basic example, but its also a very basic concept, i shouldnt even be writing this article, but i still this kind of code in the wild. 

And yes i know thats basically what the stack trace is for, but you can also add even MORE context like `Log.DebugFormat("HandleNotification: Type {0}, {1}", notifType, rawNotif")` or `Log.DebugFormat("DoSomeWebServerStuff Response From WebService X: {0}", resp.RawResponse)`.  