# Day 02 — Python Fundamentals

## Objective

Strengthen Python fundamentals required for ML engineering, with emphasis on understanding Python's behavior rather than syntax memorization.

### Topics Covered

- Rebinding vs Mutation
- List Concatenation vs Mutation
- Shallow Copy
- Deep Copy
- Operators
- Truthiness
- Conditional Statements
- `break` and `continue`
- Functions and Default Arguments
- Mutable Default Arguments
- Scope and LEGB
- `global`
- Nested Functions
- `*args`
- `**kwargs`
- Lambda Expressions
- `map()`
- List Comprehensions
- Generator Functions
- `yield`
- `next()`
- Lazy Evaluation
- `StopIteration`

---

# 1. Rebinding vs Mutation

A variable in Python is a name bound to an object.

Two important operations are:

- **Rebinding** — changing what a variable refers to.
- **Mutation** — modifying an existing mutable object.

## Rebinding

Integers are immutable.

```python
x = 10
y = x

x = 20

print(x)  # 20
print(y)  # 10
```

`x = 20` does not modify the integer object `10`.

Instead, `x` is rebound to another integer object.

```text
Before:

x ──┐
    ├──> 10
y ──┘


After x = 20:

y ─────> 10
x ─────> 20
```

## Mutation

Lists are mutable.

```python
x = [1, 2, 3]
y = x

x.append(4)

print(x)  # [1, 2, 3, 4]
print(y)  # [1, 2, 3, 4]
```

`append()` modifies the existing list object.

Both `x` and `y` still reference the same object.

```text
x ──┐
    ├──> [1, 2, 3, 4]
y ──┘
```

---

# 2. `+=` with Immutable and Mutable Objects

For immutable objects such as integers:

```python
x = 10
y = x

x += 1

print(x)  # 11
print(y)  # 10
```

`x += 1` results in `x` being rebound to a new integer object.

For mutable objects such as lists, `+=` can mutate the existing object.

```python
a = [1, 2]
b = a

a += [3]

print(a)       # [1, 2, 3]
print(b)       # [1, 2, 3]
print(a is b)  # True
```

The important distinction is whether the operation mutates the existing object or creates a new object and rebinds the variable.

---

# 3. List Concatenation vs Mutation

The `+` operator creates a new list.

```python
a = [1, 2]
b = a

a = a + [3]

print(a)       # [1, 2, 3]
print(b)       # [1, 2]
print(a is b)  # False
```

The original list was not mutated.

A new list was created and `a` was rebound to it.

By contrast:

```python
a = [1, 2]
b = a

a.append(3)

print(a)       # [1, 2, 3]
print(b)       # [1, 2, 3]
print(a is b)  # True
```

Here the existing list was mutated.

---

# 4. Shallow Copy

`copy()` creates a new outer object.

```python
a = [1, 2, 3]
b = a.copy()

b.append(4)

print(a)       # [1, 2, 3]
print(b)       # [1, 2, 3, 4]
print(a is b)  # False
```

The outer lists are different objects.

## Nested Lists

With nested mutable objects, `copy()` performs a shallow copy.

```python
a = [[1, 2], [3, 4]]
b = a.copy()
```

The outer lists are different, but the nested lists are still shared.

```python
b[0].append(99)

print(a)  # [[1, 2, 99], [3, 4]]
print(b)  # [[1, 2, 99], [3, 4]]
```

Conceptually:

```text
a ──> Outer List A
       │
       ├──> Inner List 1
       └──> Inner List 2

b ──> Outer List B
       │
       ├──> Same Inner List 1
       └──> Same Inner List 2
```

---

# 5. Deep Copy

`deepcopy()` recursively creates copies of nested objects.

```python
from copy import deepcopy

a = [[1, 2], [3, 4]]
b = deepcopy(a)

b[0].append(99)

print(a)  # [[1, 2], [3, 4]]
print(b)  # [[1, 2, 99], [3, 4]]
```

Both the outer and nested lists are separate objects.

```text
a ──> Outer List A
       │
       ├──> Inner List A1
       └──> Inner List A2

b ──> Outer List B
       │
       ├──> Inner List B1
       └──> Inner List B2
```

---

# 6. Operators

## Arithmetic Operators

