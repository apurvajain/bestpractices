# Basics of Naming Conventions for Developers

## Why is it important?
- **Better Communication**
Building a product is always a *Team Effort* which requires making sure that everyone understands the *names* that we name, we got to name them clearly.

- **Easy Management**
Developers don't hesitate to touch the code of peer members due to difference in coding conventions.

- **Easy Reviewing**
Consistent code conventions makes reviewing things easier as we don’t have to find out what something means.

- **Easy on-boarding**
For new developer onboarding, coding conventions allow them to easily grasp the inners of code by simply looking at the code.

## How to write meaningful names?

The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

- **Use intention revealing names**
Variable name must define the exact explanation of its content so it can documentate itself in case of no documentation.
    ```
    var d // elapsed time in days
    ```

    vs

    ```
    var elapsedTimeInDays
    ```

- **Use pronounceable names**
    ```
    class NwClmDmg {
        private _lyt = 'dashboard';
        private Date _modymdhms;
    }
    ```

    vs

    ```
    class NewClaimDamage {
        private _layout = 'dashboard';
        private Date _modificationTimestamp;
    }
    ```

- **Use searchable names**
If a variable or constant might be seen or used in multiple places in a body of code, it is imperative to give it a search-friendly name.
    ```
    for (int item = 0; item < 34; item++) {
        s += (t[item] * 4) / 5;
    }
    ```

    vs

    ```
    var realDaysPerIdealDay = 4;
    const WORK_DAYS_PER_WEEK = 5;
    var sum = 0;
    for (var item = 0; item < NUMBER_OF_TASKS; item++) {
        var realTaskDays = taskEstimate[item] * realDaysPerIdealDay;
        var realTaskWeeks = (realTaskDays / WORK_DAYS_PER_WEEK);
        sum += realTaskWeeks;
    }
    ```

- **Use one consistent language**
Decide and use one natural language for naming, e.g. using mixed English and Bahasa names will be inconsistent and unreadable.

- **There's no limit to the length of the variable name**
Use short enough and long enough variable names in each scope of code.
> Although short, cryptic variable names are to be shunned in favor of longer, descriptive names, that doesn't mean you should be using entire sentences. Extremely long names are inefficient because they make code dirty, hard-to-read-write and leading to possibilty of typo.


Before we get into defining conventions, here are several delimiting conventions commonly used in code:

- **Snakecase** Words are delimited by an underscore
    ```
    first_name
    ```

- **Pascalcase** Words are delimited by capital letters.
    ```
    FirstName
    ```

- **Camelcase** Words are delimited by capital letters, except the initial word.
    ```
    firstName
    ```

- **Hungarian Notation** This notation describes the variable type or purpose at the start of the variable name, followed by a descriptor that indicates the variable’s function. The Camelcase notation is used to delimit words.
    ```
    arrCompanyGroup     // Array called "Company Group"
    sUserName           // String called "User Name"
    iRandomSeed         // Integer called "Random Seed"
    ```

## Naming Conventions

Consistency and readability are key ideas that should be utilized in the naming of variables. Regardless of how you choose to name your variables, always ensure that your naming conventions are consistent throughout the code. Consistency allows others to more easily understand your code.

### Naming Conventions: Variable
Most often variables are declared with **camelCase**.

```
var firstName = 'John';
```

### Naming Conventions: Constant
Constants — intended to be non-changing variables are declared in **ALL CAPS**.

```
var SECONDS = 60;
```

### Naming Conventions: Boolean
A prefix like `is` , `are` , `has` helps developer to distinguish a boolean from another variable by just looking at it.

```
var isVisible = true;
var hasKey = false;
```

### Naming Conventions: Function
Functions names are written in **camelCase**. Always start your function name with a **"Verb"** which defines what that function is trying to do in conjunction with the name of the **"Entity"** being affected by this function.

```
getOrder()
fetchClaims()
deleteOrder()
connectToDatabase()
```

### Naming Conventions: Method
Like functions, methods names are written in **camelCase**. As class name itself depict "Entity" thus suffixing the "Entity" in a function name doesn’t make sense because it becomes self-explanatory in case of class methods.

```
class User {
  delete(id) {
    // do something
  }
}

var user = new User();
console.log(user.delete(2)); // delete the user with ID = 2
```

### Naming Conventions: Class
A class should be declared with **PascalCase** in its own file.

```
class User {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}

var user = new User('John', 'Doe');
```

### Naming Conventions: Component
Components are widely declared with **PascalCase** too. When a component gets used, it distinguishes itself from native HTML and web components, because its first letter is always written in uppercase.

```
function UserProfile(user) {
  return (
    <div>
      <span>First Name: {user.firstName}</span>
      <span>Last Name: {user.lastName}</span>
    </div>
  );
}

<div>
  <UserProfile
    user={{ firstName: 'John', lastName: 'Doe' }}
  />
</div>
```

### Naming Conventions: Private
Private and protected properties in a class MUST BE prefixed with a single underscore.

```
class User {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.name = _getName(firstName, lastName);
  }

  _getName(firstName, lastName) {
    return `${firstName} ${lastName}`;
  }
}

var me = new User('John', 'Doe');
console.log(me.name);
```

### Naming Conventions: Argument
Use self-explanatory argument label that indicates more expressively the intent of the argument.

```
function getRemainder(x, y) {

}
```

vs

```
function getRemainder(number, divisor) {

}
```

### Parentheses Style
Two commonly used conventions for parenthesis are: K&R Style Parentheses and  Allman Parentheses.

It is recommended to use K&R Style parentheses because they save one line and seem natural.

K&R Style Parentheses
```
if(isVisible) {
  // do something
}
```

Allman Parentheses
```
if(isVisible)
{
  // do something
}
```

### Consistency is the key!!
The key to following conventions is consistency. You MUST ensure that you define and stick to a particular set of conventions.