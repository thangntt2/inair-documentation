# Overview
This section contains topics that describe the delegate model, show how to consume events in applications, and describe how to raise events from your class.

## Events and Delegates
An event is a message sent by an object to signal the occurrence of an action. The action could be caused by user interaction, such as a tap, or it could be triggered by some other program logic. The object that raises the event is called the event sender. The object that captures the event and responds to it is called the event receiver.

In event communication, the event sender class does not know which object or method will receive (handle) the events it raises. What is needed is an intermediary (or pointer-like mechanism) between the source and the receiver. InAir Framework defines a special class [Delegate][Delegate] that provides the functionality of a function pointer.

A delegate is a class that can hold a reference to a method. An delegate object is thus equivalent to a type-safe function pointer or a callback. The following example shows an delegate construction:
```java
Delegate doubleTapEventDelegate = Delegate.create(view, "onDoubleTap", TouchEventArgs.class);
```
In this example, `doubleTapEventDelegate` holds a reference to `onDoubleTap` method of `view` object. 

By convention, event handlers in the InAir Framework have two parameters, the source that raised the event and the data for the event. The `onDoubleTap` method in previous example must have the following signature:
```java
public void onDoubleTap(Object sender, TouchEventArgs e);
```

Custom event handlers are needed only when an event generates event data. Many events do not generate event data. In such situations, using the built-in [EventArgs][EventArgs], is adequate.
```java
Delegate focusEventDelegate = Delegate.create(view, "onGotFocus", EventArgs.class);
```

[Delegate]: http://developer.inair.tv/documents/inair/event/Delegate.html
[EventArgs]: http://developer.inair.tv/documents/inair/event/EventArgs.html