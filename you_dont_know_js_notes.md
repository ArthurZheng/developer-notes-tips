#You Don't Know JS Book Tips


###1. LHS RHS Look-Up

It's better to conceptually think about it as: "who's the target of the assignment (LHS)" and "who's the source of the assignment (RHS)".


###2. Reference Error vs Type Error
If an RHS look-up fails to ever find a variable, anywhere in the nested Scopes, this results in a ReferenceError being thrown by the Engine. It's important to note that the error is of the type ReferenceError.

Now, if a variable is found for an RHS look-up, but you try to do something with its value that is impossible, such as trying to execute-as-function a non-function value, or reference a property on a null or undefined value, then Engine throws a different kind of error, called a TypeError.

ReferenceError is Scope resolution-failure related, whereas TypeError implies that Scope resolution was successful, but that there was an illegal/impossible action attempted against the result.

Scope is the set of rules that determines where and how a variable (identifier) can be looked-up. This look-up may be for the purposes of assigning to the variable, which is an LHS (left-hand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (right-hand-side) reference.

LHS references result from assignment operations. Scope-related assignments can occur either with the = operator or by passing arguments to (assign to) function parameters.


Both LHS and RHS reference look-ups start at the currently executing Scope, and if need be (that is, they don't find what they're looking for there), they work their way up the nested Scope, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don't.

Unfulfilled RHS references result in ReferenceErrors being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [^note-strictmode]), or a ReferenceError (if in "Strict Mode" [^note-strictmode]).


###3. Variable & Function Hoisting

Declarations themselves are hoisted, but assignments, even assignments of function expressions, are not hoisted.

Be careful about duplicate declarations, especially mixed between normal var declarations and function declarations -- peril awaits if you do!

Both function declarations and variable declarations are hoisted. But a subtle detail (that can show up in code with multiple "duplicate" declarations) is that functions are hoisted first, and then variables.


###4. Closure
Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

Any of the various ways that functions can be passed around as values, and indeed invoked in other locations, are all examples of observing/exercising closure.

Essentially whenever and wherever you treat functions (which access their own respective lexical scopes) as first-class values and pass them around, you are likely to see those functions exercising closure. 

Closure is when a function can remember and access its lexical scope even when it's invoked outside its lexical scope.


###5. this This the thing called "this"
However, the this mechanism provides a more elegant way of implicitly "passing along" an object reference, leading to cleaner API design and easier re-use.

