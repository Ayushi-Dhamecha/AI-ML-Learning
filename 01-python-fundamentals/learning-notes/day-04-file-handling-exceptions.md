# Day 04 — File Handling & Exception Handling

## Objective

Understand how Python programs interact with files and how exceptions can be handled in a controlled way.

The focus is on reading and writing files, understanding file modes and file positions, using context managers for resource management, and handling expected failures using Python's exception-handling mechanisms.

### Topics Covered

- File Handling
- `open()`
- File Objects
- `with open(...)`
- File Modes
- `read()`
- `readline()`
- `readlines()`
- `write()`
- File Cursor / Current Position
- Exceptions
- `try`
- `except`
- Multiple Exception Handling
- `else`
- `finally`
- `raise`
- `FileNotFoundError`

---

# 1. File Handling

File handling allows Python programs to read data from and write data to files stored on the filesystem.

Python provides the built-in `open()` function to interact with files.

Basic example:

```python
file = open("users.txt", "r")
```

Here:

- `"users.txt"` is the file name.
- `"r"` is the file mode.
- `file` is a Python file object.

The file object provides methods for reading from and writing to the file.

---

# 2. File Objects

When a file is opened, Python returns a file object.

```python
file = open("users.txt", "r")
```

The variable `file` does not contain the actual contents of the file.

Instead, it refers to a Python object that represents the opened file and provides methods such as:

```python
file.read()
file.readline()
file.readlines()
file.write()
```

Conceptually:

```text
users.txt
    ↓
open()
    ↓
Python file object
    ↓
read / write operations
```

---

# 3. Using `with open(...)`

The preferred way to work with files is to use a context manager:

```python
with open("users.txt", "r") as file:
    data = file.read()
```

Using `with` ensures that the file is properly closed after the block finishes.

This also applies if an exception occurs inside the block.

Without `with`, we would need to explicitly close the file:

```python
file = open("users.txt", "r")

data = file.read()

file.close()
```

With `with`, Python handles the cleanup automatically:

```python
with open("users.txt", "r") as file:
    data = file.read()
```

Therefore, the `with` pattern is preferred for file operations.

---

# 4. File Modes

The most commonly used file modes are:

| Mode | Purpose |
| ---- | ------- |
| `r`  | Read    |
| `w`  | Write   |
| `a`  | Append  |

---

# 5. Read Mode — `r`

`r` opens a file for reading.

```python
with open("users.txt", "r") as file:
    data = file.read()
```

If the file contains:

```text
Alice
Bob
Charlie
```

`data` contains the file contents as a string.

If the file does not exist, opening it in read mode raises:

```text
FileNotFoundError
```

---

# 6. Write Mode — `w`

`w` opens a file for writing.

```python
with open("users.txt", "w") as file:
    file.write("Alice\n")
    file.write("Bob\n")
```

Important behavior:

`w` mode replaces the existing contents of the file.

For example, if the file initially contains:

```text
Charlie
David
```

after:

```python
with open("users.txt", "w") as file:
    file.write("Alice\n")
    file.write("Bob\n")
```

the file will contain:

```text
Alice
Bob
```

The previous contents are removed.

---

# 7. Append Mode — `a`

`a` opens a file for appending.

```python
with open("users.txt", "a") as file:
    file.write("Charlie\n")
```

If the file initially contains:

```text
Alice
Bob
```

after the operation it will contain:

```text
Alice
Bob
Charlie
```

Unlike `w` mode, `a` preserves the existing contents and adds new data to the end.

---

# 8. Reading an Entire File with `read()`

`read()` reads the entire file and returns its contents as a string.

```python
with open("users.txt", "r") as file:
    data = file.read()

print(data)
```

For a file containing:

```text
Alice
Bob
Charlie
```

the result is:

```python
"Alice\nBob\nCharlie\n"
```

The type of `data` is:

```python
str
```

---

# 9. Reading One Line with `readline()`

`readline()` reads one line at a time.

```python
with open("users.txt", "r") as file:
    line1 = file.readline()
    line2 = file.readline()
```

If the file contains:

```text
Alice
Bob
Charlie
```

then:

```python
line1 == "Alice\n"
line2 == "Bob\n"
```

The second `readline()` does not start from the beginning.

The file object maintains its current position, so the second call continues from where the first call stopped.

---

# 10. Reading All Lines with `readlines()`

`readlines()` reads all lines and returns them as a list of strings.

```python
with open("users.txt", "r") as file:
    data = file.readlines()
```

For:

```text
Alice
Bob
Charlie
```

the result is:

```python
["Alice\n", "Bob\n", "Charlie\n"]
```

The type of `data` is:

```python
list
```

Each element is a string representing one line.

---

# 11. `read()` vs `readline()` vs `readlines()`

The three methods differ in what they return:

| Method        | Result                         |
| ------------- | ------------------------------ |
| `read()`      | Entire file as a string        |
| `readline()`  | One line as a string           |
| `readlines()` | All lines as a list of strings |

Example:

```python
file.read()
```

returns:

```python
"Alice\nBob\nCharlie\n"
```

```python
file.readline()
```

returns:

```python
"Alice\n"
```

```python
file.readlines()
```

returns:

```python
["Alice\n", "Bob\n", "Charlie\n"]
```

---

# 12. File Cursor / Current Position

A file object maintains a current position while reading.

Consider:

```python
with open("users.txt", "r") as file:
    line1 = file.readline()
    line2 = file.readline()
```

If the file contains:

```text
Alice
Bob
Charlie
```

the first `readline()` reads:

```text
Alice
```

and advances the current position.

