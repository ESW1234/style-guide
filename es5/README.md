# ESW Javascript Style Guide

## Objects
1. Use literal syntax for object creation.
```javascript
// bad
var myObject = new Object();

// good
var myObject = {};
```

2. When defining objects, put each property on a new line.
```javascript
// bad
var character = { name: "Tresdin", gold: 400 };

// good
var character = {
    name: "Tresdin",
    gold: 400
};
```

## Arrays
1. Use literal syntax for array creation.
```javascript
// bad
var items = new Array();

//good
var items = [];
```

2. To convert an array-like object (such as HTMLCollection) to an array, use slice
```javascript
var buttons = Array.prototype.slice.call(document.getElementsByTagName("button"));
```

3. To iterate through an array, use forEach. Do not use `for(var item in array)` unless you're iterating through an object's properties.
```javascript
var items = [0, 1, 2];
// bad
for(var i = 0; i < items.length; i++) {
    var item = items[i];
    ...
}

// very bad
for(var index in items) {
    var item = items[index];
    ...
}

//good
items.forEach(function(item) {
    ...
});
```

4. When iterating through object properties, use `hasOwnProperty` to avoid iterating through parent properties on the prototype chain.
```javascript
var user = {
    email: "newuser@test.com",
    isAdmin: false
};

for(var prop in user) {
    if(user.hasOwnProperty(prop)) {
        ...
    }
}
```

5. When defining a new array, if the items are all primitives keep them on one line. Otherwise, place each element on a new line, using continuation indentation.
```javascript
// good
var items = [0, 1, 2, 3, 4];

//bad
var items = [this.getUsername(), this.getEmail(), this.getLastLoginDate()];

//good
var items = [
    this.getUsername(), 
    this.getEmail(), 
    this.getLastLoginDate()
];
```

## Strings
1. Use double quotes for strings.
```javascript
// bad
var greeting = 'Hello';

// good
var greeting = "Hello";
```
_Some style guides use single quotes, but since we're also working with Java, we want double quotes for consistency._

2. Long strings should be divided across multiple lines using string concatination.
```javascript
// bad
var message = "That's great, it starts with an earthquake, Birds and snakes, and aeroplanes; and Lenny Bruce is not afraid. Eye of a hurricane, listen to yourself churn. World serves its own needs, don't mis-serve your own needs."

// good
var message = "That's great, it starts with an earthquake, Birds and snakes," +
    " and aeroplanes; and Lenny Bruce is not afraid. Eye of a hurricane, listen" +
    " to yourself churn. World serves its own needs, don't mis-serve your own needs."
```


