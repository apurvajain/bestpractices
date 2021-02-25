# JavaScript Objects

## Understanding Objects
In real life, a car is an object.
- A car has properties like `weight` and `color`, and methods like `start` and `stop`.
- All cars have the same properties, but the property values differ from car to car.
- All cars have the same methods, but the methods are performed at different times.

You have already learned that JavaScript variables are containers for data values.
This code assigns a simple value (Fiat) to a variable named car:
```
var car = 'Tesla';
```
### Objects are variable too. But objects can contain many values.

```
var car = {type:'Fiat', model:'500', color:'white'};
```
The values are written as **name:value** pairs (name and value separated by a colon).

You define (and create) a JavaScript object with an object literal:
```
var person = {
  firstName:'John',
  lastName:'Doe',
  age:50,
  eyeColor:'blue'
};
```
Spaces and line breaks are not important. An object definition can span multiple lines:

```
var person = {
  firstName: 'John',
  lastName: 'Doe',
  age: 50,
  eyeColor: 'blue'
};
```

## Object Properties
The **name:values** pairs in JavaScript objects are called properties:
| Property                     | Property Value   |
| ---------------------------- | ---------------- |
| firstName                    | Joe              |
| lastName                     | Biden            |
| age                          | 78               |

### Accessing Object Properties
You can access object properties in two ways:
1. **Dot Notation**
```
objectName.propertyName
```

2. **Square Bracket Notation**
```
objectName['propertyName']
```


## Object Methods
Objects can also have methods.

Methods are actions that can be performed on objects.

Methods are stored in properties as function definitions.

| Property       | Property Value                                             |
| -------------- | ---------------------------------------------------------- |
| firstName      | Joe                                                        |
| lastName       | Biden                                                      |
| age            | 78                                                         |
| getFullName    | function() {return this.firstName + ' ' + this.lastName;}  |

### A method is a function stored as a property.

```
var person = {
  firstName: 'John',
  lastName : 'Doe',
  id       : 5566,
  fullName : function() {
    return this.firstName + ' ' + this.lastName;
  }
};
```

### The this Keyword
In a function definition, this refers to the 'owner' of the function.

In the example above, this is the person object that 'owns' the fullName function.

In other words, this.firstName means the firstName property of this object.

### Accessing Object Properties
You access an object method with the following syntax:
```
objectName.methodName()

or
objectName.['methodName']()
```

When accessed without the `()` parentheses it will return the function definition:

```
name = person.fullName;
var fullName = name();
```

### Object.keys

```
var person = {
  firstName: 'John',
  lastName : 'Doe',
  id       : 5566,
};

// prints ['firstName', 'lastName', 'id']
console.log(Object.keys(person));
```

### Object.values

```
var person = {
  firstName: 'John',
  lastName : 'Doe',
  id       : 5566,
};

// prints ['John', 'Doe', 5566]
console.log(Object.values(person));
```
