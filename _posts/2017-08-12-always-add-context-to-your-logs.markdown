---
layout: post
title:  "Always add context to your logs"
date:   2017-08-12
comments: true
---

<p class="intro">
Its a good rule of thumb to add context to your logs, if you dont believe lets see a simple case.  
</p>


## Broken Logs:
Imagine you have a system in which you run code asynchronously on diferent places.  

You can have a webserver running, a page, if something goes wrong you log the exception:
```csharp
  try {
    DoSomeWebServerStuff()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

Then you have on the same server notifications arriving asynchronously:
```csharp
  try {
    HandleNotification()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

And also you have a service running somewhere, doing taks:  
```csharp
  try {
    DoServiceTask()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

We can already see that this smells really bad.  

The problem with this approach is that when you want to actually debug something and see the logs, you will see something like this:  

Timestamp|Level|Message
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|System.NullReferenceException: Object reference not set to an instance of an object...

## The Solution:  
This is easily fixable by adding context to the log calls:  
```csharp
  try {
    DoSomeWebServerStuff()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

Becomes:

```csharp
  try {
    DoSomeWebServerStuff()
  }
  catch (Exception e) {
    Log.ErrorFormat("DoSomeWebServerStuff Exception: {0}", e)
  }
```

<hr />

```csharp
  try {
    HandleNotification()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

Becomes:

```csharp
  try {
    HandleNotification()
  }
  catch (Exception e) {
    Log.ErrorFormat("HandleNotification Exception: {0}", e)
  }
```

<hr />

And

```csharp
  try {
    DoServiceTask()
  }
  catch (Exception e) {
    Log.Error(e)
  }
```

Becomes: 

```csharp
  try {
    DoServiceTask()
  }
  catch (Exception e) {
    Log.ErrorFormat("DoServiceTask Exception: {0}", e)
  }
```

Now when you look at the logs you will see something like this:  

Timestamp|Level|Message
XXXX-XX-XX XX:XX:XX|ERROR|DoServiceTask Exception: System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|HandleNotification Exception: System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|DoWebServerStuff Exception: System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|DoServiceTask Exception: System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|HandleNotification Exception: System.NullReferenceException: Object reference not set to an instance of an object...
XXXX-XX-XX XX:XX:XX|ERROR|HandleNotification Exception: System.NullReferenceException: Object reference not set to an instance of an object...

A little bit more easy to read, and easier to find quickly and expand on the desired error.

## Conclusion:

This is a very basic example, but its also a very basic concept.   

And yes i know thats basically what the stack trace is for, but you can also add even MORE context like 

```csharp
Log.DebugFormat("HandleNotification: Type {0}, {1}", notifType, rawNotif")
``` 
or 

```csharp 
Log.DebugFormat("DoSomeWebServerStuff Response From WebService X: {0}", resp.RawResponse)
```


## Notes:
I consider having a huge `try-catch` an anti-pattern.  
I also consider catching the root `Exception` object an anti-pattern.  

But ill leave that for another article.