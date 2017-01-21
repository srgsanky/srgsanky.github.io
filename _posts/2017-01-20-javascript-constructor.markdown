---
layout: post
title:  "How do constructors work in Javascript?"
date:   2017-01-20
categories: javascript
---

To create a custom object in javascript, we create a constructor function like

```javascript
function A() {
	// actual logic
	this.a = "abc";
}
```

We then create a new object using ``new``

```javascript
var obj1 = new A();
```

What javascript does internally is

```javascript
function A() {
	this = {}; // Added implicitly

	// actual logic
	this.a = "abc";

	return this; // Added implicitly
}
```

It implicitly creates a new object and assigns it to ``this``. At the end of the function (constructor) call, it also
implicitly returns ``this``.

Note: These implicit lines are not added by javascript when you use the function as a function without the ``new``
keyword.

[JS Bin example](https://jsbin.com/fotatidabu/1/edit?js,console)

You can also take control and create your own object and return it. For e.g.

```javascript
function B() {
	var myObj = {};

	myObj.value = 5;

	return myObj; // Remember to return your object
}

var obj2 = new B();
```

