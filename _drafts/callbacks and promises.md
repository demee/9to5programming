---
layout: post
title: Going in loops
date: 2020-12-01 08:21:01 +0000

---
The below convolution is inspired by the tweet https://twitter.com/garybernhardt/status/1333862272507158528

Let's consider little code

```javascript 
const array = [];
Promise.resolve(1).then(message => array.push(message));
array;
```
Copy & paste to chrome console and you get

```javascript
[1]
```

Looking at the code my first thought was, that is completly simple! 

Chrome is running each line one by one. And the promise callbacks (then) are microtasks. 

More about microtasks: https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide

Long story short, microtasks are executed when javascript stack is empty, and assuming that each of the line is executed separately, by the time second line is executed, it's microtasks time, callback in "then" is called, so the next line returns updated array. 

That is easy to verify, if I wrap the whole thing in IIFE i should get empty array, because calling a function would put "something" on the stack, and have it there until the whole thing finishes executing

