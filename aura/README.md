# ESW Aura Style Guide

---

## Components
* All `aura:component` and `aura:attribute` tags should have a `description` attribute.
```html
<!-- bad -->
<aura:attribute name="user" type="Object" />

<!-- good -->
<aura:attribute name="user" type="Object" description="The curren user." />
```

* Use `empty` to check for undefined, null, an empty array, or an empty string, rather than equality.
```html
<!-- bad -->
<aura:if isTrue="{!v.user == undefiend}">
    No user!
</aura:if>

<!-- good -->
<aura:if isTrue="{!empty(v.user)}">
    No user!
</aura:if>
```
_Note that `empty` returns true for the empty object `{}`._

* There's an issue with using `&&` in some Aura expressions. `||` and `!` work fine, though, and you can create the same logic using those two operators instead.

## Javascript Controllers
* Name your arguments `cmp`, `evt`, and `hlp`.
* Use anonymous function declarations.
```javascript
// bad
({
    doSomething: function doSomething(cmp, evt, hlp) {
        ...
    }
})

// good 
({
    doSomething: function(cmp, evt, hlp) {
        ...
    }
})
```
_Aura doesn't care how you define it, it becomes an anonymous function on compilation either way._