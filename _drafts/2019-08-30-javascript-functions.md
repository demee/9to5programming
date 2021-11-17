---
layout: post
title: All I know about JavaScript functions
categories:
- javascript
tags:
- javascript
- functions
excerpt: Amost all
date: 2019-08-30 08:21 +0100

---
# Almost all

JavaScript is all about functions,  it should be called FunctionScript. Functions are just... functions, reusable pieces of code. When we use them in the context of an object ( class ), we call them methods. We never call them routines in any case, please don't. 


## Simple function
Declaring simple function is straightforward in javascript. You don't need to call it main, you do not need a class around it. Just write a function declaration. 

When that comes in all languages when we declare a function, elements that we declare to be passed to such function are called parameters. When we call a function, whatever we pass the time of call, we call arguments. Why? I don't know... but if someone asks you on an interview it's better to know. 


```javascript
// Function declaration
function functionName(parameter1, parameter2) {
    console.log(arguments.length);
}

//calling a function 
let argument1 = 1; 
let argument2 = 2; 
functionName(argument1, argument2)
```

There are few simple spacing rules when writing a function declaration in JavaScript. Two most important: 

1. There should be **no** space after the name
2. There should be a space after the parenthesis, always. 

Those simple rules help us very quickly indentify if a function is named or anonymous. 
```javascript
// Function declaration
//                     never a space after name 
//  statement   name  /  always spaces after ()
// /           /     /  /
function functionName() {}
```

Anonymous functions are simply functions without a name, very useful in JavaScript when programming in a functional way. 

```javascript
//        always a space
//       /   
function () {/* code */}
```

An anonymous function can be assigned to a variable, and therefore get a name as well. 

```javascript
// Function expression
let functionWithTwoNames = function () {}
```
Functions can be also named inside objects: 
```javascript
let user = {
  name: 'Joe',
  printName: function () {
	console.log(this.name);
  }
}

user.printName(); // 'Joe'
```

There is also a function constructor. This, yet another, way of creating a function in javascript is useful for code generation. We can construct a function body dynamically based on our program need and create a function on the fly. 

```javascript
let buildFunction = function (operation) {
  let functionBody = `return a ${operation} b`;
  return new Function('a', 'b', functionBody);
}

let sum = buildFunction('+')
sum(1, 2); // 3

let sub = buildFunction('-');
sub(2, 1); // 1
```

Real-life example? [Microtemplating](https://johnresig.com/blog/javascript-micro-templating/) described by John Reising author of jQuery. 

## Function as 'classes'
Functions are also constructors and can be used to create instances of objects using `new` operator. 

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
Using the above method all class members assigned to `this` are public. Can we have private functions? Yes, we can. 

```javascript
function Calculator(name) {
  function sum(p1, p2) {
    return p1 + p2; 
  }
  function sub(p1, p2) {
    return p1 - p2; 
  }
  this.calculate = function (p1, p2, op) {
    switch(op) {
      case '+': 
        return sum(p1, p2);
        break;
      case '-': 
        return sub(p1, p2);
        break;
    }
  }
}

let calc = new Calculator();

calc.calculate(2, 2, '-')

calc.sum(2, 2); //💥Boom 💀sum is not a function error 
```
What we used above is a closure, it's a way in Javascript to encapsulate part of the code inside a function so the only subset of the program has access to them. For the rest of the code, those methods do not exist. 

## Classes
But, javascript has `class` operator right? Why not use this. Well yes, that's what we would normally do these days. But it's important to know that `class` operator is just syntactic sugar, the output is still a function. 

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
  reverseName() {
    return this.name.split('').reverse().join('');
  }
  sayHi() {
    return 'Hello ' + this.reverseName();
  }
}

let user = new User('Joe')
user.sayHi();
```
Unfortunately, at this moment there are no private methods in Javascript. You still need to 
Closures! There is a proposal for using `#` to mark functions and properties as private, but they are experimental and not available at the moment: 

```javascript
class User {
  #name; //private   
  constructor (name) {
	  this.name = name;
  }
  #sayHi() { /*...*/ } //privatre method
}
```

Above code does not execute in any of the current browsers. 

## Closures

Closures, or colloquially, functions inside functions. This is bread and butter of Javascript programming, and you know to know them! 

Closures deserve an article on their own, but I just mention them here since they are built using functions. 

```javascript
function tagGeneratorFactory() {
  let pre = '<img src="';
  let post = '" />';
  return function tagGenerator(img) {
    return pre + img + post; 
  } 
}

let tg = tagGeneratorFactory();
tg("http://images.com/image.png");
```

Closures pitfalls 

https://jsfiddle.net/demee/6ebsn9um/

```javascript
let numbers = [1, 2, 3, 4], i, element;

for(i = 0; i < numbers.length; i++) {
    element = document.createElement('div');
    
    element.innerHTML = numbers[i];
    
    element.addEventListener('click', function () {
        alert(numbers[i]);
        alert(i);
    });
    
    document.querySelector('body').appendChild(element);
};
```

IIFE (Immediately Invoked Function Expression).

```javascript
(function (global) {
    global.globalVariable = 'You should never declare global variables';
}(window || global))
```

Real life examples? Look at jQuery code. 

Calling functions

```javascript
function crateOfBeer(x, y) {
  let beer = '';
  for( let i = 0; i < y; i++) {
    beer += '🍺'.repeat(x);
    beer += '\n';
  }
  
  return beer;
}

crateOfBeer(2,2);
crateOfBeer.call(this, 2, 2);
crateOfBeer.apply(this, [2,2])
```

