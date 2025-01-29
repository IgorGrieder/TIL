# Event Bubbling

When you interact with an HTML element that has an event listener attached to it, the event propagates through its parent elements (in most cases).
First, the event executes on the target element and then propagates up the DOM tree. This is called event propagation.

### Which events do not propagate?

Some events such as `focus`, `mouseenter`, `mouseleave` do not propagate. To see the full list you can check out in [w3](https://www.w3.org/TR/uievents/).

## How does it work?

To understand properly how the event propagation happens I will illustrate it better.
Imagine the following HTML structure

```tsx
<div
  onClick={() => closeSection()}
  className="flex flex-col inset-0 bg-black/60 justify-center items-center"
>
  <div onClick={(e) => e.stopPropagation()}>
    <button onClick={() => doSomething()}>Some text</button>
  </div>
</div>
```

### Behavior

If we click on the button, the event propagates up through its parent elements until it reaches the document level.
Knowing this behavior we can use it in our favor. The code provided above acts like a modal and the desired behavior is that when
the user click outside of the div that contains the button the modal will close.

The behavior of a smooth close for the modal is achived because of the click event propagation. When the event is triggered by the `button` the function `doSomething()` will be executed and the propagation will stop once it reaches at it's direct div parent because of the `stopPropagation()` method being called.

## Stop an event bubbling with event.stopPropagation

Calling `event.stopPropagation()` prevents an event from bubbling up the DOM tree, stopping it from reaching parent elements. This is useful for preventing unintended behaviors or conflicts in an application. However, it should only be used when absolutely necessary, as excessive use can make event handling harder to manage.

## What is Event Bubbling?

There are three phases of event processing in JavaScript and although this article will just talk about the bubbling phase is good to know that exists three phases:

1. Capture - the event goes down the DOM tree to reach the event target
2. Target - the event target is reached
3. Bubbling - the event propagates up the DOM tree

In the diagram above, we see how an event propagates from the innermost element (child) to the outermost element (parent) in the DOM tree.

![Bubble image](imgs/bubble.svg)

Now that we’ve discussed event bubbling, let’s explore two key properties often used when handling bubbling events: `event.target` and `event.currentTarget`.

## The Difference Between event.target and event.currentTarget

Understanding the difference between event.target and event.currentTarget is crucial when handling complex event delegation or nested event listeners.
To talk about this topic we will use the following code:

```tsx
const handleClick = (event) => {
  // Logs the element that triggered the event
  console.log(event.target);

  // Logs the element that is the handler of the event
  console.log(event.currentTarget);
};

<form onClick={handleClick}>
  <p>Event bubbling in JavaScript</p>
  <button>Click</button>
</form>;
```

Let's imagine that we click on the button element inside the form. The event will bubble up and end up in the `onClick` handler in the `<form>`.

### event.target

- Represents the actual element that fired the event.
- If we click on the 'button' element it will display on the console the `<button>` element, since the element that triggered the event was the `<button>`.

### event.currentTarget

- Represents the element that is the handler of the event, in the context given it would be the `<form>` element.
- Always represents the element to which the event handler is attached

# Summary

In this post, we explored the concept of event bubbling, discussed which events don't propagate, and examined how to use event.target and event.currentTarget effectively in JavaScript.