```python
x = 10
y = 3

print(x / y)   # 3.333...
print(x // y)  # 3
print(x % y)   # 1
print(x ** y)  # 1000
```

Important operators:

- `/` → true division
- `//` → floor division
- `%` → remainder
- `**` → exponentiation

## Membership Operators

```python
numbers = [1, 2, 3]

print(1 in numbers)       # True
print(4 not in numbers)   # True
```

## Identity Operators

```python
a is b
a is not b
```

Identity checks whether two references point to the same object.

---

# 7. Truthiness

Python evaluates objects in a Boolean context.

Common falsy values include:

```python
None
False
0
""
[]
```

Examples:

```python
print(bool(None))    # False
print(bool(0))       # False
print(bool(""))      # False
print(bool([]))      # False
print(bool([0]))     # True
print(bool(False))   # False
print(bool("0"))     # True
print(bool("False")) # True
```

A non-empty string is truthy regardless of its textual content.

Similarly, a non-empty list is truthy even if its element is `0`.

---

# 8. Conditional Statements

Python evaluates conditional branches from top to bottom.

Once a condition is satisfied, that branch executes and the remaining `elif` and `else` branches are not evaluated.

```python
if condition_1:
    ...
elif condition_2:
    ...
else:
    ...
```

Only the first matching branch executes.

Example:

```python
x = 10

if x > 5:
    print("Pass")
elif x > 2:
    print("Partial")
else:
    print("Fail")
```

Output:

```text
Pass
```

---

# 9. `break` and `continue`

## `break`

`break` terminates the loop immediately.

```python
for number in range(1, 6):
    if number == 3:
        break
    print(number)
```

Output:

```text
1
2
```

When `number == 3`, the loop terminates and execution continues with the statement after the loop.

## `continue`

`continue` skips the current iteration and continues with the next iteration.

```python
for number in range(1, 6):
    if number == 3:
        continue
    print(number)
```

Output:

```text
1
2
4
5
```

When `number == 3`, only that iteration is skipped.

---

# 10. Functions and Default Arguments

Functions can define default parameter values.

```python
def calculate_total(amount, tax=0.18):
    return amount + amount * tax
```

Therefore:

```python
calculate_total(100)
```

uses the default value `0.18`.

While:

```python
calculate_total(100, 0.10)
```

uses `0.10`.

A supplied argument overrides the default value.

---

# 11. Mutable Default Argument Pitfall

Avoid using mutable objects such as lists as default arguments.

Problematic example:

```python
def add_user(name, users=[]):
    users.append(name)
    return users
```

The same list can be reused across multiple function calls.

Prefer:

```python
def add_user(name, users=None):
    if users is None:
        users = []

    users.append(name)
    return users
```

Now each call that does not provide `users` gets a new list.

Example:

```python
print(add_user("Alice"))
print(add_user("Bob"))
```

Output:

```text
['Alice']
['Bob']
```

---

# 12. Scope and LEGB

Python resolves names using the LEGB rule:

```text
Local
Enclosing
Global
Built-in
```

Example:

```python
x = 10

def func():
    x = 20
    print(x)

func()
print(x)
```

Output:

```text
20
10
```

The `x` inside the function is local to that function and does not rebind the global `x`.

---

# 13. `global`

The `global` keyword allows a function to rebind a global variable.

```python
x = 10

def update():
    global x
    x = 20

update()

print(x)  # 20
```

Without `global`, assigning to `x` inside the function creates a local binding.

---

# 14. Nested Functions and Enclosing Scope

A nested function can access variables from its enclosing function.

```python
def outer():
    x = "outer"

    def inner():
        print(x)

    inner()

outer()
```

Output:

```text
outer
```

The nested function finds `x` in the enclosing scope.

This is part of Python's LEGB name-resolution behavior.

---

# 15. `*args`

`*args` collects additional positional arguments into a tuple.

```python
def calculate_total(*prices):
    return sum(prices)
```

Examples:

```python
print(calculate_total(10, 20))
# 30

print(calculate_total(10, 20, 30, 40))
# 100
```

Inside the function, `prices` is a tuple.

---

# 16. `**kwargs`

`**kwargs` collects additional keyword arguments into a dictionary.

```python
def create_user(**details):
    print(details)
```

Example:

