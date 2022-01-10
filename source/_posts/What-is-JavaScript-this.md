---
title: What is JavaScript `this`?
readtime: 10
date: 2021-12-06 22:05:12
description: An layman language article explaining JavaScript's `this`.
tags:
 - JavaScript
 - Web Development
---

To be published in Walmart's Medium [<u>portal</u>](https://medium.com/walmartglobaltech) soon.
<!-- 
`this` is a JavaScript keyword which is vastly misunderstood by a lot of developers and this post aims to bridge that gap. While this article is written for beginners but even experienced developers can use it as a refresher. `this` is a special identifier keyword which is automatically defined in the scope of every function but what it actually refers to is the real deal. This post is divided into three parts and in the first part, we will talk about what `this` is and then we’ll talk about some common misconceptions about `this` and finally, we will see some features of JS related to `this` keyword.
<hr>

## Prerequisite

I believe learning is like working out and if you don’t warm yourself up properly then you might end up hurting yourself. Thus before talking about `this`, let’s talk about a few JavaScript concepts which you should be familiar with for this article.

1) **The variables declared in the global scope as `var a = 2;` are nothing but global-object properties of the same name.** Yes, they are not copies of each other but are each other. To make this clearer look at the example below how we are able to access the value of `x` as the key of the global object `window`. Further, a thing to must note here is this only works with declarations using `var`, not `let/const`.


{% codeblock lang:js %}
// This example is supposed to run on Console of any modern browser

var x = "Hello World!";

console.log(window.x); // "Hello World!"
{% endcodeblock %}

2) **Call-site:** The location where a function is called from is called *call-site*. Usually finding the *call-site* just comprises of locating the line where the function is called from but this can be misleading as a function can be called at multiple places in the code. Thus the most accurate way to find the *call-site* is to find the *call-stack* and then see where the function was called from or in other words what was the previous function in *call-stack*. Let’s understand this with an example.

{% codeblock lang:js %}
function first() {
  // call-stack: `first`
  // call-site: global-scope
  second(); //
}

function second() {
  // call-stack: `first->second`
  // call-site is in `first`
  third();
}

function third() {
  // call-stack: `first->second->third`
  // call-site is in `second`
  console.log("Hello World");
}

first();
{% endcodeblock %}

<hr>

## What is this?

`this` is a binding made for each function invocation whose value depends on how and where the function is called. Binding made for `this` is usually related to variables declared using `var` and JavaScript running in [<u>non-strict</u>](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) mode. Further on the basis of the *call-site*, we can divide such bindings into four parts as given below:

* Default Binding

* Implicit Binding

* Explicit Binding

* *new* Binding

**Default Binding**: When we call a function in the global scope normally with no special decoration. *this *inside the function gets bound with the global scope. Yes, you can access all global variables using `this` inside such functions. One thing to notice here is it is only applicable for `non-strict` mode and variables defined using `var`.

{% codeblock lang:js %}
function doSomething() {
  // "use strict"; // uncomment to run in strict mode
  console.log( this.x )
}

var x = "Hello World!";

// Here the call-site of the function is global scope
doSomething(); // "Hello World!"
{% endcodeblock %}

*Strict mode*: The value of `this` is undefined in this case.

*let/const*: The variables defined with `let/const` are not bound.

**Implicit Binding**: When the call-site of function has a containing object (context object) as given below. That context object is bound to `this` inside that function. In the example below, `obj` is the context object and `this` gets bound to this object and all the keys will be accessible inside the function using `this`.

{% codeblock lang:js %}
function doSomething() {
  console.log( this.x );
}

// This is the context object
var obj = {
  x: "Hello World!",
  y: doSomething
}

obj.y(); // "Hello World!"
{% endcodeblock %}

*Strict mode*: This will work the same as `non-strict` mode.

*let/const*: Not applicable

**Explicit Binding**: This binding gives us the superpower to assign `this` to anything. We can achieve this using `call(..)` and `apply(..)` methods. Yes, we can set `this` to an object, variable, function or anything. But there’s a catch, `null` and `undefined` are replaced with global object and primitives are converted into equivalent objects. Whatever we will pass as the first argument that will be used as `this` binding by the function call, sweet.

{% codeblock lang:js %}
function doSomething() {
  // "use strict"; // uncomment to run in strict mode
  console.log( this.x );
}

var obj = {
  x: "Hello World!"
}

var x = "global"

