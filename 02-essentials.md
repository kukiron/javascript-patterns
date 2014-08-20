# 2. Essentials

## minimizing globals

* JavaScript uses functions to manage scope
* variables declared within a function are local and not visible outside
* global variables are declared outside of functions or used without being declared
* every JS environment has a global object - accessible via ```this``` outside of any function
* every global variable becomes a property of the global object
* in browser the global object has a property called ```window``` pointing to the global object itself

### problems with globals

* shared among all the code in your app or webpage
* danger of name collisions (especially with third-party code)
* easy to create globals involuntarily: implied globals
* implied globals: undeclared var is global
* chained declaration also results in implied globals unless vars are declared in advance
* you might accidentally overwrite existing host object properties
* globals created with var outside of any function can't be ```delete```d
* ES5 strict mode doesn't allow undeclared variables

### access to the global object

* pass a reference to this from global scope to your function
* non-strict mode: any function invoked (not with ```new```) has ```this``` pointing at the global object

### single var pattern (?)

* single var statement at the top of your functions
* single place to look for all the vars needed by your function
* prevents logical errors when var is used before it's defined (hoisting)
* might be good practice to initialize them with some defaults
    * to avoid undefined
    * also communicates the intended use

### hoisting

* multiple var statements (anywhere in a function) act as if they were at the top

### caching array length in for loops (?)

* for cache array length ```for (var i, max = arr.length; i < max, i++)```
* unless length can change

### for in and hasOwnProperty when needed

* for in - filter things coming down the prototype chain
```js
for (prop in obj){
    if(obj.hasOwnProperty(prop)){
    }
}
```
* unless you know and can trust the object you're dealing with

### don't augment builtin prototypes

* augmenting the prototype property of constructor functions is powerful
* but hurts maintainability when done on builtin prototypes
* when you absolutely need to: communicate with your team and check if property already exists

```js
if (typeof Object.prototype.myMethod != = function) {
    Object.protoype.myMethod = function () { // implementation
    }
}
```

### ```switch```

* align each ```case``` with ```switch```
* indent code within each ```case```
* always end ```case``` with ```break;```
* avoid fallthroughs (no break)
* end the switch with ```default:```