"this" is actually a binding that is made when a function is invoked, and what it references is determined entirely by the call-site (the location in code where a function is called (not where it's declared).) where the function is called.

We learned instead that this is a binding made for each function invocation, based entirely on its call-site (how the function is called).

What's important is to think about the call-stack (the stack of functions that have been called to get us to the current moment in execution). The call-site we care about is in the invocation before the currently executing function.


###6. Constructor Call
So, pretty much any ol' function, including the built-in object functions like Number(..) (see Chapter 3) can be called with new in front of it, and that makes that function call a constructor call. This is an important but subtle distinction: there's really no such thing as "constructor functions", but rather construction calls of functions.

When a function is invoked with new in front of it, otherwise known as a constructor call, the following things are done automatically:

1. a brand new object is created (aka, constructed) out of thin air
2. the newly constructed object is [[Prototype]]-linked
3. the newly constructed object is set as the this binding for that function call
4. unless the function returns its own alternate object, the new-invoked function call will automatically return the newly constructed object.


###7. Determining this

Now, we can summarize the rules for determining this from a function call's call-site, in their order of precedence. Ask these questions in this order, and stop when the first rule applies.

1. Is the function called with new (new binding)? If so, this is the newly constructed object.

var bar = new foo()

2. Is the function called with call or apply (explicit binding), even hidden inside a bind hard binding? If so, this is the explicitly specified object.

var bar = foo.call( obj2 )

3. Is the function called with a context (implicit binding), otherwise known as an owning or containing object? If so, this is that context object.

var bar = obj1.foo()

4. Otherwise, default the this (default binding). If in strict mode, pick undefined, otherwise pick the global object.

var bar = foo()

That's it. That's all it takes to understand the rules of this binding for normal function calls. Well... almost.


###8. DMZ De-Millitarized Zone Object ø
Whatever you call it, the easiest way to set it up as totally empty is Object.create(null) (see Chapter 5). Object.create(null) is similar to { }, but without the delegation to Object.prototype, so it's "more empty" than just { }.


###9. "this" bounding and Arrow Functions

Once examined, four rules can be applied to the call-site, in this order of precedence:

Called with new? Use the newly constructed object.

Called with call or apply (or bind)? Use the specified object.

Called with a context object owning the call? Use that context object.

Default: undefined in strict mode, global object otherwise.

Be careful of accidental/unintentional invoking of the default binding rule. In cases where you want to "safely" ignore a this binding, a "DMZ" object like ø = Object.create(null) is a good placeholder value that protects the global object from unintended side-effects.

Instead of the four standard binding rules, ES6 arrow-functions use lexical scoping for this binding, which means they adopt the this binding (whatever it is) from its enclosing function call. They are essentially a syntactic replacement of self = this in pre-ES6 coding.
Status API Training Shop Blog About Pricing


###10. Objects, Function, Array
Function is a sub-type of object (technically, a "callable object"). Functions in JS are said to be "first class" in that they are basically just normal objects (with callable behavior semantics bolted on), and so they can be handled like any other plain object.

Arrays are also a form of objects, with extra behavior. The organization of contents in arrays is slightly more structured than for general objects.


###11. Six Primary Types
They are one of the 6 primary types (called "language types" in the specification) in JS:

string
number
boolean
null
undefined
object

Note that the simple primitives (string, number, boolean, null, and undefined) are not themselves objects.


###12. Built-in Objects
String
Number
Boolean
Object
Function
Array
Date
RegExp
Error

But in JS, these are actually just built-in functions. Each of these built-in functions can be used as a constructor (that is, a function call with the new operator -- see Chapter 2), with the result being a newly constructed object of the sub-type in question.

null and undefined have no object wrapper form, only their primitive values. By contrast, Date values can only be created with their constructed object form, as they have no literal form counter-part.

Objects, Arrays, Functions, and RegExps (regular expressions) are all objects regardless of whether the literal or constructed form is used. The constructed form does offer, in some cases, more options in creation than the literal form counterpart. Since objects are created either way, the simpler literal form is almost universally preferred. Only use the constructed form if you need the extra options.

In objects, property names are always strings. If you use any other value besides a string (primitive) as the property, it will first be converted to a string. 


###13. Property vs Methods
In objects, property names are always strings. If you use any other value besides a string (primitive) as the property, it will first be converted to a string. 


###14. Arrays
Arrays assume numeric indexing, which means that values are stored in locations, usually called indices, at non-negative integers, such as 0 and 42.

Use objects to store key/value pairs, and arrays to store values at numeric indices.


###15. Duplicating Objects

Classes mean copies.

When traditional classes are instantiated, a copy of behavior from class to instance occurs. When classes are inherited, a copy of behavior from parent to child also occurs.

JavaScript does not automatically create copies (as classes imply) between objects.

Explicit mixins are also not exactly the same as class copy, since objects (and functions!) only have shared references duplicated, not the objects/functions duplicated themselves. 


###16. Prototype
Objects in JavaScript have an internal property, denoted in the specification as [[Prototype]], which is simply a reference to another object. Almost all objects are given a non-null value for this property, at the time of their creation.


###17. "in" and "hasOwnProperty"
If you use the in operator to test for the existence of a property on an object, in will check the entire chain of the object (regardless of enumerability).


###18. Object.prototype
The top-end of every normal [[Prototype]] chain is the built-in Object.prototype. This object includes a variety of common utilities used all over JS, because all normal (built-in, not host-specific extension) objects in JavaScript "descend from" (aka, have at the top of their [[Prototype]] chain) the Object.prototype object. Some utilities found here you may be familiar with include .toString() and .valueOf(), hasOwnProperty(), isPrototypeOf().


###19. Shadowing
myObject.foo = "bar" assignment

If the property name foo ends up both on myObject itself and at a higher level of the [[Prototype]] chain that starts at myObject, this is called shadowing. The foo property directly on myObject shadows any foo property which appears higher in the chain, because the myObject.foo look-up would always find the foo property that's lowest in the chain.


###20. Javascript 'Object'
In JavaScript, classes can't (being that they don't exist!) describe what an object can do. The object defines its own behavior directly. There's just the object.


###21. Javascript prototype
all functions by default get a public, non-enumerable (see Chapter 3) property on them called prototype, which points at an otherwise arbitrary object.
In JavaScript, we don't make copies from one object ("class") to another ("instance"). We make links between objects. 

Instead, JS creates a link between two objects, where one object can essentially delegate property/function access to another object. "Delegation" (see Chapter 6) is a much more accurate term for JavaScript's object-linking mechanism.


###22. Javascript Constructor Call
Functions themselves are not constructors. However, when you put the new keyword in front of a normal function call, that makes that function call a "constructor call". In fact, new sort of hijacks any normal function and calls it in a fashion that constructs an object, in addition to whatever else it was going to do. The call withe new in front of a function was a constructor call, but the function NothingSpecial is not, in and of itself, a constructor.

In other words, in JavaScript, it's most appropriate to say that a "constructor" is any function called with the new keyword in front of it.

Functions aren't constructors, but function calls are "constructor calls" if and only if new is used.

The best thing to do is remind yourself, "constructor does not mean constructed by".

But we traditionally think of "inheritance" as being a relationship between two "classes", rather than between "class" and "instance".


###23. Object.create
Bar.prototype = Object.create( Foo.prototype );

Object.create(..) creates a "new" object out of thin air, and links that new object's internal [[Prototype]] to the object you specify (Foo.prototype in this case).

In other words, that line says: "make a new 'Bar dot prototype' object that's linked to 'Foo dot prototype'."


###24. Prototype Chain
This linkage is (primarily) exercised when a property/method reference is made against the first object, and no such property/method exists. In that case, the [[Prototype]] linkage tells the engine to look for the property/method on the linked-to object. In turn, if that object cannot fulfill the look-up, its [[Prototype]] is followed, and so on. This series of links between objects forms what is called the "prototype chain".


###25. Object.create()
```
var foo = {
    something: function() {
        console.log( "Tell me something good..." );
    }
};

var bar = Object.create( foo );

bar.something(); // Tell me something good...
```
Object.create(..) creates a new object (bar) linked to the object we specified (foo), which gives us all the power (delegation) of the [[Prototype]] mechanism, but without any of the unnecessary complication of new functions acting as classes and constructor calls, confusing .prototype and .constructor references, or any of that extra stuff.

Note: Object.create(null) creates an object that has an empty (aka, null) [[Prototype]] linkage, and thus the object can't delegate anywhere. Since such an object has no prototype chain, the instanceof operator (explained earlier) has nothing to check, so it will always return false. These special empty-[[Prototype]] objects are often called "dictionaries" as they are typically used purely for storing data in properties, mostly because they have no possible surprise effects from any delegated properties/functions on the [[Prototype]] chain, and are thus purely flat data storage.

We don't need classes to create meaningful relationships between two objects. The only thing we should really care about is objects linked together for delegation, and Object.create(..) gives us that linkage without all the class cruft.

In other words, delegation will tend to be less surprising/confusing if it's an internal implementation detail rather than plainly exposed in your API interface design.

the key distinction is that in JavaScript, no copies are made. Rather, objects end up linked to each other via an internal [[Prototype]] chain. Instead, "delegation" is a more appropriate term, because these relationships are not copies but delegation links.

Javascript objects are actually delegation links, rather than "copies" of traditional object oriented class inheritance/instantiation.


###26. Behaviour Delegation
In other words, the actual mechanism, the essence of what's important to the functionality we can leverage in JavaScript, is all about objects being linked to other objects.

Behavior Delegation means: let some object (XYZ) provide a delegation (to Task) for property or method references if not found on the object (XYZ).

Rather than organizing the objects in your mind vertically, with Parents flowing down to Children, think of objects side-by-side, as peers, with any direction of delegation links between the objects as necessary.


###27. JS Values as Types
In JavaScript, variables don't have types -- values have types. Variables can hold any value, at any time.


###28. Function, Array seen as Sub-type of Object
Function is actually a "subtype" of object. Specifically, a function is referred to as a "callable object" -- an object that has an internal [[Call]] property that allows it to be invoked.


###29. typeof
The typeof operator always returns a string. So:

typeof typeof 42; // "string"
The first typeof 42 returns "number", and typeof "number" is "string".

The typeof operator returns "undefined" even for "undeclared" (or "not defined") variables. Notice that there was no error thrown when we executed typeof b, even though b is an undeclared variable. This is a special safety guard in the behavior of typeof.


###30. Javascript built-in types

JavaScript has seven built-in types: null, undefined, boolean, number, string, object, symbol. They can be identified by the typeof operator.

The safety guard (preventing an error) on typeof when used against an undeclared variable can be helpful in certain cases.
```
// oops, this would throw an error!
if (DEBUG) {
    console.log( "Debugging is starting" );
}

// this is a safe existence check
if (typeof DEBUG !== "undefined") {
    console.log( "Debugging is starting" );
}
```

###31. Javascript Objects and Arrays
Use objects for holding values in keys/properties, and save arrays for strictly numerically indexed values.
JavaScript strings are immutable, while arrays are quite mutable. 

arrays have a reverse() in-place mutator method, but strings do not:
```
a.reverse;      // undefined

b.reverse();    // ["!","o","O","f"]
b;              // ["!","o","O","f"]
```

###32. JS Numbers 
JavaScript has just one numeric type: number. This type includes both "integer" values and fractional decimal numbers.

NaN Number
It would be much more accurate to think of NaN as being "invalid number," "failed number," or even "bad number," than to think of it as "not a number." The type of not-a-number is 'number'!"
```
var a = 2 / "foo";      // NaN
typeof a === "number";  // true

NaN is a very special value in that it's never equal to another NaN value (i.e., it's never equal to itself). It's the only value, in fact, that is not reflexive (without the Identity characteristic x === x). So, NaN !== NaN. 

if (!Number.isNaN) {
    Number.isNaN = function(n) {
        return n !== n;
    };
}
```

###33. Object.is()
As of ES6, there's a new utility that can be used to test two values for absolute equality, without any of these exceptions(NaN !== NaN, and -0 prentes it's ==== 0). It's called Object.is(..):
```
var a = 2 / "foo";
var b = -3 * 0;

Object.is( a, NaN );    // true
Object.is( b, -0 );     // true

Object.is( b, 0 );      // false
```