// passing the object to be used for `this` explicitly
doSomething.call(obj);   // "Hello World!
doSomething.apply(obj);  // "Hello World!

// passing the `null` and `undefined`, `this` will be bind to global scope
doSomething.call(null);   // "Hello World!
doSomething.apply(null);  // "Hello World!
doSomething.call(undefined);   // "Hello World!
doSomething.apply(undefined);  // "Hello World!
{% endcodeblock %}

*Strict mode*: `null` and `undefined` don’t get replaced with the global object and primitives are not converted into objects.

*let/const*: Not supported when `this` binds to the global object (either directly or after getting replaced from `null` or `undefined`) but variables defined with `let/const` can be directly passed as `this` and it will get bound.

**New Binding:** This one can feel a little odd from the rest of the binding rules. In this kind of binding, if we invoke the function with `new` keyword and assign it to a variable then `this` is bound to that variable. Look at the example below for understanding. Here we assign the value of `new  doSomething()` to `bar` thus inside the function, `this` gets bound to bar.

{% codeblock lang:js %}
function doSomething(a) {
  this.x = "Hello";
  this.y = "World!";
}

var bar = new doSomething();
// `this` has been bound to bar here

console.log(bar.x); // Hello
console.log(bar.y); // World!
{% endcodeblock %}

*Strict mode*: No effect

*let/const*: The variables defined can be both `let/const`, the same behaviour is observed.

<hr>

## What this isn’t?

Let's see the most common misconceptions about `this` which a lot of developers have and dive deeper into each of them. People think `this` in a function refers to:

 1. Itself

 2. Its Scope

**Itself** Many developers believe that `this` refers to the function itself in which it is called which is wrong. Below is a simple example to prove it.

{% codeblock lang:js %}
function increase() {
  // increment each time
  this.x++;
}

// Create a property to the function object
increase.x = 0;

// Call function few times
increase();
increase();

console.log(increase.x); // 0 which means this doesn't refers the function itself
{% endcodeblock %}

Rather the correct way to keep track of how many times `increase` is called would be to actually use the `call` method to explicitly define the `this` keyword to be the `increase` object (itself). The below-given code example is a working example of it.


{% codeblock lang:js %}
function increase() {
  // increment each time
  this.x++;
}

// Create a property to the function object
increase.x = 0;

// Call function few times with using itself as `this`
increase.call(increase);
increase.call(increase);

console.log(increase.x); // 2
{% endcodeblock %}

**Its Scope** Another famous misconception developers have is that they think that it refers to the function’s scope. It is a tricky one because in global scope, `this` refers to the global scope itself but it is quite a misguided statement as we saw that it is not true in the previous section.

<hr>

## More JavaScript Features around this

**bind() function:** ES5 introduced this function which helps bind the value of `this` explicitly. `bind` returns a function with `this` set to the first argument of `bind`. This is something similar to `call` and `apply` but the difference is once we use `bind` the function retains its context so that it can be reused.

{% codeblock lang:js %}
function doSomething() {
  return this.a;
}

var x = doSomething.bind({a: 'Hello World!'});
console.log(x()); // Hello World!
{% endcodeblock %}

**arrow function:** In these functions, `this` retains the value of the enclosing lexical context's `this`. In global code, it will be set to the global object. Let's see a few examples below:

{% codeblock lang:js %}
var doSomething = (() => this.a);

var a = "From Global";
var Context = {a: 'Hello World!'};

// Use bind to set the `this` context
var x = doSomething.bind(Context);
console.log(x()); // From Global

// Use call to set the `this` context
var y = doSomething.call(Context);
console.log(y); // From Global
{% endcodeblock %}

**`this` in classes:** The behaviour of `this` in classes and functions is the same since classes are nothings but functions under the hood. Still, I would suggest a read [<u>here</u>](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this#class_context) that has a few corner cases. A common convention is to override `this` behavior so that `this` within classes always refers to the class instance. This is commonly seen in React Class components.

**this as a DOM handler:** When a function is used as an event handler, its `this` is set to the element on which the listener is placed. The code given below will print the DOM element itself.

{% codeblock lang:html %}
<button onclick="console.log(this.tagName.toLowerCase())">
  Click to see the name of tag
</button>
{% endcodeblock %}

<hr>

## References and Recommended Reads:

 1. [<u>You Don't Know JS: this and Object Prototype</u>](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/this%20&%20object%20prototypes/README.md#you-dont-know-js-this--object-prototypes)

 2. [<u>this: MDN docs</u>](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) -->
