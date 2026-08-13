# Day 03 — Python Object-Oriented Programming

## Objective

Understand Python's object-oriented programming model well enough to design, read, and maintain practical Python applications.

The focus is on understanding how objects hold state and behavior, how classes organize that behavior, and how OOP concepts can be applied in real projects.

### Topics Covered

- Classes and Objects
- `__init__`
- `self`
- Instance Attributes
- Instance Methods
- Class Attributes
- Instance vs Class Attribute Lookup
- Encapsulation
- Properties
- Getters and Setters
- Inheritance
- Method Overriding
- Polymorphism
- Composition
- "is-a" vs "has-a" relationships

---

# 1. Classes and Objects

A class defines the structure and behavior of objects.

```python
class Player:
    pass
```

Objects are instances created from the class.

```python
player1 = Player()
player2 = Player()
```

`player1` and `player2` are separate objects.

Therefore:

```python
player1 is player2
```

returns:

```text
False
```

The fact that two objects come from the same class does not make them the same object.

---

# 2. `__init__`

`__init__` is used to initialize an object's state when an instance is created.

```python
class Player:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Creating instances:

```python
player1 = Player("Alice", 25)
player2 = Player("Bob", 28)
```

creates separate state for each instance.

Conceptually:

```text
player1
├── name → "Alice"
└── age  → 25

player2
├── name → "Bob"
└── age  → 28
```

---

# 3. `self`

`self` refers to the current instance.

Example:

```python
class Player:
    def __init__(self, name):
        self.name = name

    def rename(self, new_name):
        self.name = new_name
```

When:

```python
player1.rename("Charlie")
```

is called, `player1` becomes the `self` argument.

Conceptually:

```python
Player.rename(player1, "Charlie")
```

Therefore:

```python
self.name = new_name
```

modifies the state of `player1`.

It does not modify other `Player` instances.

---

# 4. Instance Attributes

Instance attributes belong to individual objects.

```python
class Counter:
    def __init__(self):
        self.value = 0
```

Creating:

```python
a = Counter()
b = Counter()
```

creates separate `value` attributes.

If:

```python
a.value += 1
a.value += 1

b.value += 1
```

then:

```text
a.value → 2
b.value → 1
```

The objects do not share their instance state.

---

# 5. Instance Methods

Methods define behavior associated with objects.

```python
class Counter:
    def __init__(self):
        self.value = 0

    def increment(self):
        self.value += 1
```

Calling:

```python
a.increment()
```

operates on `a`.

Calling:

```python
b.increment()
```

operates on `b`.

The same method can operate on different instances because `self` identifies the current object.

---

# 6. Equality vs Identity for Custom Objects

Consider:

```python
class Player:
    def __init__(self, name):
        self.name = name

player1 = Player("Alice")
player2 = Player("Alice")
```

Both have the same attribute value:

```python
player1.name == player2.name
```

returns:

```text
True
```

But:

```python
player1 is player2
```

returns:

```text
False
```

They are two separate objects.

By default, separately created custom objects are not considered equal merely because their attributes contain the same values.

Equality behavior can be customized using `__eq__`.

```python
class Player:
    def __init__(self, name):
        self.name = name

    def __eq__(self, other):
        return self.name == other.name
```

Now two players with the same name can compare equal.

Important distinction:

```text
is  → identity
==  → equality
```

---

# 7. Class Attributes

A class attribute belongs to the class rather than being initialized independently for every instance.

```python
class Player:
    team = "RH"

    def __init__(self, name):
        self.name = name
```

Both instances can access:

```python
p1.team
p2.team
```

and initially both get:

```text
RH
```

Changing the class attribute:

```python
Player.team = "Team A"
```

makes both instances see the new class value, provided they do not have their own instance-level `team` attribute.

---

# 8. Instance Attribute vs Class Attribute

Consider:

```python
class Player:
    team = "RH"

    def __init__(self, name):
        self.name = name


p1 = Player("Alice")
p2 = Player("Bob")

p1.team = "Team B"
```

Now:

```python
print(p1.team)
print(p2.team)
print(Player.team)
```

outputs:

```text
Team B
RH
RH
```

`p1.team = "Team B"` creates an instance-level attribute on `p1`.

It does not modify `Player.team`.

Conceptually:

```text
Player
└── team = "RH"