###34. JS Value vs Reference
Simple values (aka scalar primitives) are always assigned/passed by value-copy: null, undefined, string, number, boolean, and ES6's symbol.

Compound values -- objects (including arrays, and all boxed object wrappers -- see Chapter 3) and functions -- always create a copy of the reference on assignment or passing.

To effectively pass a compound value (like an array) by value-copy, you need to manually make a copy of it, so that the reference passed doesn't still point to the original. For example:

foo( a.slice() );
slice(..) with no parameters by default makes an entirely new (shallow) copy of the array. So, we pass in a reference only to the copied array, and thus foo(..) cannot affect the contents of a.


###35. null, undefined, void
The null type has just one value: null, and likewise the undefined type has just the undefined value. undefined is basically the default value in any variable or property if no other value is present. The void operator lets you create the undefined value from any other value.

Numbers include several special values, like NaN (supposedly "Not a Number", but really more appropriately "invalid number"); +Infinity and -Infinity; and -0.

Simple scalar primitives (strings, numbers, etc.) are assigned/passed by value-copy, but compound values (objects, etc.) are assigned/passed by reference-copy. References are not like references/pointers in other languages -- they're never pointed at other variables/references, only at the underlying values.


###36. Symbols
New as of ES6, an additional primitive value type has been added, called "Symbol". Symbols are special "unique" (not strictly guaranteed!) values that can be used as properties on objects with little fear of any collision. They're primarily designed for special built-in behaviors of ES6 constructs, but you can also define your own symbols.

