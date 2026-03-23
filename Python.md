# Python Notes
> Focused on cloud engineering (automation, scripting, AWS SDK) and basic competitive programming.

<details>
    <summary><strong>Things to be clear</strong></summary>

1. This note is not for complete beginner, first get comfortable with the basics then use it.
    
</details>

## Table of Contents

1. [Getting Started](#1-getting-started)
2. [Variables & Data Types](#2-variables--data-types)
3. [Control Flow](#3-control-flow)
4. [Functions](#4-functions)
5. [Data Structures](#5-data-structures)
6. [File Handling](#6-file-handling)
7. [Error Handling](#7-error-handling)
8. [OOP Basics](#8-oop-basics)
9. [Regular Expressions](#9-regular-expressions)
10. [subprocess](#10-subprocess)
11. [Modules & Packages](#11-modules--packages)
12. [CP Essentials](#12-cp-essentials)

---

## 1. Getting Started

Python is an interpreted, dynamically-typed, general-purpose language. Its philosophy emphasizes readability — code blocks are defined by **indentation**, not braces.

### Key things to know upfront

- **Indentation** (4 spaces) defines code blocks — this is not optional
- Variables and functions use `snake_case`
- You never declare variable types explicitly (dynamic typing)
- Python supports both object-oriented and functional programming styles

### Install & verify

```bash
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

> The REPL (Read-Eval-Print Loop) is great for quickly testing small ideas without writing a full script.

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
type(42)              # <class 'int'>
isinstance(42, int)   # True

# Explicit conversion
int("42")             # 42
float("3.14")         # 3.14
str(100)              # "100"
bool(0)               # False  (0, "", [], None are all falsy)
```

### Type annotations

Annotations don't affect runtime behavior, but they improve readability and are good practice for scripts others may read.

```python
def greet(name: str, times: int = 1) -> str:
    return f"Hello, {name}!" * times
```

### String formatting
> Note: Strings are immutable, so we can't change it, (.) methods creates new strings.

```python
name, score = "Alice", 98.67

# f-strings (format specifiers)
f"{name} scored {score:.1f}"    # "Alice scored 98.7"  — 1 decimal
f"{score:.2f}"                  # "98.67"   — 2 decimals
f"{score:.0f}"                  # "99"  — rounded
f"{42:05d}"                     # "00042"   — zero padded

# Case
"hello".upper()                 # "HELLO"
"HELLO".lower()                 # "hello"
"hello world".capitalize()      # "Hello world"  — first letter only
"hello world".title()           # "Hello World"  — every word

# Whitespace
"  hello  ".strip()             # "hello"   — both sides
"  hello  ".lstrip()            # "hello  " — left only
"  hello  ".rstrip()            # "  hello" — right only

# Search
"hello".find("l")               # 2 — index of first match, -1 if not found
"hello".count("l")              # 2 — total occurrences
"error: disk".startswith("error")  # True
"error: disk".endswith("disk")     # True

# Split & Join
"a,b,c".split(",")              # ["a", "b", "c"]
", ".join(["a", "b", "c"])      # "a, b, c"

# Replace & Pad
"hello".replace("l", "r")       # "herro"
"42".zfill(5)                   # "00042"
"hi".ljust(10, "-")             # "hi--------"
"hi".rjust(10, "-")             # "--------hi"

# Validation
"123".isdigit()     # True — useful for input validation
"hello".isalpha()   # True
"hello".islower()   # True
"HELLO".isupper()   # True
```

---

## 3. Control Flow

### if / elif / else
```python
# Syntax
if <condition>:
    <statement>
elif <condition>:
    <statement>
else:
    <statement>
```

```python
score = int(input())

if score >= 90:
    grade = "A"
elif score >= 75:
    grade = "B"
elif score >= 60:
    grade = "C"
else:
    grade = "F"
```

### Ternary (inline if)
```python
# Syntax
value_if_true if condition else value_if_false
```

```python
grade = "A" if score >= 90 else "not A"
```

### for loop (Used on iterables)

```python
services = ["ec2", "s3", "lambda"]

for service in services:
    print(service)

# With index
for i, service in enumerate(services, start=1): # Unpacking
    print(f"{i}: {service}")

# Range
for i in range(0, 10, 2):   # start, stop, step
    print(i)                 # 0 2 4 6 8
```

### while loop (Loop pressists until condition is false)

```python
retries = 0

while retries < 3:
    print(f"Attempt {retries + 1}")
    retries += 1
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
    # runs only if loop completed without hitting break
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

Functions are **first-class objects** (can be passed around) in Python — they can be assigned to variables, passed as arguments, and returned from other functions.

### Basic definition

```python
def add(a: int, b: int) -> int: # Return the sum of two integers.
    return a + b

result = add(5, 3)   # 8
```

### Default arguments

```python
def connect(host: str, port: int = 22) -> None:
    print(f"Connecting to {host} :{port}")

connect("192.168.1.1")           # Connecting to 192.168.1.1 :22
connect("192.168.1.1", 80)       # Connecting to 192.168.1.1 :80
```

### *args and **kwargs

```python
# *args — collect extra positional args as a tuple
def order(food: str, *toppings: str) -> None:
    print(f"You get {food} With:", end=" ")
    for top in toppings:
        print(f"{top}", end="-")

order("Pizza", "Pineapple", "tomato", "milk")    # You get Pizza With: Pineapple-tomato-milk-


# **kwargs — collect extra keyword args as a dict
def log_event(**kwargs) -> None:
    for key, value in kwargs.items():   # Unpacking
        print(f"{key}: {value}")

log_event(level="INFO", service="ec2", message="Instance started")
# level: INFO
# service: ec2
# message: Instance started
```

### Unpacking into a function call

```python
config = {"host": "10.0.0.1", "port": 443}
connect(**config)   # unpacks dict as keyword args
```

### Lambda functions

Anonymous single-expression functions — used inline where a full `def` would be overkill.

```python
# Syntax
lambda <parameters> : <expression to return>
```

```python
# Sort a list of dicts by a field
instances = [{"id": "i-3", "cpu": 80}, {"id": "i-1", "cpu": 20}]
instances.sort(key=lambda x: x["cpu"])
# key= tells sort what value to extract from each item for comparison
# lambda x: x["cpu"] — for each dict x, return the cpu value
# result: [{"id": "i-1", "cpu": 20}, {"id": "i-3", "cpu": 80}]
```

---

## 5. Data Structures

### List — ordered, mutable

```python
regions = ["us-east-1", "us-west-2", "eu-west-1"]

# Access
regions[0]        # "us-east-1"
regions[-1]       # "eu-west-1"
regions[0:2]      # ["us-east-1", "us-west-2"]  (slicing)

# Modify
regions.append("ap-south-1")    # Append ap-south-1 to the end
regions.insert(1, "ca-central-1")   # Insert ca-central-1 at index 1
regions.remove("us-west-2")     # Removes by value
del regions[0]                  # Removes by index
popped = regions.pop()          # Removes and returns last item (default)
popped = regions.pop(0)         # Removes and returns item at index 0
popped = regions.pop(regions.index("us-west-2"))  # Removes by value, and returns it

# Useful
regions.index("us-east-1")  # Returns index of us-east-1
len(regions)                 # Length of regions
regions.sort()               # Sorts the region list (default ascending order)
"us-east-1" in regions       # True
```

### Tuple — ordered, immutable (hashable)

```python
endpoint = ("https://api.example.com", 443)
host, port = endpoint   # unpacking

# Tuples are hashable — can be used as dict keys
# Hashing means converting a value into a fixed number(a hash) to store and find it quickly.
cache = {("us-east-1", "ec2"): "data"}
# Tuples are immutable, so dict key hash won't change.
```

### Set — unordered, mutable, unique elements

```python
active = {"ec2", "s3", "ec2"}   # duplicates are dropped → {"ec2", "s3"}

active.add("lambda")      # adds "lambda" to the set
active.discard("s3")      # removes "s3" (no error if it doesn't exist)
active.remove("s3")       # removes "s3" (raises KeyError if it doesn't exist)
"ec2" in active           # True  (O(1) lookup)

# Set operations — useful for diffing resource lists
deployed = {"ec2", "rds", "s3"}
expected = {"ec2", "s3", "lambda"}
missing  = expected - deployed    # in expected but not deployed → {"lambda"}
extra    = deployed - expected    # in deployed but not expected → {"rds"}
```

### Dictionary — key-value pair

```python
instance = {"id": "i-abc123", "type": "t3.micro", "state": "running"}

# Access
instance["id"]                      # "i-abc123" — access value by key
instance.get("region", "unknown")   # access value of region, if not found, return unknown

# Modify
instance["state"] = "stopped"                          # update existing key
instance["zone"] = "us-east-1a"                        # add new key
instance.update({"region": "us-east-1", "az": "us-east-1a"})  # add/update multiple at once
del instance["az"]                                     # remove a key (KeyError if missing)

# Iterate
for key in instance:                  # loops over keys
for value in instance.values():       # loops over values
for key, value in instance.items():   # loops over both (most common)

# Check membership
"id" in instance    # True — checks keys only, not values

# Dict comprehension — build a dict from a list of pairs
env_map = {k: v for k, v in [("DB_HOST", "localhost"), ("PORT", "5432")]}
# {"DB_HOST": "localhost", "PORT": "5432"}
```

### Comprehensions

A shorter way to build collections from loops — replaces a loop + append with a single readable line.

```python
# Syntax
[<expression> for <item> in <iterable> if <condition>]      # list
{<key>: <value> for <item> in <iterable>}                   # dict
(<expression> for <item> in <iterable>)                     # generator (lazy)
```

```python
instances = [
    {"id": "i-1", "state": "running", "cost": 10},
    {"id": "i-2", "state": "stopped", "cost": 5},
    {"id": "i-3", "state": "running", "cost": 20},
]

# List comprehension — filter
running = [i for i in instances if i["state"] == "running"]
# [{"id": "i-1", ...}, {"id": "i-3", ...}]

# List comprehension — extract field
ids = [i["id"] for i in instances]
# ["i-1", "i-2", "i-3"]

# Dict comprehension — build id → state map
id_to_state = {i["id"]: i["state"] for i in instances}
# {"i-1": "running", "i-2": "stopped", "i-3": "running"}

# Generator expression — lazy, no list built in memory
total_cost = sum(i["cost"] for i in instances)
# 35
```

---

## 6. File Handling

Always use `with` — it guarantees the file is closed even if an error occurs.

### Reading

```python
# Read entire file
with open("config.txt", "r", encoding="utf-8") as f:
    content = f.read()

# Read line by line (memory-efficient for large log files)
with open("app.log", "r") as f:
    for line in f:
        print(line.strip())

# Read all lines into a list
with open("hosts.txt", "r") as f:
    hosts = [line.strip() for line in f]
```

### Writing

```python
with open("report.txt", "w", encoding="utf-8") as f:
    f.write("Deployment report\n")
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

### JSON files (very common in cloud scripts)

```python
import json

# Read a config or API response saved to file
with open("config.json", "r") as f:
    config = json.load(f)       # file → dict

# Write a dict to a JSON file
with open("output.json", "w") as f:
    json.dump(config, f, indent=2)

# Convert between string and dict (for API responses)
data = json.loads('{"region": "us-east-1"}')   # str → dict
text = json.dumps(data, indent=2)               # dict → str
```

### Working with paths (pathlib — recommended)

```python
from pathlib import Path

p = Path("logs/app.log")

p.exists()           # True / False
p.suffix             # ".log"
p.stem               # "app"
p.parent             # Path("logs")
p.read_text()        # read content directly
p.write_text("hi")   # write content directly

# Build paths safely (works on all OS)
log_file = Path("logs") / "2024" / "app.log"
```

---

## 7. Error Handling

Wrap risky code in `try` — Python jumps to the matching `except` block if something goes wrong instead of crashing.

```python
# Syntax
try:
    <risky code>
except <ExceptionType>:
    <handle it>
except <ExceptionType> as e:
    <handle it, e holds the error message>
else:
    <runs only if no exception was raised>
finally:
    <always runs — cleanup goes here>
```

```python
try:
    result = int(input("Enter a number: "))
    print(10 / result)
except ValueError:
    print("That's not a valid number.")       # wrong value type
except ZeroDivisionError:
    print("Cannot divide by zero.")           # division by zero
except Exception as e:
    print(f"Unexpected error: {e}")           # catch-all for anything else
else:
    print("Success!")       # runs only if no exception was raised
finally:
    print("Always runs.")   # cleanup code goes here
```

### Raising exceptions

Manually throw an exception when input is invalid — stops bad data early.

```python
def set_port(port: int) -> None:
    if not isinstance(port, int):
        raise TypeError(f"Expected int, got {type(port).__name__}")
    if not (1 <= port <= 65535):
        raise ValueError(f"Invalid port: {port}")
    print(f"Port set to {port}")
```

### Common built-in exceptions

| Exception | When it occurs |
|-----------|----------------|
| `ValueError` | Right type, wrong value (`int("abc")`) |
| `TypeError` | Wrong type (`"a" + 1`) |
| `KeyError` | Dict key not found |
| `IndexError` | List index out of range |
| `AttributeError` | Object has no such attribute |
| `FileNotFoundError` | File doesn't exist |
| `ZeroDivisionError` | Dividing by zero |
| `ImportError` | Module not found |

> For cloud scripts, catching specific exceptions and logging them cleanly matters more than building custom exception hierarchies. Keep it simple.

---

## 8. OOP Basics

A class is a blueprint — it defines attributes (data) and methods (behavior) that each object created from it will have.

> You'll mostly *read* OOP when working with SDKs like boto3, not write complex class hierarchies. Know enough to understand what you're looking at and write simple classes when needed.

```python
# Syntax
class <ClassName>:
    def __init__(self, <params>) -> None:   # constructor — runs when object is created
        self.<attribute> = <value>          # self = the object being created

    def <method>(self) -> None:             # method — a function belonging to the class
        ...
```

### Basic class

```python
class EC2Instance:
    def __init__(self, instance_id: str, instance_type: str) -> None:
        self.instance_id   = instance_id    # store as attribute on the object
        self.instance_type = instance_type
        self.state         = "stopped"      # default value

    def start(self) -> None:
        self.state = "running"              # modify the object's own attribute
        print(f"{self.instance_id} is now running.")

    def stop(self) -> None:
        self.state = "stopped"
        print(f"{self.instance_id} is now stopped.")

    def __repr__(self) -> str:              # controls what print(obj) shows
        return f"EC2Instance(id={self.instance_id!r}, state={self.state!r})"


inst = EC2Instance("i-abc123", "t3.micro")  # creates an object from the class
inst.start()
print(inst)   # EC2Instance(id='i-abc123', state='running')
```

### Inheritance

A child class inherits everything from the parent and can override or extend it.

```python
class AWSResource:
    def __init__(self, resource_id: str, region: str) -> None:
        self.resource_id = resource_id
        self.region      = region

    def describe(self) -> str:
        return f"{self.resource_id} in {self.region}"


class S3Bucket(AWSResource):                        # S3Bucket inherits from AWSResource
    def __init__(self, name: str, region: str) -> None:
        super().__init__(name, region)              # call parent's __init__
        self.name = name

    def describe(self) -> str:                      # override parent's describe
        return f"S3 Bucket: {self.name} ({self.region})"


bucket = S3Bucket("my-bucket", "us-east-1")
print(bucket.describe())   # S3 Bucket: my-bucket (us-east-1)
```

### What to recognize in SDK code

```python
import boto3

# boto3 returns objects — you call methods on them, not raw dicts
ec2 = boto3.resource("ec2")
instance = ec2.Instance("i-abc123")

instance.start()                # method call on object
print(instance.state)           # attribute access
print(instance.instance_type)
```

---

## 9. Regular Expressions

A way to search, match, and manipulate text using patterns instead of exact strings — essential for parsing logs and validating inputs.

```python
import re

# Syntax
re.search(pattern, string)            # first match anywhere → match object or None
re.findall(pattern, string)           # all matches → list
re.sub(pattern, replacement, string)  # replace matches → new string
re.match(pattern, string)             # match at start of string only
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
log = "2024-12-01 ERROR [ec2] Instance i-abc123 failed to start"

# re.search — find first match anywhere in string
match = re.search(r"i-[a-z0-9]+", log)
if match:
    print(match.group())    # i-abc123

# re.findall — return all matches as a list
errors = re.findall(r"ERROR.+", log)

# re.sub — replace matches (e.g. sanitize output)
clean = re.sub(r"\s+", " ", "too   many    spaces")

# re.match — match at the start of string only
if re.match(r"^\d{4}-\d{2}-\d{2}", log):
    print("Log has valid date prefix")
```

### Compiled patterns (reuse for performance)

```python
# Compile once, use many times — good for processing log files
instance_re = re.compile(r"i-[a-z0-9]{8,17}")

with open("app.log") as f:
    for line in f:
        ids = instance_re.findall(line)
        if ids:
            print(ids)
```

---

## 10. subprocess

Run shell commands from Python — the backbone of any cloud automation or deployment script.

```python
import subprocess

# Syntax
subprocess.run(<cmd_as_list>, capture_output=True, text=True, check=False)
# capture_output=True  → capture stdout and stderr instead of printing them
# text=True            → return output as string instead of bytes
# check=True           → raise CalledProcessError if command exits non-zero
```

### Run a command

```python
result = subprocess.run(["ls", "-la"], capture_output=True, text=True)

print(result.stdout)       # command output as string
print(result.stderr)       # any error output
print(result.returncode)   # 0 = success
```

### Raise automatically on failure

```python
# check=True raises CalledProcessError if the command exits non-zero
result = subprocess.run(
    ["git", "status"],
    capture_output=True,
    text=True,
    check=True
)
print(result.stdout)
```

### Handy wrapper

```python
def run(cmd: list[str]) -> str:
    """Run a command, return its stdout. Raises on failure."""
    return subprocess.run(
        cmd, capture_output=True, text=True, check=True
    ).stdout.strip()

branch  = run(["git", "branch", "--show-current"])
version = run(["python", "--version"])
print(f"On branch: {branch}")
```

### Stream output in real time

```python
# Use Popen when you can't wait for the command to finish
with subprocess.Popen(
    ["terraform", "apply", "-auto-approve"],
    stdout=subprocess.PIPE,
    text=True
) as proc:
    for line in proc.stdout:
        print(line, end="")
```

### shell=True — use with caution

```python
# Lets you write the command as a plain string
result = subprocess.run("echo hello && ls", shell=True, capture_output=True, text=True)

# WARNING: never use shell=True with user-supplied input — shell injection risk
```

### Common patterns

```python
# Check if a tool is installed
result = subprocess.run(["which", "docker"], capture_output=True)
docker_installed = result.returncode == 0

# Run an AWS CLI command and parse JSON output
import json
result = subprocess.run(
    ["aws", "ec2", "describe-instances", "--output", "json"],
    capture_output=True, text=True, check=True
)
data = json.loads(result.stdout)

# Run a deploy script
subprocess.run(["python", "deploy.py", "--env", "prod"], check=True)
```

---

## 11. Modules & Packages

### Importing

```python
import os                              # full module
import json                            # full module
from pathlib import Path               # specific name
from datetime import datetime          # specific name
import subprocess as sp                # alias

print(os.environ.get("HOME"))
print(datetime.now())
```

### os — environment & filesystem

```python
import os

# Environment variables (essential for secrets & config)
db_host = os.environ.get("DB_HOST", "localhost")   # with default
api_key = os.environ["API_KEY"]                    # raises KeyError if missing

# Filesystem
os.getcwd()                              # current working directory
os.listdir(".")                          # list directory contents
os.path.exists("config.json")            # check if path exists
os.makedirs("logs/2024", exist_ok=True)  # create nested dirs safely
```

### sys — runtime info

```python
import sys

sys.argv       # CLI arguments: ["script.py", "--env", "prod"]
sys.exit(1)    # exit with error code
sys.platform   # "linux", "darwin", "win32"
sys.version    # Python version string
```

### logging — proper log output

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s"
)

logging.info("Instance started successfully")
logging.warning("CPU usage above 80%")
logging.error("Failed to connect to database")
```

> Use `logging` instead of `print()` in any script that runs unattended — you get timestamps, levels, and the ability to redirect to files.

### Virtual environments

Always isolate project dependencies.

```bash
python -m venv .venv              # create
source .venv/bin/activate         # activate (Linux/macOS)
.venv\Scripts\activate            # activate (Windows)
deactivate                        # deactivate
```

### pip — package management

```bash
pip install boto3                     # install
pip install boto3==1.34.0             # specific version
pip install -r requirements.txt       # from file
pip freeze > requirements.txt         # export current deps
pip list                              # list installed
pip uninstall boto3                   # remove
```

### Useful standard library modules

| Module | Use |
|--------|-----|
| `os` | Env vars, filesystem, paths |
| `sys` | Runtime info, CLI args, exit codes |
| `subprocess` | Run shell commands |
| `pathlib` | Modern file path handling |
| `json` | JSON encode/decode |
| `re` | Regular expressions |
| `logging` | Proper log output |
| `datetime` | Dates and times |
| `collections` | `Counter`, `defaultdict`, `deque` |
| `argparse` | Parse CLI arguments in scripts |
| `time` | Sleep, timing |
| `unittest` | Built-in testing |

---

## 12. CP Essentials

A focused reference for competitive programming. Nothing here is new — just what actually comes up.

### collections

```python
from collections import Counter, defaultdict, deque

# Counter — frequency map
nums = [1, 2, 2, 3, 3, 3]
c = Counter(nums)
c[3]                  # 3
c.most_common(2)      # [(3, 3), (2, 2)]

words = "the cat sat on the mat".split()
Counter(words).most_common(3)

# defaultdict — dict that never raises KeyError
graph = defaultdict(list)
graph["a"].append("b")    # no need to check if "a" exists first
graph["a"].append("c")

freq = defaultdict(int)
for ch in "hello":
    freq[ch] += 1         # no KeyError on first access

# deque — O(1) append/pop from both ends
dq = deque([1, 2, 3])
dq.appendleft(0)          # [0, 1, 2, 3]
dq.popleft()              # 0  → O(1), unlike list.pop(0) which is O(n)
```

### Sorting

```python
students = [("Alice", 90), ("Bob", 75), ("Eve", 85)]
students.sort(key=lambda s: s[1], reverse=True)   # by score, descending

# sorted() returns a new list — doesn't modify original
top3 = sorted(students, key=lambda s: s[1], reverse=True)[:3]
```

### Comprehensions & generator expressions

```python
squares      = [x**2 for x in range(10)]
evens        = [x for x in range(20) if x % 2 == 0]

# Nested
flat = [n for row in [[1,2],[3,4]] for n in row]   # [1, 2, 3, 4]

# Dict comprehension
s = "hello world"
freq = {ch: s.count(ch) for ch in set(s)}

# Generator expression — no list built in memory
total = sum(x**2 for x in range(1_000_000))
```

### Useful built-ins for CP

```python
min(3, 1, 4)                           # 1
max([3, 1, 4])                         # 4
sum([1, 2, 3])                         # 6
abs(-5)                                # 5
pow(2, 10, 1000)                       # 24  (fast modular exponentiation)
divmod(17, 5)                          # (3, 2) → quotient and remainder

any([False, True, False])              # True
all([True, True, True])                # True
zip([1,2,3], ["a","b","c"])            # [(1,"a"), (2,"b"), (3,"c")]
enumerate(["a","b","c"], start=1)      # (1,"a"), (2,"b"), (3,"c")

INF = float("inf")                     # useful for min/max tracking
```

### Unpacking

```python
a, b, c = [1, 2, 3]
first, *rest = [1, 2, 3, 4, 5]    # first=1, rest=[2,3,4,5]
*init, last  = [1, 2, 3, 4, 5]    # init=[1,2,3,4], last=5
a, b = b, a                        # swap — no temp variable needed
```

### Input patterns for CP

```python
n = int(input())                              # single integer
a, b = map(int, input().split())              # two integers on one line
nums = list(map(int, input().split()))        # list of integers
grid = [input() for _ in range(n)]           # n lines of input
```

---

## Quick Reference

### Truthy & Falsy values

```python
# Falsy: False, None, 0, 0.0, "", [], {}, set()
# Everything else is truthy

if not my_list:
    print("List is empty")

if response.get("errors"):
    print("There were errors")
```

### f-string formatting

```python
pi = 3.14159
print(f"{pi:.2f}")       # 3.14        (2 decimal places)
print(f"{1000:,}")       # 1,000       (thousands separator)
print(f"{'hi':>10}")     # "        hi" (right-align in 10 chars)
print(f"{42:05d}")       # 00042       (zero-padded)
```

### Walrus operator `:=` (Python 3.8+)

```python
# Assign and test in one expression
while chunk := f.read(8192):
    process(chunk)

if (n := len(data)) > 10:
    print(f"Too many items: {n}")
```

---

*References: [Python Docs](https://docs.python.org/3/) · [boto3 Docs](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) · [Real Python](https://realpython.com/)*