---
layout: post
title: All I know about JavaScript functions
date: 2019-08-30 08:21 +0100
---
# Almost all

JavaScript is all about functions,  it should be called FunctionScript . Functions are just... functions, a reusable pieces of code. When we use them in cotext of object ( class ), we call them methods. We never call them routines in any case, please don't. 


## Simple function
Declaring simple function is straightforward in javascript. You don't need to call it main, you do not need a class around it. Just write a function declaration. 

When that comes in all languages when we declare function, elements that we declare to be passed to such function are called parameters. When we call a function, whatever we pass a the time of calling, we call arguments. Why, I don't know... but if someone asks you on an interview it's better to know. 


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
2. There should be a space after parenthesis, allways

Those simple rules help us very quickly indentify if a function is named or anonymous. 
```javascript
// Function declaration
//                     never a space after name 
//  statement   name  /  always spaces after ()
// /           /     /  /
function functionName() {}
```

Anonymous functions are simply functions without name, very useful in JavaScript when programming in functional way. 

```javascript
//        always a space
//       /   
function () {/* code */}
```

Anonymous function can be assined to a variable, and therefore get a name as well. 

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
```

There is also function constructor

```javascript
var sum = new Function('a', 'b', 'return a + b');
sum(1, 2)
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

Can we have private functions? 

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
```

Special functions?

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    return 'Hello ' + this.reverseName();
  }
  
  // private ? 
  
  private reverseName() {
    return this.name.split('').reverse().join('');
  }
}


let user = new User('Joe');
user.sayHi()

typeof User;    
```

Closures! This you must know

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

IIFE (Immediately Invoked Function Expression) 

```javascript
(function (global) {
    global.globalVariable = 'You should never declare global variables';
}(window || global))
```

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





> Written with [StackEdit](https://stackedit.io/). 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY1NjAzNzc0Miw2NDExOTA5MTEsMTkxMD
cyMjQwOCw2MDExMDk5MzUsLTExMDcxMjQyNjIsMTI0NzEyMjM0
MF19
-->