---
layout: post
title: javascript-functions
date: 2019-08-30 08:21 +0100
---
# All about JavaScript functions

Javascript is all just functions. Functions are just functions, some call them methods, never call them routines... :D 

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

Functions are also constructors 

```javascript
function User(name) {
    this.name = name;
  
    this.welcomeUser = function () {
      return "Hello " + this.name;
    }
}

let user = new User("Joe");

user.welcomeUser();

user.welcomeUser //public 

``` 
