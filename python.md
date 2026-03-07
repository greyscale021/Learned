# Python Notes

## Table of Contents

1. [Getting Started](#1-getting-started)
2. [Variables & Data Types](#2-variables--data-types)
3. [Control Flow](#3-control-flow)
4. [Functions](#4-functions)
5. [Data Structures](#5-data-structures)
6. [File Handling](#6-file-handling)
7. [Error Handling](#7-error-handling)
8. [Advanced Features](#8-advanced-features)
9. [Generators](#9-generators)
10. [Regular Expressions](#10-regular-expressions)
11. [Context Managers](#11-context-managers)
12. [Modules & Packages](#12-modules--packages)

---

## 1. Getting Started

Python is an interpreted, dynamically-typed, general-purpose language. Its philosophy emphasizes readability — code blocks are defined by **indentation**, not braces. So, no grammer needed.

### Key things to know upfront

- **Indentation** (4 spaces) defines code blocks — this is not optional
- Variables and functions use `snake_case`
- You never declare variable types explicitly (dynamic typing)
- Python supports both object-oriented and functional programming styles

### Install & verify

```bash
# Download from https://www.python.org/downloads/
python --version        # verify install
pip --version           # pip comes bundled with Python
```

### Run a script

```bash
python script.py
```

### Interactive mode (REPL)

```bash
python
>>> print("Hello!")
Hello!
>>> 2 ** 10
1024
```

> The REPL(Read-Eval-Print Loop) is great for quickly testing small ideas without writing a full script.

---

## 2. Variables & Data Types

### Declaring variables

Python infers the type from the assigned value.

```python
name       = "Alice"     # str
age        = 30          # int
price      = 9.99        # float
is_active  = True        # bool
nothing    = None        # NoneType
```

### Core built-in types

| Type | Example | Description |
|------|---------|-------------|
| `int` | `42`, `-7` | Whole numbers |
| `float` | `3.14`, `-0.5` | Decimal numbers |
| `str` | `"hello"` | Text (immutable sequence of characters) |
| `bool` | `True`, `False` | Boolean — subclass of `int` |
| `NoneType` | `None` | Represents the absence of a value |

### Type checking & conversion

```python
type(42)           # <class 'int'>
isinstance(42, int)  # True

# Explicit conversion
int("42")          # 42
float("3.14")      # 3.14
str(100)           # "100"
bool(0)            # False  (0, "", [], None are all falsy)
```

### Type annotations

Annotations don't affect runtime behavior, but they improve readability and enable static analysis with tools like `mypy`.

```python
def greet(name: str, times: int = 1) -> str:
    return f"Hello, {name}!" * times

user: str = "Alice"
count: int = 3

print(greet(user, count))
```

### String formatting

```python
name, score = "Alice", 98.6

# f-strings
print(f"{name} scored {score:.1f}")     # Alice scored 98.6

# .format()
print("{} scored {}".format(name, score))

# Common string methods
"hello world".upper()         # "HELLO WORLD"
"  hello  ".strip()           # "hello"
"a,b,c".split(",")            # ["a", "b", "c"]
", ".join(["a", "b", "c"])    # "a, b, c"
"hello".replace("l", "r")    # "herro"
```

---

## 3. Control Flow

### if / elif / else

```python
score = 87

if score >= 90:
    grade = "A"
elif score >= 75:
    grade = "B"
elif score >= 60:
    grade = "C"
else:
    grade = "F"

print(f"Grade: {grade}")  # Grade: B
```

### Ternary (inline if)

```python
status = "adult" if age >= 18 else "minor"
```

### for loop

Iterates over any iterable — lists, strings, ranges, dicts, generators.

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(fruit)

# With index
for i, fruit in enumerate(fruits, start=1):
    print(f"{i}. {fruit}")

# Range
for i in range(0, 10, 2):   # start, stop, step
    print(i)                 # 0 2 4 6 8
```

### while loop

```python
attempts = 0

while attempts < 3:
    print(f"Attempt {attempts + 1}")
    attempts += 1
```

### Loop control

```python
for n in range(10):
    if n == 3:
        continue    # skip this iteration
    if n == 7:
        break       # exit the loop
    print(n)
else:
    # runs if loop completed without hitting break
    print("Done")
```

### match statement

```python
command = "quit"

match command:
    case "quit":
        print("Quitting...")
    case "help":
        print("Showing help...")
    case _:
        print(f"Unknown command: {command}")
```

---

## 4. Functions

Functions are **first-class objects** in Python — they can be assigned to variables, passed as arguments, and returned from other functions.

### Basic definition

```python
def add(a: int, b: int) -> int:
                
    return a + b # Return the sum of two integers.

result = add(5, 3)   # 8
```

### Default arguments

```python
def greet(name: str, greeting: str = "Hello") -> None:
    print(f"{greeting}, {name}!")

greet("Alice")                  # Hello, Alice!
greet("Bob", greeting="Hi")     # Hi, Bob!
```

### *args and **kwargs

```python
# *args — collect extra positional args as a tuple
def total(*args: float) -> float:
    return sum(args)

total(1, 2, 3, 4)   # 10.0

# **kwargs — collect extra keyword args as a dict
def describe(**kwargs) -> None:
    for key, value in kwargs.items():
        print(f"{key}: {value}")

describe(name="Alice", role="admin", age=30)
```

### Unpacking into a function call

```python
nums = [1, 2, 3]
print(add(*nums))           # unpack list as positional args
```
```python
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

config = {"greeting": "Hey"}
greet("Alice", **config)    # unpack dict as keyword args
```

### Lambda functions

Small, anonymous, single-expression functions.

```python
square = lambda x: x ** 2
square(7)   # 49

# Common use: sort with a custom key
people = [("Bob", 32), ("Alice", 25), ("Eve", 28)]
people.sort(key=lambda p: p[1])   # sort by age
```

### Nested functions & closures

```python
def make_multiplier(factor: int):
    def multiply(x: int) -> int:
        return x * factor   # 'factor' is captured from outer scope
    return multiply

double = make_multiplier(2)
double(5)   # 10
```

### Decorators

A decorator wraps a function to extend its behavior without modifying it.

```python
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time() - start:.4f}s")
        return result
    return wrapper

@timer
def slow_sum(n):
    return sum(range(n))

slow_sum(1_000_000)   # slow_sum took 0.0312s
```

---

## 5. Data Structures

### List — ordered, mutable

```python
fruits = ["apple", "banana", "cherry"]

# Access
fruits[0]       # "apple"
fruits[-1]      # "cherry"
fruits[1:3]     # ["banana", "cherry"]  (slicing)

# Modify
fruits.append("date")           # add to end
fruits.insert(1, "avocado")     # insert at index
fruits.remove("banana")         # remove by value
popped = fruits.pop()           # remove & return last
del fruits[0]                   # delete by index

# Useful methods
len(fruits)
fruits.sort()
fruits.reverse()
fruits.index("cherry")  # 2
"apple" in fruits       # True
```

### Tuple — ordered, immutable

```python
point = (10, 20, 30)

x, y, z = point       # unpacking

# Tuples are hashable — can be used as dict keys or set members
coordinates = {(0, 0): "origin", (1, 0): "right"}
```

### Set — unordered, unique elements

```python
tags = {"python", "dev", "python"}   # {"python", "dev"}

tags.add("code")
tags.discard("dev")       # remove if present (no error if missing)
"python" in tags          # True  (O(1) lookup)

# Set operations
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
a | b    # union        → {1, 2, 3, 4, 5, 6}
a & b    # intersection → {3, 4}
a - b    # difference   → {1, 2}
a ^ b    # symmetric diff → {1, 2, 5, 6}
```

### Dictionary — key-value store

```python
user = {"name": "Alice", "age": 30, "admin": True}

# Access
user["name"]                    # "Alice"
user.get("role", "viewer")      # "viewer" (safe, with default)

# Modify
user["age"] = 31
user.update({"city": "NY", "age": 32})
del user["admin"]

# Iterate
for key in user:                        # keys
for value in user.values():             # values
for key, value in user.items():         # both

# Check membership
"name" in user      # True (checks keys)

# Dict comprehension
squares = {x: x**2 for x in range(6)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

### Comprehensions

A concise, readable way to build collections.

```python
# List comprehension
squares      = [x**2 for x in range(10)]
even_squares = [x**2 for x in range(10) if x % 2 == 0]

# Nested
flat = [n for row in [[1,2],[3,4]] for n in row]   # [1, 2, 3, 4]

# Set comprehension
unique_lengths = {len(word) for word in ["hi", "hello", "hey"]}

# Dict comprehension
inverted = {v: k for k, v in {"a": 1, "b": 2}.items()}

# Generator expression (lazy, memory-efficient)
total = sum(x**2 for x in range(1_000_000))
```

---

## 6. File Handling

Always use `with` — it guarantees the file is closed even if an error occurs.

### Reading

```python
# Read entire file
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()

# Read line by line (memory-efficient for large files)
with open("data.txt", "r") as f:
    for line in f:
        print(line.strip())

# Read all lines into a list
with open("data.txt", "r") as f:
    lines = f.readlines()
```

### Writing

```python
with open("output.txt", "w", encoding="utf-8") as f:
    f.write("Hello, World!\n")
    f.writelines(["line 1\n", "line 2\n"])
```

### File modes

| Mode | Description |
|------|-------------|
| `"r"` | Read (default) — error if file doesn't exist |
| `"w"` | Write — creates file, **overwrites** if exists |
| `"a"` | Append — creates file, adds to end if exists |
| `"x"` | Create — error if file already exists |
| `"b"` | Binary mode (e.g., `"rb"`, `"wb"`) |

### Working with paths (pathlib — recommended)

```python
from pathlib import Path

p = Path("data/reports/summary.txt")

p.exists()          # True / False
p.suffix            # ".txt"
p.stem              # "summary"
p.parent            # Path("data/reports")
p.read_text()       # read content directly
p.write_text("hi")  # write content directly

# Build paths safely (works on all OS)
base = Path("data")
file = base / "reports" / "summary.txt"
```

---

## 7. Error Handling

### try / except / else / finally

```python
try:
    result = int(input("Enter a number: "))
    print(10 / result)
except ValueError:
    print("That's not a valid number.")
except ZeroDivisionError:
    print("Cannot divide by zero.")
except Exception as e:
    print(f"Unexpected error: {e}")
else:
    print("Success!")       # runs only if no exception was raised
finally:
    print("Always runs.")   # cleanup code goes here
```

### Raising exceptions

```python
def set_age(age: int) -> None:
    if not isinstance(age, int):
        raise TypeError(f"Expected int, got {type(age).__name__}")
    if age < 0 or age > 150:
        raise ValueError(f"Age out of range: {age}")
    print(f"Age set to {age}")
```

### Custom exceptions

```python
class AppError(Exception):
    """Base class for application errors."""
    pass

class AuthError(AppError):
    def __init__(self, user: str):
        super().__init__(f"Access denied for user: {user}")
        self.user = user

try:
    raise AuthError("bob")
except AuthError as e:
    print(e)   # Access denied for user: bob
```

### Common built-in exceptions

| Exception | When it occurs |
|-----------|---------------|
| `ValueError` | Right type, wrong value (`int("abc")`) |
| `TypeError` | Wrong type (`"a" + 1`) |
| `KeyError` | Dict key not found |
| `IndexError` | List index out of range |
| `AttributeError` | Object has no such attribute |
| `FileNotFoundError` | File doesn't exist |
| `ZeroDivisionError` | Dividing by zero |
| `ImportError` | Module not found |

---

## 8. Advanced Features

### Classes & OOP

```python
class Animal:
    # Class variable (shared by all instances)
    kingdom = "Animalia"

    def __init__(self, name: str, sound: str) -> None:
        self.name = name        # instance variable
        self.sound = sound

    def speak(self) -> str:
        return f"{self.name} says {self.sound}!"

    def __repr__(self) -> str:
        return f"Animal(name={self.name!r})"


class Dog(Animal):
    def __init__(self, name: str) -> None:
        super().__init__(name, "woof")

    def fetch(self, item: str) -> str:
        return f"{self.name} fetches the {item}!"


dog = Dog("Rex")
print(dog.speak())      # Rex says woof!
print(dog.fetch("ball")) # Rex fetches the ball!
```

### Dunder (magic) methods

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

    def __len__(self):
        return 2

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)   # Vector(4, 6)
```

### dataclasses

Eliminate boilerplate for data-holding classes.

```python
from dataclasses import dataclass, field

@dataclass
class Point:
    x: float
    y: float
    label: str = "point"

    def distance_from_origin(self) -> float:
        return (self.x**2 + self.y**2) ** 0.5

p = Point(3.0, 4.0)
print(p)                          # Point(x=3.0, y=4.0, label='point')
print(p.distance_from_origin())   # 5.0
```

---

## 9. Generators

Generators produce values **one at a time** using `yield`. They are memory-efficient — they don't build the full sequence in memory.

### Generator function

```python
def count_up(start: int, end: int):
    """Yield integers from start to end (inclusive)."""
    current = start
    while current <= end:
        yield current
        current += 1

for n in count_up(1, 5):
    print(n)   # 1 2 3 4 5
```

### Infinite generator

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
print([next(fib) for _ in range(8)])   # [0, 1, 1, 2, 3, 5, 8, 13]
```

### Generator expression

```python
# Lazy equivalent of a list comprehension
squares = (x**2 for x in range(1_000_000))
print(next(squares))    # 0
print(next(squares))    # 1

# Memory-efficient pipeline
total = sum(x**2 for x in range(1_000_000))
```

> 💡 Use a list `[...]` when you need random access or multiple passes. Use a generator `(...)` when you only need to iterate once — especially over large data.

---

## 10. Regular Expressions

The `re` module provides pattern matching for strings.

```python
import re
```

### Common patterns

| Pattern | Matches |
|---------|---------|
| `.` | Any character except newline |
| `\d` | Digit (`[0-9]`) |
| `\w` | Word character (`[a-zA-Z0-9_]`) |
| `\s` | Whitespace |
| `^` | Start of string |
| `$` | End of string |
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 (optional) |
| `{n,m}` | Between n and m times |
| `(...)` | Capture group |

### Core functions

```python
text = "Contact alice@example.com or bob@test.org for help."

# re.search — find first match anywhere in string
match = re.search(r"\w+@\w+\.\w+", text)
if match:
    print(match.group())    # alice@example.com

# re.findall — return all matches as a list
emails = re.findall(r"[\w.+-]+@[\w-]+\.[a-z]{2,}", text)
# ["alice@example.com", "bob@test.org"]

# re.sub — replace matches
clean = re.sub(r"\s+", " ", "too   many    spaces")
# "too many spaces"

# re.match — match at the start of string only
if re.match(r"^\d{4}-\d{2}-\d{2}$", "2024-12-01"):
    print("Valid date format")
```

### Compiled patterns (reuse for performance)

```python
email_re = re.compile(r"[\w.+-]+@[\w-]+\.[a-z]{2,}", re.IGNORECASE)

for line in open("contacts.txt"):
    for email in email_re.findall(line):
        print(email)
```

---

## 11. Context Managers

Context managers handle setup and teardown automatically using the `with` statement.

### Built-in examples

```python
# File — auto-closed after block
with open("file.txt") as f:
    data = f.read()

# Multiple at once
with open("input.txt") as src, open("output.txt", "w") as dst:
    dst.write(src.read().upper())
```

### Custom context manager — class-based

```python
class ManagedDB:
    def __enter__(self):
        print("Connecting to DB...")
        return self          # value assigned to `as` variable

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Closing connection.")
        return False         # False = don't suppress exceptions

with ManagedDB() as db:
    print("Running query...")
```

### Custom context manager — using contextlib

```python
from contextlib import contextmanager
import time

@contextmanager
def timer(label: str = ""):
    start = time.perf_counter()
    try:
        yield
    finally:
        elapsed = time.perf_counter() - start
        print(f"{label} took {elapsed:.4f}s")

with timer("Matrix multiply"):
    result = sum(i * j for i in range(1000) for j in range(1000))
```

---

## 12. Modules & Packages

### Importing

```python
import math                            # full module
from datetime import date, timedelta   # specific names
import json as j                       # alias

print(math.pi)                         # 3.14159...
print(date.today())                    # 2024-12-01
print(j.dumps({"key": "val"}))         # '{"key": "val"}'
```

### Creating a module

Any `.py` file is a module. Use `__name__` to guard code that should only run when the file is executed directly.

```python
# utils.py
def format_name(first: str, last: str) -> str:
    """Return name in 'Last, First' format."""
    return f"{last}, {first}"

if __name__ == "__main__":
    # Only runs when executed directly: python utils.py
    print(format_name("Alice", "Smith"))

# In another file:
from utils import format_name
print(format_name("Bob", "Jones"))   # Jones, Bob
```

### Virtual environments

Always use a virtual environment to isolate project dependencies.

```bash
python -m venv .venv            # create
source .venv/bin/activate       # activate (macOS/Linux)
.venv\Scripts\activate          # activate (Windows)
deactivate                      # deactivate
```

### pip — package management

```bash
pip install requests                  # install latest
pip install requests==2.31.0          # install specific version
pip install -r requirements.txt       # install from file
pip freeze > requirements.txt         # export current deps
pip list                              # list installed packages
pip show requests                     # info about a package
pip uninstall requests                # remove a package
```

### Useful standard library modules

| Module | Use |
|--------|-----|
| `os` | OS interactions, env vars, paths |
| `sys` | System info, command-line args |
| `pathlib` | Modern file path handling |
| `json` | JSON encode/decode |
| `datetime` | Dates and times |
| `re` | Regular expressions |
| `math` | Math functions |
| `random` | Random number generation |
| `collections` | `Counter`, `defaultdict`, `deque` |
| `itertools` | Efficient iterators (`chain`, `product`, etc.) |
| `functools` | Higher-order functions (`lru_cache`, `partial`) |
| `typing` | Type hints (`List`, `Dict`, `Optional`, etc.) |
| `logging` | Flexible logging |
| `unittest` | Built-in testing framework |

---

## Quick Reference

### Truthy & Falsy values

```python
# Falsy: False, None, 0, 0.0, "", [], {}, set()
# Everything else is truthy

if not my_list:
    print("List is empty")
```

### Unpacking

```python
a, b, c = [1, 2, 3]
first, *rest = [1, 2, 3, 4, 5]       # first=1, rest=[2,3,4,5]
*init, last = [1, 2, 3, 4, 5]        # init=[1,2,3,4], last=5
a, b = b, a                           # swap
```

### Walrus operator `:=` (Python 3.8+)

```python
# Assign and test in one expression
while chunk := f.read(8192):
    process(chunk)

if (n := len(data)) > 10:
    print(f"Too many items: {n}")
```

### Useful built-in functions

```python
len([1, 2, 3])              # 3
range(0, 10, 2)             # 0 2 4 6 8
zip([1,2], ["a","b"])       # [(1,"a"), (2,"b")]
map(str, [1, 2, 3])         # ["1", "2", "3"]
filter(None, [0, 1, 2])     # [1, 2]
sorted([3,1,2], reverse=True)  # [3, 2, 1]
any([False, True, False])   # True
all([True, True, True])     # True
max([1, 5, 3])              # 5
sum([1, 2, 3])              # 6
enumerate(["a","b"], start=1)  # (1,"a"), (2,"b")
```

---

*References: [Python Docs](https://docs.python.org/3/) · [Python Cheatsheet](https://www.pythoncheatsheet.org/) · [Real Python](https://realpython.com/)*