Symbols can be used as property names, but you cannot see or access the actual value of a symbol from your program, nor from the developer console. If you evaluate a symbol in the developer console, what's shown looks like Symbol(Symbol.create), for example.

There are several predefined symbols in ES6, accessed as static properties of the Symbol function object, like Symbol.create, Symbol.iterator, etc. To use them, do something like:
```
obj[Symbol.iterator] = function(){ /*..*/ };
```
Note: Symbols are not objects, they are simple scalar primitives.


###37. Native Prototypes
Each of the built-in native constructors has its own .prototype object -- Array.prototype, String.prototype, etc.

These objects contain behavior unique to their particular object subtype.

Note: By documentation convention, String.prototype.XYZ is shortened to String#XYZ, and likewise for all the other .prototypes.

All functions have access to apply(..), call(..), and bind(..) because Function.prototype defines them.

As you can see, Function.prototype is a function, RegExp.prototype is a regular expression, and Array.prototype is an array. Interesting and cool, huh?

JavaScript provides object wrappers around primitive values, known as natives (String, Number, Boolean, etc). These object wrappers give the values access to behaviors appropriate for each object subtype (String#trim() and Array#concat(..)).

If you have a simple scalar primitive value like "abc" and you access its length property or some String.prototype method, JS automatically "boxes" the value (wraps it in its respective object wrapper) so that the property/method accesses can be fulfilled.


###38. Coercion (JavaScript Type Conversion)
Converting a value from one type to another is often called "type casting," when done explicitly, and "coercion" when done implicitly (forced by the rules of how a value is used).

Note: It may not be obvious, but JavaScript coercions always result in one of the scalar primitive (see Chapter 2) values, like string, number, or boolean. There is no coercion that results in a complex value like object or function. 

Another way these terms are often distinguished is as follows: "type casting" (or "type conversion") occur in statically typed languages at compile time, while "type coercion" is a runtime conversion for dynamically typed languages.

However, in JavaScript, most people refer to all these types of conversions as coercion, so the way I prefer to distinguish is to say "implicit coercion" vs. "explicit coercion."

The difference should be obvious: "explicit coercion" is when it is obvious from looking at the code that a type conversion is intentionally occurring, whereas "implicit coercion" is when the type conversion will occur as a less obvious side effect of some other intentional operation.

For example, consider these two approaches to coercion:
```
var a = 42;

var b = a + "";         // implicit coercion

var c = String( a );    // explicit coercion
```

###39. JS Syntax / Grammer
Put simply, the console is just reporting the statement's completion value.

Function arguments have an interesting relationship to their formal declared named parameters. Specifically, the arguments array has a number of gotchas of leaky abstraction behavior if you're not careful. Avoid arguments if you can, but if you must use it, by all means avoid using the positional slot in arguments at the same time as using a named parameter for that same argument.


###40. JS Event Loop and Job Queue (event means async function invocations)
So, the best way to think about this that I've found is that the "Job queue" is a queue hanging off the end of every tick in the event loop queue. Certain async-implied actions that may occur during a tick of the event loop will not cause a whole new event to be added to the event loop queue, but will instead add an item (aka Job) to the end of the current tick's Job queue.

It's kinda like saying, "oh, here's this other thing I need to do later, but make sure it happens right away before anything else can happen."

Or, to use a metaphor: the event loop queue is like an amusement park ride, where once you finish the ride, you have to go to the back of the line to ride again. But the Job queue is like finishing the ride, but then cutting in line and getting right back on.

A Job can also cause more Jobs to be added to the end of the same queue. So, it's theoretically possible that a Job "loop" (a Job that keeps adding another Job, etc.) could spin indefinitely, thus starving the program of the ability to move on to the next event loop tick. This would conceptually be almost the same as just expressing a long-running or infinite loop (like while (true) ..) in your code.

Jobs are kind of like the spirit of the setTimeout(..0) hack, but implemented in such a way as to have a much more well-defined and guaranteed ordering: later, but as soon as possible.

A JavaScript program is (practically) always broken up into two or more chunks, where the first chunk runs now and the next chunk runs later, in response to an event. Even though the program is executed chunk-by-chunk, all of them share the same access to the program scope and state, so each modification to state is made on top of the previous state.

Whenever there are events to run, the event loop runs until the queue is empty. Each iteration of the event loop is a "tick." User interaction, IO, and timers enqueue events on the event queue.

At any given moment, only one event can be processed from the queue at a time. While an event is executing, it can directly or indirectly cause one or more subsequent events.

Concurrency is when two or more chains of events interleave over time, such that from a high-level perspective, they appear to be running simultaneously (even though at any given moment only one event is being processed).

It's often necessary to do some form of interaction coordination between these concurrent "processes" (as distinct from operating system processes), for instance to ensure ordering or to prevent "race conditions." These "processes" can also cooperate by breaking themselves into smaller chunks and to allow other "process" interleaving.


###41. Events (Async function invocations) / Callbacks
In all these cases, the function is acting as a "callback," because it serves as the target for the event loop to "call back into" the program, whenever that item in the queue is processed.

As you no doubt have observed, callbacks are by far the most common way that asynchrony in JS programs is expressed and managed. Indeed, the callback is the most fundamental async pattern in the language.

```
// A
setTimeout( function(){
    // C
}, 1000 );
// B
```

You might have caught yourself and self-edited to: "Do A, setup the timeout for 1,000 milliseconds, then do B, then after the timeout fires, do C." That's more accurate than the first version. 

Callbacks are the fundamental unit of asynchrony in JS. But they're not enough for the evolving landscape of async programming as JS matures.

First, our brains plan things out in sequential, blocking, single-threaded semantic ways, but callbacks express asynchronous flow in a rather nonlinear, nonsequential way, which makes reasoning properly about such code much harder. Bad to reason about code is bad code that leads to bad bugs.

We need a way to express asynchrony in a more synchronous, sequential, blocking manner, just like our brains do.

Second, and more importantly, callbacks suffer from inversion of control in that they implicitly give control over to another party (often a third-party utility not in your control!) to invoke the continuation of your program. This control transfer leads us to a troubling list of trust issues, such as whether the callback is called more times than we expect.


###42. JS Promises

Note: Because a Promise is externally immutable once resolved, it's now safe to pass that value around to any party and know that it cannot be modified accidentally or maliciously. This is especially true in relation to multiple parties observing the resolution of a Promise. It is not possible for one party to affect another party's ability to observe Promise resolution. 

Promises are an easily repeatable mechanism for encapsulating and composing future values.

As we just saw, an individual Promise behaves as a future value. But there's another way to think of the resolution of a Promise: as a flow-control mechanism -- a temporal this-then-that -- for two or more steps in an asynchronous task.

Note: Another beneficial side effect of wrapping Promise.resolve(..) around any function's return value (thenable or not) is that it's an easy way to normalize that function call into a well-behaving async task. If foo(42) returns an immediate value sometimes, or a Promise other times, Promise.resolve( foo(42) ) makes sure it's always a Promise result. And avoiding Zalgo makes for much better code.

Promises are a pattern that augments callbacks with trustable semantics, so that the behavior is more reason-able and more reliable. By uninverting the inversion of control of callbacks, we place the control with a trustable system (Promises) that was designed specifically to bring sanity to our async.


###43. Promise Chain Flow
We've hinted at this a couple of times already, but Promises are not just a mechanism for a single-step this-then-that sort of operation. That's the building block, of course, but it turns out we can string multiple Promises together to represent a sequence of async steps.

The key to making this work is built on two behaviors intrinsic to Promises:

Every time you call then(..) on a Promise, it creates and returns a new Promise, which we can chain with.
Whatever value you return from the then(..) call's fulfillment callback (the first parameter) is automatically set as the fulfillment of the chained Promise (from the first point).
###44. JS Generator
A generator is a special kind of function that can start and stop one or more times, and doesn't necessarily ever have to finish. 

In other words, yield caused a value to be sent out from the generator during the middle of its execution, kind of like an intermediate return.

=================================================
=================================================

##Up and Going

###Chapter 2.

###1. JS Typed Values 
Javascript has only typed Values, not typed variables. Only values have types in Javascript, and variables are just simpe containers for those values.


###2. JS 7 Built-in Types 
There are 6 (7 as of ES6 symbol) built-in types: number, string, boolean, null, undefined, object, symbol(ES6).


###3. typeof Operator
typeof operator examine a value and tell you what type the value is (the type of value that's currently in a variable), not the varaible as variables don't have types. The return value of typeof operation is one of the 6 (7 as of ES6) string values. 


###4. 'Undefined' variable
var a = undefined. A variable can get to this 'undefined' value state in several different ways, including functions that return no values, and usage of the void operator. 


###5. Dot notation or bracket notation
Properties of an object can either be accessed via dot notation (i.e. obj.a) or bracket notation (i.e. obj["a"]). The [] notation requires either a variable or a string literal (which needs to be wrapped in ".." or '..'). Bracket notation is useful if you have a property name that has special characters in it, like obj["hello world!"].


###6. Subtypes: Array & Function
Array and functions should be thought of more like subtypes -- specialized version of object type, rather than being proper built-in types (number, string, boolean, null, undefined, object, symbol).


###7. Array vs Object 
The best and most natural approach is to use arrays for numerically positioned values and use object for named properties. 


###8. JS Explicit and Implicit Coercion
Coercion comes in two forms in JS: explicit and implicit. Explicit coercion is simply that you can see obviously from the code that a conversion from one type to another will occur, whereas implicit coercion is when the type conversion can happen as more of a non-obvious side effect of some other operation. 


###9. Truthy and Falsy. 
The specific list of "falsy" values in Javascript is as follows:

```
"" (empty string)
0, -0, NaN (invalid number)
null, undefined
false.
```
Any value that's not on this "falsy" list is "truthy".

###10. Equality:
Equality: difference between == and === being that == checks for value equality, and === checks for both value and type equality. However, this is inaccurate. The proper way to characterize them is that == checks for value equality with coercion allowed, and === checks for value equality without allowing coercion, and === is often called 'strict equality' for this reason. 


###11. Inequality: 
```
var a = 41; var b = "42"; var c = "43"
a < b; // true   b < c; // true
```
Section 11.8.5 of ES5 specification says that if both values in the < comparison are strings, as it is with b < c, the comparison is made lexicographically (aka alphabetically like a dictionary). But if one or both is not a string, as it is with a < b, then both values are coerced to be numbers, so a typical numeric comparison occurs. 


###12. Variable -- Nested Scopes
When you declare a variable, it is available anywhere in that scope, as well as any lower/inner scopes. If you try to access a variable's value in a scope where it's not available, you'll get a ReferenceError thrown. 

###13. ES 6: let. 
ES6 lets you declare variables to belong to individual blocks (pairs of {...}), using the let keyword. 

###14. Function as values: 
function foo() {...}. Though it may not seem obvious from the syntax, foo is basically just a variable in the outer enclosing scope that's given a reference to the function being declared. That is, the function itself ia a value, just like 42 or [1, 2, 3] would be. Not only can you pass a value (argument) to a function, but a function itself can be a value that's assigned to variables, or passed to or returned from other functions. 

Consider: var foo = function(){...} var x = function bar(){...}

###15. IIEFs: Immediately Invoked Function Expressions
```
(function IIFE(){
	console.log("Hello World!");
})();
```
The outer (...) surrounds the (function IIFE(){...}) function expression is just a nuance of JS grammar needed to prevent it from being treated as a normal function declaration.

The final () on the end of the expression -- the })(); line -- is what actually executes the function expression referenced immediately before it. 