The second `readline()` continues from that position and reads:

```text
Bob
```

It does not start reading from the beginning again.

Conceptually:

```text
Initial:

[Alice] [Bob] [Charlie]
   ↑

After first readline():

[Alice] [Bob] [Charlie]
          ↑

After second readline():

[Alice] [Bob] [Charlie]
                  ↑
```

---

# 13. `file.write()`

`write()` writes a string to the file.

```python
with open("users.txt", "w") as file:
    result = file.write("Alice")
```

`write()` returns the number of characters written.

For example:

```python
result == 5
```

because `"Alice"` contains five characters.

It does not return the file object.

---

# 14. Exceptions

An exception is an event that occurs during program execution when Python encounters an error or an invalid operation.

Exception handling allows us to handle such situations in a controlled way instead of allowing the program to terminate unexpectedly.

The basic structure is:

```python
try:
    # code that may raise an exception
except SomeException:
    # handle the exception
```

---

# 15. `try` and `except`

Example:

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")

print("Program continues")
```

When:

```python
10 / 0
```

is executed, Python raises:

```text
ZeroDivisionError
```

The matching `except` block handles the exception.

After handling the exception, execution continues with the code after the `try/except` block.

Output:

```text
Cannot divide by zero
Program continues
```

Execution flow:

```text
try
 ↓
exception occurs
 ↓
matching except
 ↓
handle exception
 ↓
continue after try/except
```

---

# 16. Handling Multiple Exceptions

Different exceptions can be handled using separate `except` blocks.

```python
try:
    number = int(input("Enter number: "))
    result = 100 / number

except ValueError:
    print("Invalid number")

except ZeroDivisionError:
    print("Cannot divide by zero")
```

Here:

- `ValueError` handles invalid input such as `"abc"`.
- `ZeroDivisionError` handles division by zero.

Only the matching `except` block is executed.

---

# 17. `else`

The `else` block executes only when the `try` block completes successfully without raising an exception.

```python
try:
    number = int(input("Enter number: "))
    result = 100 / number

except ValueError:
    print("Invalid number")

except ZeroDivisionError:
    print("Cannot divide by zero")

else:
    print(result)
```

If the input is:

```text
10
```

the `try` block succeeds, so the `else` block executes.

Output:

```text
10.0
```

If an exception occurs, the `else` block does not execute.

---

# 18. `finally`

The `finally` block executes regardless of whether an exception occurred.

```python
try:
    number = int(input("Enter number: "))
    result = 100 / number

except ZeroDivisionError:
    print("Cannot divide by zero")

finally:
    print("Finished")
```

For both successful and failed execution, `finally` executes.

The general structure is:

```python
try:
    ...
except:
    ...
else:
    ...
finally:
    ...
```

Conceptually:

```text
try
 │
 ├── success ──→ else
 │
 └── exception ─→ matching except
                       │
                       ↓
                    finally
```

---

# 19. `raise`

`raise` is used when we explicitly want to raise an exception.

It is useful when a program has a rule that must be enforced.

Example:

```python
def withdraw(amount):
    if amount <= 0:
        raise ValueError("Amount must be positive")
```

Here the program deliberately raises `ValueError` when the provided amount violates the function's rule.

Another example:

```python
if balance < 0:
    raise ValueError("Balance cannot be negative")
```

This allows the program to explicitly reject an invalid state.

---

# 20. Handling `FileNotFoundError`

File operations can raise specific exceptions.

For example:

```python
def read_user_file(filename):
    try:
        with open(filename, "r") as file:
            return file.read()

    except FileNotFoundError:
        return "File does not exist"
```

If the requested file does not exist:

```python
result = read_user_file("users.txt")
```

the `open()` operation raises:

```text
FileNotFoundError
```

The matching `except` block handles the exception.

Therefore:

```python
result == "File does not exist"
```

The program does not crash because the exception was handled.

---

# 21. File Handling and Exception Handling Together

File operations commonly need exception handling because operations such as opening, reading, and writing files can fail.

Example:

```python
try:
    with open("users.txt", "r") as file:
        data = file.read()

except FileNotFoundError:
    print("File does not exist")
```

The responsibilities are separated:

```text
with open(...)
      ↓
manage file resource

try
      ↓
execute operation that may fail

except
      ↓
handle expected failure
```

---

# 22. Key Takeaways

## File Handling

1. `open()` creates a Python file object for interacting with a file.
2. `with open(...)` is the preferred way to work with files because the file is automatically closed.
3. `r` is used for reading.
4. `w` is used for writing and replaces existing content.
5. `a` is used for appending and preserves existing content.
6. `read()` returns the entire file as a string.
7. `readline()` returns one line and continues from the current file position.
8. `readlines()` returns all lines as a list of strings.
9. `write()` returns the number of characters written.
10. A file object maintains a current position while reading.

## Exception Handling

1. `try` contains code that may raise an exception.
2. `except` handles a matching exception.
3. Multiple `except` blocks can handle different exception types.
4. `else` executes only when the `try` block succeeds.
5. `finally` executes regardless of whether an exception occurred.
6. `raise` explicitly raises an exception.
7. Specific exceptions such as `FileNotFoundError`, `ValueError`, and `ZeroDivisionError` can be handled explicitly.

---

# 23. Practical Relevance

File handling and exception handling are commonly used together in real applications.

Examples include:

- Reading configuration files
- Processing datasets
- Reading CSV or JSON files
- Writing generated reports
- Handling missing files
- Validating input
- Preventing invalid application states
- Cleaning up resources after operations

These concepts will be applied in the Week 1 practical exercises and projects.
