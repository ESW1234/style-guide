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

* Set default argument values using `||`
```javascript
// bad
function createHero(name, attribute) {
    var hero = {
        name: name
    };
    
    hero.attribute = "Strength";
    if(attribute) {
        hero.attribute = attribute;
    }
    
    return hero;
}

// good
function createHero(name, attribute) {
    var hero = {
        name: name,
        attribute: attribute || "Strength"
    };
    
    return hero;
}
```

* Prefer named function expressions. 
```javascript
// bad
var sumRuns = function(innings) { 
    ...
};

// acceptable, but not preferred.
window.addEventListener("scroll", function() {
    ...
}, false);

// good
window.addEventListener("scroll", function onScroll() {
    ...
}, false);

// good
var sumRuns = function sumRuns(innings) {
    ...
};
```
_Named function expressions show up as the function's name rather than `(anonymous)` in the debugger. There's some issues with IE8, but we don't support IE8, so who cares._

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
 
## Blocks
* Use braces for all multiline blocks.
```javascript
// bad
if(found)
    return true;

// good
if(found) return true;

// still good
if(found) { 
    return true;
}

// bad
function() { return false; }

// good
function() { 
    return false;
}
```

* Put `else` at the same line as your closing `if` brace.
```javascript
// bad
if(found) { 
    notifyUsers();
    markAsFound();
}
else {
    markAsUnfound();
}

// good
if(found) {
    notifyUsers();
    markAsFound();
} else {
    markAsUnfound();
}
```

## Comments
* Use `/* */` for multiline comments.
* Use `//` for single line comments. Place single line comments on the line directly above the subject of the comment, not on the same line. Place an empty line before the comment. Add a space between the `//` and the comment body. 
```javascript
// bad
var showUser = true; // Should the user be shown?

// bad
//Should the user be show?
var showUser = true;

// good
// Should the user be shown?
var showUser = true;

// bad
function markUserAsShown(shouldShow) {
    Logging.logEvent("markUserAsShown");
    // Mark the user as shown.
    var showUser = shouldShow;
    
    return showUser;
}

// good
function markUserAsShown(shouldShow) {
    Logging.logEvent("markUserAsShown");
    
    // Mark the user as shown.
    var showUser = shouldShow;
    
    return showUser;
}
```

* Don't place comments in the first line of a block.
```javascript
// bad
if(saveRating) { 
    // Saves the rating to the database.
    fireXHRRequest("/saveRating", ratingData);
}

// good
// Saves the rating to the database if saveRating is set to true.
if(saveRating) { 
    fireXHRRequest("/saveRating", ratingData);
}
```

## Whitespace
* ... we'll figure this out eventually.

## Punctuation
* Avoid leading commas.
```javascript
// bad
var robots = [
    "R2D2"
    , "Deckard"
    , "Johnny 5"
];

// good
var robots = [
    "R2D2",
    "Deckard",
    "Johnny 5"
];
```

* Avoid a final trailing comma.
```javascript
// bad
var validCatNames = [
    "Mittens",
    "Socks",
    "Strygwyr",
];

// good 
var validCatNames = [
    "Mittens",
    "Socks",
    "Strygwyr"
];
```

* Use them, except on function declarations.
```javascript
// bad
function doSomething() { 
    ...
};

// bad
var myFunction = function doSomething() {
    ...
}

// good
function doSomething() { 
    ...
}

// good
var myFunction = function doSomething() { 
    ...
};
```
_Function expressions require a semicolon becuase you're doing assignment, just as `var myNumber = 3;` requires a semicolon_

## Typecasting
* Coerse types at the beginning of a statement, but avoid unnecessary concatination.
```javascript
// bad
var userId = this.id + '';

// good
var userId = '' + this.id;

// bad
var userId = '' + this.id + '0000';

// good
var userId = this.id + '0000';
```

* Use `Number` to cast strings to numbers, unless you need a radix that isn't ten.
```javascript
var userInput = '234';

// bad
var myValue = new Number(userInput);

// bad
var myValue = +userInput;

// bad, but see note below.
var myValue = userInput >> 0;

// bad, no radix
var myValue = parseInt(userInput);

// good
var myValue = Number(userInput);

// good, if radix != 10
var myValue = parseInt(userInput, 8);
```
_Bitshifting is the quickest way to parse an integer, but rarely if ever do you need that tiny performance enhancement. If you're doing something crazy and integer conversion is your bottleneck, make a comment above it explaining why you used bitshift._

* Use `Boolean` or double bang to cast to a boolean.
```
var userInput = 0;

// bad
var myValue = new Boolean(userInput);

// good
var myValue = Boolean(userInput);

// good
var myValue = !!userInput;
```
_Double bang is twice as fast, but half as readable. Prefer `Boolean` unless you have performance concerns._

## Naming
* Be descriptive.
```javascript
// bad
function do() {
    ...
}

// good
function validateUserFormContent() {
    ...
}
```

* Use camelCase for objects, functions, and variables.
```javascript
// bad
var LOUDER_VARIABLES_STORE_DATA_BETTER = 0;
var beware_of_snake_case = {};
var o = {}; //Exception: for(var i....), although avoid for loops unless necessary.

// good
var myBrandNewObject = {};
function addDataToThatBrandNewObject() {}
```

* Use PascalCase for constructors and classes.
```javascript
// bad 
function user(first, last, email) {
    ...
}

var badUser = new user('wrong', 'case', 'disappointing@baduser.com');

// good
function User(first, last, email) {
    ...
}

var goodUser = new User('looks', 'better', 'hurray@gooduser.com');
```

* Don't use leading or trailing underscores.
```javascript
// bad
this._firstName = "Test";
this.__lastName__ = "Person";
this.email_ = "me@test.com";

// good
this.firstName = "Test";
```
_Javascript doesn't have private properties or methods, no matter how you name your variables. The only thing you can do to keep them private is keep them in the scope of the object on it's creation, but not part of the object itself. Adding underscores is just misleading._

* Don't make references to `this`. Use `bind`.
```javascript
// bad
function () { 
    var self = this;
    return function() { 
        console.log(self);
    }
}

// good
function () { 
    return function() { 
        console.log(self);
    }.bind(this);
}
```
_[`bind `is the best.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)_


## Accessors
* You don't need to make accessors for properties.
* If for whatever reason you're feeling like making them (maybe because you have a computed getter), use `getVal()` and `setVal(value)`
```javascript
// bad
hero.primaryAttribute();

// good
hero.getPrimaryAttribute();

// bad
hero.primaryAttribute("Strength");

// good
hero.setPrimaryAttribute("Strength");
```

* If it's a getter for a boolean, use `isVal` or `hasVal`
```javascript
// bad
if(!hero.primaryAttribute()) {
    return false;
}

// good
if(!hero.hasPrimaryAttribute()) {
    return false;
}
```

* Don't overwrite the prototype.
```
function Tower() {
    console.log('New hero created.');
}

// bad
Tower.prototype = {
    targetUnit: function targetUnit(unit) {
        ...
    },
    hasBackdoorProtection: function hasBackdoorProtection() {
        ...
    }
};

// good
Tower.prototype.targetUnit = function targetUnit(unit) {
    ...
}

Tower.prototype.hasBackdoorProtection = function hasBackdoorProtection() {
    ...
}
```
_Overwriting the prototype makes inheritance impossible._

* If you want to chain methods, have them return `this`. 


## Credits
* Based on the [Airbnb Javascript Style Guide](https://github.com/airbnb/javascript/tree/es5-deprecated/es5).