(function IIEF(){...})(); (function IIEF(){...}) is the function expression, and () executes it.

Using IIFE we can declare private variable scope that won't affect surrounding code outside the IIEF. 

var x = (function IIFE(){
	return 42;
})();

###16. Closure: 
you can think of 'closure' as a way to 'remember' and continue to access a function's scope (its variables) even once the function has finished running. 

Consider:
```
function makeAdder(x){
	// parmater 'x' is an inner variable

	// inner function 'add()' uses 'x', so it has a 'closure' over it;
	function add(y){
		return y + x;
	};

	return add;
}
```
The reference to the inner add(...) function that gets returned with each call to the outer makeAdder(..) is able to remember whatever 'x' value was passed in to makeAdder(..).
```
var plusOne = makeAdder(1);
// 'plusOne' gets a reference to the inner 'add(..)' function with closure over the 'x' paramter of the outer 'makeAdder(1)';

var pulsTen = makeAdder(10);
// 'plusTen' gets a reference to the inner 'add(..)' function with closure over the 'x' parameter of the outer 'makeAdder(..)';

plusOne( 3 ); // 4 <-- 1 + 3;
plusOne( 41 ); // 42 <-- 1 + 41;

plusTen( 13 ); // 23 <-- 10 + 13;
```
More on how this code works:

