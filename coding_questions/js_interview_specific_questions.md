# Table of Contents

[tripleAdd(10)(20)(30)](#tripleAdd(10)(20)(30))

#javascript
# tripleAdd(10)(20)(30)

How to write a function in which would return the sum of these three numbers?

Write a function that would return a funcion that takes a parameter, that returns a funcion that takes another parameter but adds all the parameters from all functions and returns that value. 

**As a result**:

```javascript
function tripleAdd(num1) {
    return function(num2) {
        return function(num3) {
            return num1+num2+num3;
        }
    }
}
```

This above means to "curry" a function.

We can also write a curried function that takes in more than one parameter.

**Example**:

```javascript
function quadrupleAdd(num1) {
    return function(num2) {
        return function(num3, num4) {
            return num1+num2+num3+num4;
        }
    }
}
```

## What is an IIFE?

It is defined as an "Immediately Invoked Function Expression'

It is a function that is executed right after it is created

**Example**

```js
function doubleNumber(num) {
    return num * 2;
 }
```

Change the above method to an IIFE.

```js
(function doubleNumber(num) {
    return num * 2;
})(10);
```

The above function is now an IIFE in which 10 is passed to the function that proceeds it, in the above, 10 as num to the argument, and 20 should be what is returned to us.

So why are we using IIFEs?

The main reason is to preserve a private scope without one's function. we want to make sure we are not overwriting any global variables, which may accidentally happen, either a global local, or something in a library.

In order to solve the above problem is to wrap entire JS file inside an IIFE. For Example:

```js
(function() {
    function getTotal(a,b) {
        return a + b;
    }

    let $ = 'currency';
    if (true) console.log('hello world')'

}();
```

The above function would have its own private scope.

## Button Example

The below button example is coded badly. While the Buttons all display Button 1, Button 2, Button 3, when any of the buttons are clicked it will simply display 'This is button 6'. The reason is that before the button is clicked, i has already incremented up to 6, so that when the button is pressed, it will show what i is currently at, which is 6.
```js
function createButtons() {
   for (var i = 1; i <= 5; i++) {
     var body = document.getElementsByTagName("BODY")[0];
     var button = document.createElement("BUTTON");
     button.innerHTML = 'Button ' + i;
     button.onclick = function() {
     	alert('This is button ' + i);
     };
     body.appendChild(button);
   }
}
 
createButtons();
```

How can we solve this? We can modify the function to look like the following:
```js
function createButtons() {
   for (var i = 1; i <= 5; i++) {
     var body = document.getElementsByTagName("BODY")[0];
     var button = document.createElement("BUTTON");
     button.innerHTML = 'Button ' + i;
     (function(num) {
     	button.onclick = function() {
     		alert('This is button ' + num);
     	};
     })(i);
     body.appendChild(button);
   }
}
createButtons();
```
The cool thing about this solution is that it uses an IIFE. It passes the value of i every time the function is invoked (and it is invoked immediately) so that the num argument holds onto the value as needed.

Another method is:
```js
function createButtons() {
   for (var i = 1; i <= 5; i++) {
     var body = document.getElementsByTagName("BODY")[0];
     var button = document.createElement("BUTTON");
     button.innerHTML = 'Button ' + i;
     addClickfunctionality(button, i);
     body.appendChild(button);
   }
}

function addClickfunctionality(button, num) {
	button.onclick = function() {
    	alert('This is button ' + num);
    };
}
```

Changing `var` to `let` will also fix this. The reason why is because `var` is function scoped, whereas `let` is block scoped.  A variable declared with var in a function scope can’t be accessed outside that function scope.
