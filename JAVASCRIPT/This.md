# Javascript: call(), apply() and bind()

In Object Oriented JS we learned that **in JS, everything is an object**.
Because everything is an object, we came to understand that we could set and access additional properties to functions.

**The JavaScript `this` keyword refers to the object it belongs to.**

Basically, `this` keyword in Javascript always refers to the 'owner' / 'receiver' of the function that is being executed or called. If no explicit owner / receiver is defined, then the top most owner, the window object, is referenced.

It has different values depending on where it is used:
- In a method, `this` refers to the owner object.
- Alone, `this` refers to the global object.
- In a function, `this` refers to the global object.
- In a function, in strict mode, `this` is undefined.
- In an event, `this` refers to the element that received the event.
- Methods like call(), and apply() can refer `this` to any object.

```
// normal function
var exampleThis = function () {
  return this;
};
console.log(exampleThis() === global);  // returns true

example().a = 10; // *polluting the global!*
console.log(a); // 10

var strictThisExample = function () {
  'use strict';
  return this;
};
console.log(strictThisExample()); //  returns undefined

var ConstructorThisExample = function () {
  console.log(this);
};
var obj = new ConstructorThisExample(); // {}
```

## call(), apply(), bind() - A new hope
Up until now we have treated functions as objects that are composed of a name (optional, can also be an anonymous function) and the code it executes when it is invoked. But that isn’t the entire truth. As a truth loving person, I must let you know that a function actually looks closer to the following:

Function:
- NAME Optional, can be anonymous
- call()
- bind()
- apply()
- code


## Function.prototype.call()

Each function calls gets its own `this` binding - But call() gives us a way to "borrow" a method from one object to use for another by specifyng what that `this` binding would be. .call() attaches `this` into function and executes the function immediately.

```
var steveJobs = {
    mission: "Change the world",
    describe: function() {
        console.log(this.mission);
    }
};
steveJobs.describe(); // Outputs: "Change the world",

// Now borrow the describe function with call() method
var steveWoz = {
    mission: "Do great engineering"
};
steveJobs.describe.call(steveWoz); // Outputs: "Do great engineering"
```

One of the features of JavaScript is that **functions are first-class objects** - This means that functions can be assigned to a variable and passed around.
And if functions are objects and objects can have methods, then functions can have methods. Thus `call` is such a method of functions and we already saw above how its used to borrow methods from other Objects and set this value explicitly.

It also allows me to call utility methods outside the scope, but still bring the local scope into the method.

```
func.call([thisArg[, arg1, arg2, ...argN]])
```

- `thisArg`: The value to use as `this` when calling function
- `arg1, arg2, ...argN`
- Returns - The result of calling the function with the specified this value and arguments.


### Uscases
- Common usecase of call as a Function-borrowing tool

```
var Website = {
  updatePageView: function() {
    for(var i = 0; i < arguments.length; i++) {
      this.pageViewNumber += arguments[i];
    }
    return this.pageViewNumber;
  }
}

var profilePage = {
  name: 'My Web page',
  pageViewNumber: 0
};

Website.updatePageView.call(profilePage, 5, 6, 7, 8);

console.log(Website.updatePageView.call(profilePage));
// Output => 26
```

If I wanted to use apply method instead of call in the above example, the argument passed need to be a single array of values. So the code will need to modified like below..
Website.updatePageView.apply(profilePage, [5, 6, 7, 8]);
The numbers 5, 6, 7, 8 may come from any number of sources like an API or whatever.


call() and apply() serve the exact same purpose. The only difference between how they work is that call() expects all parameters to be passed in individually, whereas apply() expects an array of all of our parameters.
```
var pokemon = {
    firstname: 'Pika',
    lastname: 'Chu ',
    getPokeName: function() {
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
    }
};

var pokemonName = function(snack, hobby) {
    console.log(this.getPokeName() + ' loves ' + snack + ' and ' + hobby);
};

pokemonName.call(pokemon,'sushi', 'algorithms'); // Pika Chu  loves sushi and algorithms
pokemonName.apply(pokemon,['sushi', 'algorithms']); // Pika Chu  loves sushi and algorithms

```

It’s important to remember that using .call() or .apply() actually invokes my function, so instead of doing this:
`myFunction(); // invoke myFunction`
I wil have to let .call() handle it and chain the method:
`myFunction.call(scope); // invoke myFunction using .call()`

## Function.prototype.apply()

The limitations of `call()` quickly become apparent when you want to write code that doesn’t (or shouldn’t) know the number of arguments that the functions need.
So that’s where apply comes in — the second argument needs to be an array, which is unpacked into arguments that are passed to the called function. e.g. finding the maximum value from an array.
We can not directly use, Math.min or Math.max as we will get NaN.

```
const nums = [1, 2, 3]
console.log(Math.min(nums)); // NaN
console.log(Math.max(nums)); // NaN
```

That is because Math.min or Math.max expect distinct variables and not an array. Hence we use .apply()

```
var getMaxNumberFromArray = function (arr) {
  return Math.max.apply(null, arr);
};
var getMinNumberFromArray = function (arr) {
  return Math.min.apply(null, arr);
};
```

```
func.apply(thisArg, [ argsArray])
```

- `thisArg` The value of this provided for the call to func
- `argsArray` An array-like object, specifying the arguments with which func should be called, or null or undefined if no arguments should be provided to the function
- `returns` The result of calling the function with the specified this value and arguments


**Exercise #1**
Using apply to append an array to another
<!--
const array = ['a', 'b'];
const elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]
-->

## Function.prototype.bind()

Now that we know that calling a function is actually applying a set of arguments to a function, is it possible to pass just a few of the arguments, not all of them?
This Partial functions is a functional programming concept. A partial function takes a function and fewer arguments than normal. It returns a function that takes the remaining arguments. When called the returned function call the original function with both sets of arguments.

`bind()` is particularly powerful in that, as beyond making our code more concise, with it we can make partial functions, also called function Currying. It refers to the process of fixing a number of arguments to a function, producing another function of smaller arity.

```
var pokemon = {
    firstname: 'Pika',
    lastname: 'Chu ',
    getPokeName: function() {
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
    }
};

var pokemonName = function() {
    console.log(this.getPokeName() + 'I choose you!');
};

var logPokemon = pokemonName.bind(pokemon); // creates new object and binds pokemon. 'this' of pokemon === pokemon now

logPokemon(); // 'Pika Chu I choose you!'
```

Let’s break it down. When we use the bind() method:
the JS engine is creating a new pokemonName instance and binding pokemon as its this variable. It is important to understand that it copies the pokemonName function.

After creating a copy of the pokemonName function it is able to call logPokemon(), although it wasn’t on the pokemon object initially. It will now recognizes its properties (Pika and Chu) and its methods.

And the cool thing is, after we bind() a value we can use the function just like it was any other normal function. We could even update the function to accept parameters, and pass them like so:

```
var pokemon = {
    firstname: 'Pika',
    lastname: 'Chu ',
    getPokeName: function() {
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
    }
};

var pokemonName = function(snack, hobby) {
    console.log(this.getPokeName() + 'I choose you!');
    console.log(this.getPokeName() + ' loves ' + snack + ' and ' + hobby);
};

var logPokemon = pokemonName.bind(pokemon); // creates new object and binds pokemon. 'this' of pokemon === pokemon now

logPokemon('sushi', 'algorithms'); // Pika Chu  loves sushi and algorithms
```