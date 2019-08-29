---
layout: post
title: javascript-functions
---

# All about JavaScript functions

Javascript is all just functions. Fuctions are just functions, some call them methods, never call them rutines... :D 

```javascript
function funnyFunction(parameter1, parameter2) {
    console.log(arguments.length);
}

//calling a function 
let argument1 = 1; 
let argument2 = 2; 
funnyFunction(argument1, argument2)
```

Writing function rules: 

```javascript
//                     never a space after name 
//  statement   name  /  always spaces after ()
// /           /     /  /
function functionName() {}
```

Anonymous functions 

```javascript
//                                always a space
//                               /   
let noLongerAnonymous = function () {

}
```

But it can also get a name

```javascript
let functionWithTwoNames = function actualFunctionName() {}
```