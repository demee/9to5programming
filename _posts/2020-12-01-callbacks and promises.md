---
layout: post
title: Going in loops
date: 2020-12-01T08:21:01.000+00:00

---
The tweet inspires the below convolution.

Let's consider little code.

    const array = [];
    Promise.resolve(1).then(message => array.push(message));
    array;

Copy & paste to chrome console, and you get

```javascript
[1]
```

Looking at the code, my first thought was, that is completely simple, and I know how it works:

Chrome is running each line separately, one by one. And the promise callbacks (the code in then) are [microtasks](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide).

Long story short, microtasks are executed when the javascript stack is empty, and assuming that each of the lines is run separately, by the time the second line is completed, it's microtasks time, the callback in "then" is called, so the following line returns updated array.

That is easy to verify. If I wrap the whole thing in IIFE, I should get an empty array because calling a function would put "something" on the stack and have it there until the entire thing finishes executing.

```javascript
function banana() {
  const array = [];
  Promise.resolve(1).then(message => array.push(message));
  return array;
}

banana()
```

But that still returns

```javascript
[1]
```

What is even more interesting, when running the following code:

```javascript
const array = [];
Promise.resolve(1).then(message => array.push(message));
console.log(array);
```

We do get the correct value. Or the expected one, as many of us, javascript developers intuitively think of:

```javascript
[]
```

Promise's `then` It is an asynchronous operation, after all, and we should not see its effects until later. Also, when the original code is running in Firefox or NodeJS, we get expected results, so I agree with the tweet's author that Chrome is doing something wrong, or, at least different.

So now was time to [explore](https://www.infoq.com/presentations/chrome-debugger-protocol/) and learn a little about how [Chrome's dev tools protocol](https://chromedevtools.github.io/devtools-protocol/) works.

The most straightforward answer here is, dev tools take the input from the console as a string and post as WebSocket message to currently running page context. Chromium can read the statement and execute the command that it is asked to do.

In our case, the command is [evaluateOnCallFrame](https://chromedevtools.github.io/devtools-protocol/tot/Debugger/#method-evaluateOnCallFrame).

So we send a piece of JS, expect it to be evaluated on V8 in the context of the currently opened website (yet we don't care about it in this case), and then we expect the result back sent over that WebSocket. That does not explain why we get the wrong result. This should be no different from running it on Node. In the end, they both use V8, but the Node uses [REPLServer](https://github.com/nodejs/node/blob/master/lib/repl.js#L474), whereas chrome dev tools use dev tools protocol [built into V8](https://github.com/v8/v8/blob/dc712da548c7fb433caed56af9a021d964952728/src/inspector/v8-debugger-agent-impl.cc#L1229) to evaluate the expression.