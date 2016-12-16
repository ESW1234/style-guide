# ESW Javascript Style Guide

## Objects
* Use literal syntax for object creation.
```javascript
// bad
var myObject = new Object();

// good
var myObject = {};
```

* When defining objects, put each property on a new line.
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
* Use literal syntax for array creation.
```javascript
// bad
var items = new Array();

//good
var items = [];
```

* To convert an array-like object (such as HTMLCollection) to an array, use slice
```javascript
var buttons = Array.prototype.slice.call(document.getElementsByTagName("button"));
```

* To iterate through an array, use forEach. Do not use `for(var item in array)` unless you're iterating through an object's properties.
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

* When iterating through object properties, use `hasOwnProperty` to avoid iterating through parent properties on the prototype chain.
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

* When defining a new array, if the items are all primitives keep them on one line. Otherwise, place each element on a new line, using continuation indentation.
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
* Use double quotes for strings.
```javascript
// bad
var greeting = 'Hello';

// good
var greeting = "Hello";
```
_Some style guides use single quotes, but since we're also working with Java, we want double quotes for consistency._

* Long strings should be divided across multiple lines using string concatination.
```javascript
// bad
var message = "That's great, it starts with an earthquake, Birds and snakes, and aeroplanes; and Lenny Bruce is not afraid. Eye of a hurricane, listen to yourself churn. World serves its own needs, don't mis-serve your own needs."

// good
var message = "That's great, it starts with an earthquake, Birds and snakes," +
    " and aeroplanes; and Lenny Bruce is not afraid. Eye of a hurricane, listen" +
    " to yourself churn. World serves its own needs, don't mis-serve your own needs."
```

## Functions
* Never have a parameter named `arguments`. This will override this `arguments` parameter automatically passed to every function.
```javascript
// bad
function callSomething(functionName, arguments) {
    ...
}

// good
function callSomething(functionName, args) {
    ...
}
```

* When defining a function within a non-function block (if, for, etc), always assign it to a variable.
```javascript
// bad
if(truthCondition) {
    function validateTruth() {
        return truthCondition;
    }
}

// good
var truthValidator;
if(truthCondition) {
    truthValidator = function validateTruth() {
        return truthCondition;
    }
}
```

## Properties
* Always use dot notation when accessing properties.
```javascript
var item = {
    name: "Ironwood Branch",
    cost: 50
};

// bad
var itemCost = item["cost"];

// good
var itemCost = item.cost;
```

* ...except when accessing properties with a variable.
```javascript
var item = {
    name: "Ironwood Branch",
    cost: 50
};

function getProperty(item, prop) {
    return item[prop];
}

var itemCost = getProperty(item, prop);
```

## Variables
* Always use `var` to declare variables. Declaring a variable without `var` will create a global variable, which pollutes the global namespace.
```javascript
// bad
team = new Team("Chicago", "Cubs");

// good
var team = new Team("Chicago", "Cubs");
```

* Preface each variable declaration with var, rather than separating a list of them with commas.
```javascript
// bad
var teams = teamManager.getTeams(),
    isTeamGood = team.isGood,
    teamColor = team.getColor();
    
// good
var teams = teamManager.getTeams();
var isTeamGood = team.isGood;
var teamColor = team.getColor();
```
_It's easy to make mistakes when doing comma separated variable declarations. Plus, you never have to swap out a semicolon for a comma._

* When declaring a list of variables, declare each variable on a new line, and declare unassigned variables last.
```javascript
// bad
var numTeams, teamColorMapping, goodTeams = teamManager.filterTeams("isGood");

// also bad
var numTeams;
var teamColorMapping;
var goodTeams = teamManager.filterTeams("isGood");

// good
var goodTeams = teamManager.filterTeams("isGood");
var numTeams;
var teamColorMapping;
```

* Hoist variables to the top of their scope.
```javascript
// bad
function validateSeason(team) {
    simulateSeason();
    var seasonResults = team.seasonResults;
    if(seasonResults.wins > 162) {
        return false;
    }
}

// good
function validateSeason(team) {
    var seasonResults;
    simulateSeason();
    seasonResults = team.seasonResults;
    if(seasonResults.wins > 162) {
        return false;
    }
}
```
_When hoisting variables, don't hoist their assignment._
* Named functions automatically have the variable name hoisted, but not the function name or body.
```javascript
function() {
    console.log(playerAction); // undefined
    
    playerAction(); // TypeError tradePlayer is not a function
    
    tradePlayer(); // ReferenceError tradePlayer is not defined
    
    var playerAction = function tradePlayer() {
        console.log("Player traded");
    };
}
```
* Function declarations hoist their name and their function body. 
```javascript
function() {
    console.log(tradePlayer); // Player traded
    
    function tradePlayer() {
        console.log("Player traded");
    }
}
```

## Comparison
* Avoid == and !== (truthy and falsy), and use === and !==.
```javascript
// Abandon all hope, ye who enter here.
if([0]) { // If statement evaluates to true ([0] is an array, arrays are objects, objects are true).
    console.log([0] == true); // false (true is converted to a number, then compared against [0])
    console.log(!![0] == true); //true ([0] is cast to a boolean, then compared against true)
}
```

* Conditional statements are evalualted by wrapping the conditional in a boolean cast, which follows these rules:
  * Objects evaluate to true.
  * Undefined evaluates to false.
  * Null evaluates to false.
  * Boolean evalulate to the value of the boolean.
  * Numbers evalulate to false if +0, -0, or NaN, otherwise true.
  * Strings evaluate to false if they're the empty string `""`, otherwise true.


