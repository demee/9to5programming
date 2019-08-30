---
layout: post
title: All I know about JavaScript functions
date: 2019-08-30 08:21 +0100
---
# All I know about JavaScript functions 

JavaScript is all functions. It should be called FunctionScript because it's all about functions.  Functions are just functions when we write them. When we us some call them methods, never call them routines... :D 

```javascript
// Function declaration
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
// Function expression
//                                always a space
//                               /   
let noLongerAnonymous = function () {

}
```

But it can also get a name

```javascript
let functionWithTwoNames = function actualFunctionName() {}
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











Next Episode







> Written with [StackEdit](https://stackedit.io/). 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkzNzA1OTczMSwxMjQ3MTIyMzQwXX0=
-->