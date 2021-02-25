# JavaScript Recurion

Recursion in JavaScript is, simply put, the ability to call a function from within itself.

**Example #1**:
Infinite Recursion (do not try to run this code!)

```
function demo() {
    demo();
}
demo();
```

As you can see the function above(demo) is called/invoked. It then proceeds to run the instructions found within demo, which consists of another call/invocation of the function demo. This will call/invoke the function demo which will call/invoke the function demo, which will call/invoke the function demo… and this process will continue ad infinitum until either the page or the browser crashes.

This is **recursion**. The ability of the function `demo` to invoke the function `demo` from within itself.

**Example #2**:
Setting Up A Leave Event

When constructing a **recursive** function in JavaScript there must *always* be a **Leave Event**.

### What is a Leave Event?
It is any control statement that allows the function to exit the recursive loop. This can be an if statement, a ternary argument, a switch statement, etc.
An example of a simple countdown function:
```
function countDown(n) {
  console.log(n);
  if(n >= 1) countDown(n-1);
}

// print from 5 to 0
countDown(5);
```

**Example #3**:
A Real world problem - Implement Object "deep freeze"

Let’s take the objects. We know, variables defined with `const` cannot be reassigned. It is important that I used the word "reassigned" and not "changed" because objects, functions, and arrays cannot be reassigned, but they can be changed. Take this for example:
```
const obj = { name : "Bob" };
obj = { name = "Sam" } //this will throw an error
obj.name = "Sam" //no error
```

This is because the properties of the object can still be changed even if the variable can’t be reassigned to another value.

In JavaScript, Object has a static method called `freeze` that lets you "freeze" each of the properties of an object so that those properties can no longer be reassigned. Now we can do this:
```
const obj = { name : "Bob" };
Object.freeze(obj)
obj.name = "Sam" //This now throws an error
```
The gotcha with `Object.freeze` is that it only does a **shallow freeze**, which means it only freezes the properties of the object passed in. If one of the properties was assigned another object as it’s value, the properties in that object are not frozen, like this:
```
const obj =
   {
      name: "Bob"
      job = { title : "Worker" }
   };
Object.freeze(obj);
obj.job.title = "Manager" //no error
```

### Problem Statement:
What we need to do is write some type of “deep freeze” function. Luckily, nested objects are a perfect use case for using recursion.

Let’s think through the problem first:
1. Given a value, we want to determine if it is an object
2. If it is, we want to iterate over its properties and freeze them if they are objects
3. and then freeze the object itself
4. Otherwise, do nothing

Step #1 of our `deepFreeze` function is determining if the value passed into the function is an object and if it isn’t, do nothing.
```
function deepFreeze(obj){
  if(typeof obj !== 'object') return;
}
```

Step #2 is to iterate over the property values and freeze them if they are objects as well. This is made easy using the `Object.values` static method, which takes an object and returns an array of all the values.

Given that it is an array, we can then call the `forEach` method which takes a callback function that is called on each of the values.

Can you guess what function we are going to pass in as the callback? Exactly, the `deepFreeze` function is the perfect function to pass into it, like this:

```
function deepFreeze(obj){
  if(typeof obj !== 'object') return;

  Object.values(obj).forEach(deepFreeze)

}
```

Step #3 Now all we need to do is freeze the object itself and we are done:
```
function deepFreeze(obj){
  if(typeof obj !== 'object') return;

  Object.values(obj).forEach(deepFreeze)

  Object.freeze(obj);
}
```
Now we can do this and get the expected results:
```
const obj =
   {
      name: "Bob"
      job = { title : "Worker" }
   };
deepFreeze(obj);
obj.job.title = "Manager" //This now throws an error
```

Recursion can be hard to wrap your mind around, especially at first. When done right, recursion is a powerful tool that lets you write clean and elegant code that lets you do some amazing things.
<!-- 
```
// map
map = (_fn_, [_head_, ..._tail_]) _=>_ (
  head === undefined && tail.length < 1
    ? []
    : [fn(head), ...map(fn, tail)]
);

// filter
filter = (pred, [head, ...tail]) =>
  head === undefined
    ? []
    : pred(head)
    ? [head, ...filter(pred, tail)]
    : [...filter(pred, tail)];

// reduce
reduce = (fn, acc, [head, ...tail]) =>
  head === undefined ? acc : reduce(fn, fn(acc, head), tail);
``` -->