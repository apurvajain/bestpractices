# Currying

Currying refers to the process of transforming a function with multiple arity into the same function with less arity.
The curried effect is achieved by binding some of the arguments to the first function invoke, so that those values are fixed for the next invocation. Here’s an example of what a curried function looks like:

```
var babyAnimals = function(animal1) {
    return function(animal2) {
        return `I love ${animal1} and ${animal2}`;
    };
}

var babyKoala = babyAnimals('Koala');
console.log(babyKoala('Monkey'))
console.log(babyAnimals('cats')('dogs'))
```
