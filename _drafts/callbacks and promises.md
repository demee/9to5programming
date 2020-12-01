---
layout: post
title: Going in loops
date: 2020-12-01 08:21:01 +0000

---
The below convolution is inspired by the [tweet](https://twitter.com/garybernhardt/status/1333862272507158528 "tweet").

Let's consider little code

    const array = [];
    Promise.resolve(1).then(message => array.push(message));
    array;

Copy & paste to chrome console and you get

```javascript
[1]
```

Looking at the code my first thought was, that is completely simple and I know how it works:

Chrome is running each line separately, one by one. And the promise callbacks (the code in then) are [microtasks](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide).

Long story short, microtasks are executed when javascript stack is empty, and assuming that each of the lines is executed separately, by the time the second line is executed, it's microtasks time, the callback in "then" is called, so the next line returns updated array.

That is easy to verify, if I wrap the whole thing in IIFE I should get an empty array because calling a function would put "something" on the stack, and have it there until the whole thing finishes executing

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

What is even more interesting, when running following code: 

```javascript
const array = [];
Promise.resolve(1).then(message => array.push(message));
console.log(array);
```

We do get the correct value. Or the expected one, as many of us, javascript developers inuitively think of: 

```javascript
[]
```

Promise's `then` is asychronous opearation after all, and we should not see its efects until the later. Also , when original code is ran in Firefox or NodeJS, we do get expected results, so I do have to agree with the author of the tweet that chrome is doing something wrong, or, at least diffrent. 


So now was time to [explore](https://www.infoq.com/presentations/chrome-debugger-protocol/) and learn a litlle how [Chrome's devtools protocol](https://chromedevtools.github.io/devtools-protocol/) works. 

Most simple answer here is, devtools takes the input from the console as string and post as WebSocket message to currently running page context. Chromium is able to read the message and execute the command that it is asked to do. 

In our case the command is [evaluateOnCallFrame](https://chromedevtools.github.io/devtools-protocol/tot/Debugger/#method-evaluateOnCallFrame). 

So we send a piece of JS, expect it to be evaluated on V8 in context of currently opened website (yet we don't care about it in this case), and then we expect the result back sent over that WebSocket. That does not explain why we get wrong result. This should be no diffrent from running it on Node. 







