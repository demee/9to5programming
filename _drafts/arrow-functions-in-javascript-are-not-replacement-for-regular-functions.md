---
tags: []
layout: post
title: Arrow functions in JavaScript are not replacement for regular functions
date: 2019-12-08 00:00:00 +0000

---
This post is not a tutorial on arrow functions, and you can find many [out there](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions). This is a warning and also an expression of my discontent with, what I see more and more common, using the arrow function as a replacement for a regular function without a particular reason. Like using old-style `function` expression was no longer cool ( maybe it's not? ).

It wouldn't be a coding blog without a bit of code, let's start with a comparison

```javascript
function () { /* function expression */ } 
() => { /* arrow function expression */ }
```

I assume you would know the diffrence beetween those two. There are actually serveral but the most preminent one is the `this` biding. The latter is equivalent of calling: 

```javascript 
(function () { /* function expression */)).bind(this)
```

Sure there are neuances in strict more etc, but it's all described in MDN page and I won't be getting into details now. 