p1
├── name = "Alice"
└── team = "Team B"

p2
└── name = "Bob"
```

---

# 9. Attribute Lookup

When accessing:

```python
p1.team
```

Python looks for the attribute on the instance and then through the class hierarchy if necessary.

The simplified lookup idea is:

```text
Instance
   ↓
Class
   ↓
Parent classes
```

Therefore, an instance-level attribute can override the value obtained from the class.

---

# 10. Encapsulation

Encapsulation means controlling how an object's internal state is accessed or modified and keeping the rules associated with that state within the object.

Example:

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance

    def deposit(self, amount):
        if amount > 0:
            self._balance += amount
```

The class can enforce rules before modifying its internal state.

For example:

```python
account.deposit(500)
```

is allowed.

While:

```python
account.deposit(-200)
```

does not modify the balance because the input fails validation.

---

# 11. Underscore Convention

Python does not enforce private attributes in the same way as some other languages.

A leading underscore is a convention:

```python
self._balance
```

communicates:

> This attribute is intended to be treated as an internal implementation detail.

It is still technically accessible.

---

# 12. Properties

A property allows attribute-like access while executing custom logic behind the scenes.

Example:

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance
```

Now:

```python
account.balance
```

looks like normal attribute access, but Python calls the property getter.

The actual stored value is:

```python
account._balance
```

while:

```python
account.balance
```

is the property interface.

---

# 13. Getter

A getter controls what happens when a property is read.

```python
@property
def balance(self):
    return self._balance
```

Therefore:

```python
account.balance
```

invokes the getter.

Conceptually:

```text
account.balance
      ↓
   getter
      ↓
return account._balance
```

---

# 14. Setter

A setter controls what happens when a property is assigned a new value.

```python
@balance.setter
def balance(self, value):
    if value < 0:
        raise ValueError("Balance cannot be negative")

    self._balance = value
```

Now:

```python
account.balance = 1500
```

runs through the setter.

The setter can validate the value before modifying `_balance`.

Therefore:

```python
account.balance = -200
```

raises:

```text
ValueError: Balance cannot be negative
```

The setter prevents the invalid state from being stored.

---

# 15. Getter vs Setter

The mental model:

```text
Reading:

account.balance
      ↓
   getter
      ↓
account._balance
```

Assigning:

```text
account.balance = 1500
      ↓
   setter
      ↓
 validation
      ↓
account._balance = 1500
```

A property therefore provides an attribute-like interface while allowing logic to be executed during access or assignment.

---

# 16. Inheritance

Inheritance allows a child class to reuse behavior from a parent class.

```python
class Model:
    def predict(self):
        print("Generic prediction")


class ClassificationModel(Model):
    pass
```

`ClassificationModel` inherits from `Model`.

Therefore:

```python
model = ClassificationModel()
model.predict()
```

can call the inherited `predict()` method.

Relationship:

```text
Model
  ↑
  │ inherits
  │
ClassificationModel
```

This represents an **"is-a" relationship**:

```text
ClassificationModel IS-A Model
```

---

# 17. Method Overriding

A child class can provide its own implementation of an inherited method.

```python
class Model:
    def predict(self):
        print("Generic prediction")


class ClassificationModel(Model):
    def predict(self):
        print("Classification prediction")


class RegressionModel(Model):
    pass
```

Now:

```python
classification = ClassificationModel()
regression = RegressionModel()

classification.predict()
regression.predict()
```

Output:

```text
Classification prediction
Generic prediction
```

`ClassificationModel` overrides the inherited `predict()` method.

`RegressionModel` does not define its own `predict()`, so it inherits the implementation from `Model`.

---

# 18. Inheritance Lookup

For:

```python
classification.predict()
```

Python first looks for `predict()` on the object's class.

If found, it uses that implementation.

If not found, Python looks through the parent classes.

For example:

```text
classification.predict()
        ↓
ClassificationModel
        ↓
predict() found
        ↓
use ClassificationModel.predict()
```

For `regression.predict()`:

```text
regression.predict()
        ↓
RegressionModel
        ↓
predict() not found
        ↓
Model
        ↓
