# Day 01 - Python Object Model

## Objective

Understand how Python stores and manages data internally by learning about objects, references, identity, mutability, and equality.

---

# Topics Covered

- Variables
- Objects
- References
- Object Identity
- Object Type
- Object Value
- Mutable vs Immutable (Introduction)
- Equality (`==`)
- Identity (`is`)
- Singleton Object (`None`)

---

# 1. Variables

Unlike some programming languages, Python variables do **not** directly store values.

A variable stores a **reference** to an object.

Example:

```python
x = 10
```

Here,

- `10` is an object.
- `x` is a reference pointing to that object.

---

# 2. Everything in Python is an Object

Everything in Python is treated as an object.

Examples:

```python
10              # Integer Object
3.14            # Float Object
"Hello"         # String Object
[1, 2, 3]       # List Object
{"a": 1}        # Dictionary Object
```

Even functions and classes are objects.

---

# 3. Every Object Has Three Properties

## Identity

Identity uniquely identifies an object during its lifetime.

```python
x = 10

print(id(x))
```

Use `id()` to get the object's identity.

---

## Type

Type defines what kind of object it is.

```python
print(type(x))
```

Output

```python
<class 'int'>
```

---

## Value

The actual data stored inside the object.

Example

```python
x = 10
```

Value = `10`

---

# 4. References

Assignment copies **references**, not objects.

Example

```python
x = 10
y = x
```

Both variables refer to the same object.

```
      Object
    +--------+
    |   10   |
    +--------+
      ▲    ▲
      │    │
      x    y
```

Therefore

```python
id(x) == id(y)
```

returns

```python
True
```

---

# 5. Immutable Objects

Examples

- int
- float
- bool
- str
- tuple

Example

```python
x = 10
y = x

x = 20
```

Python does **not** modify the existing integer object.

Instead,

- A new integer object (`20`) is used.
- `x` is rebound to the new object.
- `y` still points to `10`.

```
     +--------+      +--------+
     |   10   |      |   20   |
     +--------+      +--------+
         ▲               ▲
         │               │
         y               x
```

---

# 6. Mutable Objects (Introduction)

Examples

- list
- dict
- set

Mutable objects can be modified without creating a new object.

Example

```python
a = [1, 2, 3]
b = a

b.append(4)
```

Memory

```
        +----------------+
        | [1,2,3,4]      |
        +----------------+
           ▲         ▲
           │         │
           a         b
```

Both variables still point to the same object.

Output

```python
print(a)
print(b)
```

```
[1, 2, 3, 4]
[1, 2, 3, 4]
```

---

# 7. Equality (`==`) vs Identity (`is`)

## Equality (`==`)

Compares values.

```python
a = [1,2,3]
b = [1,2,3]

print(a == b)
```

Output

```python
True
```

Because both lists contain the same values.

---

## Identity (`is`)

Compares object identity.

```python
a = [1,2,3]
b = [1,2,3]

print(a is b)
```

Output

```python
False
```

Because both variables point to different objects.

---

Example

```python
a = [1,2,3]
b = a

print(a == b)
print(a is b)
```

Output

```python
True
True
```

Both variables refer to the same object.

---

# 8. Why Use `is None`

`None` is a singleton object.

Recommended

```python
if value is None:
    ...
```

Instead of

```python
if value == None:
    ...
```

Reason:

- `is` checks object identity.
- `None` has only one instance.
- This is the recommended and idiomatic Python style.

---

# Key Takeaways

- Variables store references, not values.
- Everything in Python is an object.
- Every object has an identity, type, and value.
- Assignment copies references.
- Immutable objects create a new object when reassigned.
- Mutable objects can be modified in place.
- `==` compares values.
- `is` compares object identity.
- Use `is None` when checking for `None`.

---

# Common Mistakes

❌ Thinking variables store values.

✔ Variables store references.

---

❌ Using `is` to compare integers or strings.

✔ Use `==` for value comparison.

---

❌ Assuming assignment copies an object.

✔ Assignment copies the reference.

---

# My Learning Reflection

Today I strengthened my understanding of Python's object model.

The biggest takeaway for me was realizing that variables do not store values directly—they store references to objects. This explains why immutable objects behave differently from mutable objects during assignment and modification.

One important lesson was to distinguish between:

- Rebinding a reference
- Modifying an existing object

Understanding this foundation will make it easier to work with complex data structures like Pandas DataFrames, NumPy arrays, and PyTorch tensors in upcoming ML topics.

---

**Status:** ✅ Completed
