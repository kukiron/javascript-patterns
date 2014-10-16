# 6. Code reuse patterns

Prefer composition over inheritance.

## classical inheritance

* play on the word 'class', nothing to do with the word classical
* JavaScript has no classes but constructor functions make same people think that
* use the term constructor function
* probably a bad idea but worth knowing about

```js
function Parent(){ }
function Child(){ }
//inheritance magic happens here
inherit(Child, Parent);
```

* inherit is not part of the language have to implement it yourself

## classical default pattern

```js
function inherit(Child, Parent){
    child.prototype = new Parent();
}
```
* prototype points at new parent object
* children gets parent functionality via prototype
* drawback: children gets both own and prototype properties from parent
* drawback: can't really pass parameters, or end up with a lot of objects


## the prototype chain

* can think of objects as blocks of memory
* all objects from the same constructor point at same prototype object
* can think of it as if they were pointing at it via a ```__proto__``` property
* ```__proto__``` is actually available in some JavaScript environments
* properties are looked up by walking through this prototype chain:
* if an object doesn't have a property it's ```__proto__``` is consulted

## classical rent-a-constructor pattern

```js
function Child(a, b, c, d){
    Parent.apply(this, arguments);
}
```
* solves the problem of passing arguments
* borrows parent constructor
* passing the child object to be bound to this
* and passing any arguments
* children does not inherit prototype properties
* but gets true copies of own properties (not links)
* inheritance is just a one of action
* only copying own properties from parent
* no proto links are kept, can't access parent prototype properties
* multiple inheritance can be achieved by applying more than one constructors
* in case of multiple inheritance and duplicate properties - last one wins

## classical rent-and-set prototype

```js
function Child(a, b, c, d){
    Parent.apply(this, arguments);
}
Child.prototype = new Parent();
```
* combine the two patterns above
* children gets copies of parents own members
* and references re-usable functionality (from parents prototype)
* drawback: parent constructor is called twice

## classical share the prototype pattern

```js
function inherit(Child, Parent){
    C.prototype = P.prototype;
}
```
* no calls to the parent construtor at all
* just share prototype as reusable functionality should be in the prototype anyways
* fast lookups as all object references one prototype
* BUT children can modify parent behaviour

## classical temporary constructor pattern

```js
function inherit(Child,Parent){
    var Temp = function(){};
    Temp.prototype = Parent.prototype;
    Child.prototype = new Temp();
}
```
* a.k.a. proxy constructor
* similar to default pattern but proxying via Temp constructor
* Temp constructor makes sure we only inherit prototype properties
* storing 'superclass' may be achieved by ```Child.uber = Parent.prototype```
* may also reset the constructor property on the prototype (```Child.prototype.constructor=Child```)
* constructor property otherwise equals to 'Parent' and may be confusing when introspecting
* optimized version with immediate function and temp function in closure:

```js

var inherit = (function(){
    var Temp = function(){};
    return function(Child,Parent){
        Temp.prototype = Parent.prototype;
        Child.prototype = new Temp();
        Child.uber = Parent.prototype;
        Child.prototype.constructor = Child;
    }
})();
```