predict() found
```

---

# 19. Polymorphism

Polymorphism allows the same interface or method call to result in different behavior depending on the actual object.

Example:

```python
class Model:
    def predict(self):
        return "Generic"


class ClassificationModel(Model):
    def predict(self):
        return "Classification"


class RegressionModel(Model):
    def predict(self):
        return "Regression"
```

Now:

```python
models = [
    Model(),
    ClassificationModel(),
    RegressionModel(),
    ClassificationModel()
]

for model in models:
    print(model.predict())
```

Output:

```text
Generic
Classification
Regression
Classification
```

The code uses the same operation:

```python
model.predict()
```

but each object provides its appropriate behavior.

---

# 20. Inheritance + Polymorphism

These concepts often work together.

Inheritance provides the common structure:

```text
Model
├── ClassificationModel
└── RegressionModel
```

Polymorphism allows the caller to use a common interface:

```python
model.predict()
```

without needing to know the exact model type at the call site.

---

# 21. Composition

Composition represents a **"has-a" relationship**.

Example:

```python
class Engine:
    def start(self):
        print("Engine started")


class Car:
    def __init__(self):
        self.engine = Engine()

    def start(self):
        self.engine.start()
        print("Car started")
```

A `Car` does not inherit from `Engine`.

Instead, the `Car` contains an `Engine` object.

```text
Car
│
└── engine → Engine object
```

Calling:

```python
car = Car()
car.start()
```

produces:

```text
Engine started
Car started
```

The `Car.start()` method delegates part of its behavior to its `Engine`.

---

# 22. Inheritance vs Composition

## Inheritance

Represents:

```text
"is-a"
```

Example:

```text
ClassificationModel IS-A Model
```

Implemented using:

```python
class ClassificationModel(Model):
    ...
```

## Composition

Represents:

```text
"has-a"
```

Example:

```text
Car HAS-A Engine
```

Implemented by storing another object:

```python
self.engine = Engine()
```

The choice between inheritance and composition depends on the relationship and design requirements.

---

# 23. OOP Mental Model

The major concepts covered can be summarized as:

```text
Class
  ↓
defines structure + behavior
  ↓
Object / Instance
  ↓
holds individual state
  ↓
Instance attributes + methods

Class attributes
  ↓
shared class-level state/behavior

Encapsulation
  ↓
controls object state

@property
  ↓
controlled attribute access

Inheritance
  ↓
reuse/extend behavior
  ↓
"is-a"

Polymorphism
  ↓
same interface, different behavior

Composition
  ↓
objects work together
  ↓
"has-a"
```

---

# 24. Practical Relevance to ML Engineering

OOP appears frequently in ML and software systems.

Examples include:

- Dataset classes
- Model wrappers
- Training pipelines
- Configuration objects
- Data processors
- Feature transformers
- API/service classes
- Custom utilities
- Framework classes

A practical example is the relationship between different model types:

```text
Model
├── ClassificationModel
├── RegressionModel
└── ClusteringModel
```

A common interface such as:

```python
model.predict(...)
```

can allow different implementations to be used interchangeably.

---

# Key Takeaways

1. A class defines structure and behavior; an object is an instance of a class.
2. `__init__` initializes an object's state.
3. `self` refers to the current instance.
4. Instance attributes belong to individual objects.
5. Instance methods operate on the current instance through `self`.
6. Class attributes belong to the class and can be accessed through instances.
7. Instance attributes can override class-level attributes during lookup.
8. `==` represents equality while `is` represents identity.
9. Encapsulation keeps state-management rules associated with the object.
10. `_attribute` is a convention for internal implementation details.
11. A property provides attribute-like access with custom logic.
12. A getter controls property reads.
13. A setter controls property assignments and can perform validation.
14. Inheritance represents an "is-a" relationship.
15. Method overriding allows child classes to provide specialized behavior.
16. Polymorphism allows the same interface to produce different behavior.
17. Composition represents a "has-a" relationship.
18. Composition and inheritance solve different design problems.

---

# Practical Exercise

Apply these concepts while building the Week 1 projects.

The projects should demonstrate:

- Classes
- Objects
- Instance attributes
- Methods
- Encapsulation where appropriate
- Composition where appropriate
- Clear separation of responsibilities
- Practical use of OOP rather than isolated examples