When we call makeAdder(1), we get back a reference to its inner add(..) that remembers x as 1. We call this function reference plusOne(..).
When we call makeAdder(10), we get back another reference to its inner add(..) that remembers x as 10. We call this function reference plusTen(..).
When we call plusOne(3), it adds 3 (its inner y) to the 1 (remembered by x), and we get 4 as the result.
When we call plusTen(13), it adds 13 (its inner y) to the 10 (remembered by x), and we get 23 as the result.


###17. Modules:
The most common usage of closure in JS is the module pattern. Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as a public API that is accessible from the outside.
```
function User(){
	var username, password;

	function doLogin(user, pw){
		username = user;
		password = pw;

		// do the rest of the login work/logic;
	}

	var publicAPI = {
	login: doLogin
	}

	return publicAPI;
}

// create a 'User' module instance
var fred = User(); // executing User() creates an instance of the User module, -- a whole new scope is created, and thus a whole new copy of each of these inner variables/functions.

fred.login('Fred', 'password');
```

###18. "this" identifier:
If a function has a "this" reference inside it, that "this" reference usually points to an object. But which object it points to depends on how the function was called.


###19. Prototypes:
When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.
The internal prototype reference linkage from one object to its fallback happens at the time the object is created. The simplest way to illustrate it is with a built-in utility called Object.create(..).