```python
create_user(
    name="Alice",
    age=25,
    city="Ahmedabad"
)
```

Output:

```text
{'name': 'Alice', 'age': 25, 'city': 'Ahmedabad'}
```

Difference:

```text
*args    → positional arguments → tuple
**kwargs → keyword arguments   → dictionary
```

---

# 17. Lambda Expressions

A lambda is an anonymous function generally used for small expressions.

Example:

```python
double = lambda x: x * 2

print(double(5))  # 10
```

Lambda expressions are commonly used with functions such as `map()` and `filter()`.

---

# 18. `map()`

`map()` applies a function to each item in an iterable.

Example:

```python
numbers = [1, 2, 3, 4, 5]

result = map(lambda x: x * 2, numbers)

print(list(result))
```

Output:

```text
[2, 4, 6, 8, 10]
```

`map()` returns a lazy `map` object.

Using `list()` materializes the results into a list.

Without `list()`:

```python
result = map(lambda x: x * 2, numbers)

print(result)
```

the output will be a representation of a `map` object rather than the actual list of values.

---

# 19. List Comprehensions

A list comprehension provides a concise way to create a list.

Example:

```python
numbers = [1, 2, 3, 4, 5]

result = [x * 2 for x in numbers if x > 2]

print(result)
```

Output:

```text
[6, 8, 10]
```

Equivalent traditional structure:

```python
result = []

for x in numbers:
    if x > 2:
        result.append(x * 2)
```

The condition filters which elements participate in the transformation.

---

# 20. Conditional Expressions in Comprehensions

A comprehension can also produce different values based on a condition.

```python
numbers = [1, 2, 3, 4, 5]

result = [
    "even" if x % 2 == 0 else "odd"
    for x in numbers
]

print(result)
```

Output:

```text
['odd', 'even', 'odd', 'even', 'odd']
```

Here the condition does not filter elements out.

Every input produces an output value.

---

# 21. Generator Functions

A generator function uses `yield`.

```python
def numbers():
    yield 10
    yield 20
    yield 30
```

Calling:

```python
gen = numbers()
```

creates a generator object.

The function does not execute all the way through immediately.

---

# 22. `next()` and Generator State

```python
gen = numbers()

print(next(gen))
print(next(gen))
```

Output:

```text
10
20
```

The generator remembers its execution state.

If we then use:

```python
for value in gen:
    print(value)
```

the loop continues from where the previous `next()` calls stopped.

Output:

```text
30
```

The generator does not restart from `10`.

After all values have been consumed, another `next()` call raises `StopIteration`.

---

# 23. `yield` vs `return`

`return` finishes the function and returns a result.

`yield` pauses a generator function and preserves its state so execution can continue later.

```text
return
  ↓
function ends

yield
  ↓
pause
  ↓
resume later
```

A generator produces values one at a time rather than returning the complete sequence at once.

---

# 24. Lazy Evaluation

Generators provide lazy evaluation.

Instead of creating all results in memory at once, values can be produced one at a time as required.

This becomes particularly important in ML and data engineering when working with large datasets or streams where loading everything into memory is undesirable.

---

# Key Takeaways

1. Rebinding changes what a name refers to.
2. Mutation changes an existing mutable object.
3. `==` checks equality while `is` checks identity.
4. `copy()` creates a shallow copy.
5. `deepcopy()` recursively copies nested objects.
6. Python has rich truthiness rules.
7. `break` terminates a loop while `continue` skips the current iteration.
8. Default arguments are evaluated once; mutable defaults should generally be avoided.
9. Python uses LEGB for name resolution.
10. `*args` collects positional arguments into a tuple.
11. `**kwargs` collects keyword arguments into a dictionary.
12. Lambda expressions are useful for small functions.
13. `map()` applies a function lazily to each item.
14. List comprehensions can transform and/or filter data.
15. Generators provide lazy evaluation using `yield`.
16. `next()` resumes a generator from its current state.
17. `StopIteration` indicates that a generator has no more values.

---

# Practical Relevance to ML Engineering

These concepts will repeatedly appear in:

- Data preprocessing
- Dataset transformations
- Feature engineering
- Model pipelines
- Configuration handling
- Iterating through large datasets
- Writing reusable utilities
- Processing large datasets without unnecessary memory usage
