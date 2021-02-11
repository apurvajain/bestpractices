
"Good judgement is the result of experience ... Experience is the result of bad judgement." — Fred Brooks
# Builder Pattern applied to Unit Testing
If we are dealing with test driven development, it is a good practice having your unit tests well implemented, because in the future, as the software evolves, you will have to look into your tests, understand them, modify them to acomplish your job. An important aspect to achieve this is making your test data robust.

## The problem statement:
- **Voilates DRY principle:**
You should follow the DRY principle even in your test code, to reduce technical debt (and total size of the code). This especially means being careful with how many places you're instantiating types you're testing (or testing with).

- **Makes tests brittle**
The more duplication you have in this area, the more expensive it will be (in terms of time and effort) for a change to be made to the type you're instantiating.

- **Resistance to writing tests**
Creating test data for each test from scratch makes it time-consuming, boring and repetitive leading to band aids and even not writing tests.

## Possible solutions:

Usually in application code, objects are constructed in few places and all the information required by the constructor is at hand, having been provided by user input, obtained from a database query or received in a message for example. In tests, on the other hand, you have to provide all those constructor arguments every time you want to create an object, whether to test its behaviour or to create a value to use as input to the code being tested.

The code to create all those objects makes tests messy and hard to read and fills the tests with lots of unnecessary information that has nothing to do with the behaviour being tested. It also makes tests brittle: changes to the constructor arguments or the structure of the objects will break many tests.


### A. Static Helpers or Object Mother
An **Object Mother** is a set of factory methods that allow us creating similar objects in tests. In simplest terms, you should replace many instance of 'new' with a helper method like GetTestAddress().
```
// Object Mother
public class TestUsers {

    static public function aRegularUser() {
        return new User('John Smith', 'jsmith', 'qwerty', 'ROLE_USER');
    }

    static public function anAdminUser() {
        return new User('John Doe', 'jdoe', 'asdfgh', 'ROLE_ADMIN');
    }

    // other factory methods

}

// Test
$normalUser = TestUsers::aRegularUser();
$adminUser = TestUsers::anAdminUser();
```
- An Object Mother helps keep tests readable by moving the code that creates new objects out of the tests themselves and giving clear names to the objects being constructed.
- It also helps maintain the test data by gathering the code that creates new objects together into the Object Mother class and allowing it to be reused between tests.

### Limitations of Object Mother:
ObjectMother does not cope well with variability in the test data being created - Each time when a user with slightly different variation of data is required, new factory method is added, which makes that the Object Mother bloated, messy and hard to maintain.
- Programmers add new factory methods without refactoring, in which case the Object Mother becomes full of duplicated code
or
- Programmers refactor diligently, in which case the Object Mother becomes full of many, many fine-grained methods that each contain little more than a single new statement

```
public class TestUsers {

    static public function aRegularUser() {
        return new User('John Smith', 'jsmith', 'qwerty', 'ROLE_USER');
    }

    static public function anAdminUser() {
        return new User('John Doe', 'jdoe', 'asdfgh', 'ROLE_ADMIN');
    }

    static public function aSalesAgent() {
        return new User('John Mill', 'jmill', 'zxcvbn', 'ROLE_USER');
    }

    // other factory methods

}
```
<!----
The biggest problem you might come across with object mothers, is when you start to require lots of different variations of the same thing, where these variations need to be known at construction time. Your choice is to compromise the object model, making things mutable when they shouldn't be so you can customise more general example objects in the test methods, or create really long and silly method names:
-->

### B. Test Data Builder

However, if you need to have more fine-grained control over the instance, the Builder Pattern can be helpful.
The builder pattern is an object creation software design pattern - the intention of the builder pattern is to find a solution to the telescoping constructor anti-pattern.

Test Data Builder uses the Builder pattern to create objects in Unit Tests.

For each class you want to use in a test, create a Builder for that class that:

- Has an instance variable for each constructor parameter
- Initialises its instance variables to commonly used or safe values
- Has a `build` method that creates a new object using the values in its instance variables
- Has "chainable" public methods for overriding the values in its instance variables.

```
public class UserBuilder {

    private const DEFAULT_USERNAME = 'jdoe';
    private const DEFAULT_PASSWORD = 'qwerty';
    private const DEFAULT_ROLE = 'ROLE_USER';

    private $_username = DEFAULT_USERNAME;
    private $_password = DEFAULT_PASSWORD;
    private $_role = DEFAULT_ROLE;

    public function __construct() {
    }

    static public function aUser() {
        return new self();
    }

    public function withUsername($username) {
        $this->_username = $username;
        return $this;
    }

    public function withPassword($password) {
        $this->password = $password;
        return $this;
    }

    public function withNoPassword() {
        $this->password = null;
        return $this;
    }

    public function inUserRole() {
        $this->role = 'ROLE_USER';
        return $this;
    }

    public function inAdminRole() {
        $this->role = 'ROLE_ADMIN';
        return $this;
    }

    public function inRole($role) {
        $this->role = $role;
        return $this;
    }

    public function but() {
        return self::aUser()
                ->withUsername(username)
                ->withPassword(password)
                ->inRole(role);
    }

    public function build() {
        return new User($this->_username, $this->_password, $this->_role);
    }
}

// Working with the builder looks like this:

$userBuilder = UserBuilder::aUser()
                    ->withName('Apurva Jain')
                    ->withUsername('appi2393');

$defaultUser = $userBuilder.build();
$adminUser = $userBuilder->but()
                ->withNoPassword()
                ->inAdminRole()

$defaultUser = (new  UserBuilder())->build();

$customUser = (new  UserBuilder())
                ->withUsername('appi2393')
                ->withPassword('asdfgh')
                ->build();

$adminUser = (new UserBuilder())->but()
                ->withNoPassword()
                ->inAdminRole()
                ->build();
```

- Test Data Builder improves the readability of the test code because the parameters are clearly identified
```
TestUsers::aRegularUser(
    'jsmith',
    'qwerty',
    'ROLE_USER'
);
```
vs
```
(new  UserBuilder())
    ->withUsername('appi2393')
    ->withPassword('asdfgh')
    ->build();
```
- Eliminates the problem of having multiple factory methods for object variations
- Tests are isolated from those aspects of the objects' structure that have no bearing on the test
- TestDataBuilder pattern allows tests to specify only those parts of the objects that need to vary and use sensible defaults for those that are not relevant to the test.

## Object Mother and Test Data Builder combined


To summarize, these techniques helps us:
- **Reuse and find test data** by placing methods on the class as a extension methods. They are also fast because they are just code.
- **Minimize unintended consequences when something changes** by having the same data setup in as few methods as possible. If fields are added, renamed or changed there are not thousands of places that needs changing.
- **Make it easy to write tests** by having quick and easy to find methods. We know that resistance to writing tests leads to band aids and even not writing tests. We want to make it as easy as possible.