But a more natural way of applying prototypes is a pattern called "behavior delegation," where you intentionally design your linked objects to be able to delegate from one to the other for parts of the needed behavior.

The this keyword is dynamically bound based on how the function in question is executed, and it turns out there are four simple rules to understand and fully determine this binding.

Closely related to the this keyword is the object prototype mechanism, which is a look-up chain for properties, similar to how lexical scope variables are found.


====================================================================================
====================================================================================

##Scope and Closures

##Chatper 1 What is Scope?

###1. LHS Lookup and RHS Lookup

Engine performs "LHS" look up and "RHS" look up for a variable.
In other words, an LHS look-up is done when a variable appears on the left-hand side of an assignment operation, and an RHS look-up is done when a variable appears on the right-hand side of an assignment operation.

An RHS look-up is indistinguishable, for our purposes, from simply a look-up of the value of some variable, whereas the LHS look-up is trying to find the variable container itself, so that it can assign. In this way, RHS doesn't really mean "right-hand side of an assignment" per se, it just, more accurately, means "not left-hand side".

By contrast:

a = 2;
The reference to a here is an LHS reference, because we don't actually care what the current value is, we simply want to find the variable as a target for the = 2 assignment operation.

Note: LHS and RHS meaning "left/right-hand side of an assignment" doesn't necessarily literally mean "left/right side of the = assignment operator". There are several other ways that assignments happen, and so it's better to conceptually think about it as: "who's the target of the assignment (LHS)" and "who's the source of the assignment (RHS)".

Consider this program, which has both LHS and RHS references:
```
function foo(a) {
    console.log( a ); // 2
}

foo( 2 );
```
The last line that invokes foo(..) as a function call requires an RHS reference to foo, meaning, "go look-up the value of foo, and give it to me." Moreover, (..) means the value of foo should be executed, so it'd better actually be a function!

There's a subtle but important assignment here. Did you spot it?

You may have missed the implied a = 2 in this code snippet. It happens when the value 2 is passed as an argument to the foo(..) function, in which case the 2 value is assigned to the parameter a. To (implicitly) assign to parameter a, an LHS look-up is performed.

There's also an RHS reference for the value of a, and that resulting value is passed to console.log(..). console.log(..) needs a reference to execute. It's an RHS look-up for the console object, then a property-resolution occurs to see if it has a method called log.

Finally, we can conceptualize that there's an LHS/RHS exchange of passing the value 2 (by way of variable a's RHS look-up) into log(..). Inside of the native implementation of log(..), we can assume it has parameters, the first of which (perhaps called arg1) has an LHS reference look-up, before assigning 2 to it.


###2. We said that Scope is a set of rules for looking up variables by their identifier name. 

Scope is the set of rules that determines where and how a variable (identifier) can be looked-up. This look-up may be for the purposes of assigning to the variable, which is an LHS (left-hand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (right-hand-side) reference.

LHS references result from assignment operations. Scope-related assignments can occur either with the = operator or by passing arguments to (assign to) function parameters.

The JavaScript Engine first compiles code before it executes, and in so doing, it splits up statements like var a = 2; into two separate steps:

First, var a to declare it in that Scope. This is performed at the beginning, before code execution.

Later, a = 2 to look up the variable (LHS reference) and assign to it if found.

Both LHS and RHS reference look-ups start at the currently executing Scope, and if need be (that is, they don't find what they're looking for there), they work their way up the nested Scope, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don't.

Unfulfilled RHS references result in ReferenceErrors being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [^note-strictmode]), or a ReferenceError (if in "Strict Mode" [^note-strictmode]).

Now, if a variable is found for an RHS look-up, but you try to do something with its value that is impossible, such as trying to execute-as-function a non-function value, or reference a property on a null or undefined value, then Engine throws a different kind of error, called a TypeError.

ReferenceError is Scope resolution-failure related, whereas TypeError implies that Scope resolution was successful, but that there was an illegal/impossible action attempted against the result.


=================================================================================
=================================================================================

##Scope and Closure
###Chapter 2: Lexical Scope

###1. Scope look-up
Scope look-up stops once it finds the first match. The same identifier name can be specified at multiple layers of nested scope, which is called "shadowing" (the inner identifier "shadows" the outer identifier). Regardless of shadowing, scope look-up always starts at the innermost scope being executed at the time, and works its way outward/upward until the first match, and stops.


###2. Note: Global variables
Global varialbe are also automatically properties of the global object (window in browsers, etc.), so it is possible to reference a global variable not directly by its lexical name, but instead indirectly as a property reference of the global object.

window.a
This technique gives access to a global variable which would otherwise be inaccessible due to it being shadowed. However, non-global shadowed variables cannot be accessed.


###3. Function lexical scope
No matter where a function is invoked from, or even how it is invoked, its lexical scope is only defined by where the function was declared.

The lexical scope look-up process only applies to first-class identifiers, such as the a, b, and c. If you had a reference to foo.bar.baz in a piece of code, the lexical scope look-up would apply to finding the foo identifier, but once it locates that variable, object property-access rules take over to resolve the bar and baz properties, respectively.


=================================================================================
=================================================================================

##Scope and Closure
###Chapter 3: Function vs. Block Scope

###1. Scope form Functions
It doesn't matter where in the scope a declaration appears, the variable or function belongs to the containing scope bubble, regardless. 

Function scope encourages the idea that all variables belong to the function, and can be used and reused throughout the entirety of the function (and indeed, accessible even to nested scopes).


###2. "Principle of Least Privilege" 
Principle of Least Privilege [^note-leastprivilege], also sometimes called "Least Authority" or "Least Exposure". This principle states that in the design of software, such as the API for a module/object, you should expose only what is minimally necessary, and "hide" everything else.


















