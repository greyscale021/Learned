# Python Notes
> Focused on cloud engineering — automation, scripting, AWS SDK, and DevOps tooling.

<details>
    <summary><strong>Things to be clear</strong></summary>

1. This note assumes you know the basics. It's a reference, not a beginner guide.
2. Sections are ordered by how often you'll actually use them in DevOps work.
3. Every section has real-world context — not just syntax.

</details>

---

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
12. [argparse — CLI Scripts](#12-argparse--cli-scripts)
13. [Environment Variables & Config](#13-environment-variables--config)
14. [requests — HTTP Calls](#14-requests--http-calls)
15. [boto3 — AWS SDK](#15-boto3--aws-sdk)
16. [pytest — Testing Basics](#16-pytest--testing-basics)
17. [CP Essentials](#17-cp-essentials)

---

## 1. Getting Started

Python is interpreted, dynamically typed, and general-purpose. Blocks are defined by **indentation**, not braces.

```bash
python --version        # verify install
python script.py        # run a script
python                  # open REPL
pip --version           # package manager
```

**Key rules:**
- 4-space indentation is not optional
- Variables and functions: `snake_case`, Classes: `PascalCase`
- Never declare types explicitly (dynamic typing)

---

## 2. Variables & Data Types

```python
name      = "Alice"     # str
age       = 30          # int
price     = 9.99        # float
is_active = True        # bool
nothing   = None        # NoneType
```

### Type checking & conversion

```python
type(42)              # <class 'int'>
isinstance(42, int)   # True

int("42")             # 42
float("3.14")         # 3.14
str(100)              # "100"
bool(0)               # False  (0, "", [], None → all falsy)
```

### Type annotations

Don't affect runtime but improve readability — use them in scripts others will read.

```python
def greet(name: str, times: int = 1) -> str:
    return f"Hello, {name}!" * times
```

### String formatting

> Strings are immutable — methods return new strings, they don't modify in place.

```python
name, score = "Alice", 98.67

# f-strings
f"{name} scored {score:.1f}"    # "Alice scored 98.7"
f"{42:05d}"                     # "00042"  (zero padded)
f"{1000:,}"                     # "1,000"  (thousands separator)
f"{'hi':>10}"                   # "        hi"  (right-align)

# Case
"hello".upper()                 # "HELLO"
"hello world".title()           # "Hello World"

# Whitespace
"  hello  ".strip()             # "hello"

# Search
"hello".find("l")               # 2  (-1 if not found)
"error: disk".startswith("error")  # True

# Split & Join
"a,b,c".split(",")              # ["a", "b", "c"]
", ".join(["a", "b", "c"])      # "a, b, c"

# Replace
"hello".replace("l", "r")       # "herro"

# Validation
"123".isdigit()   # True
"abc".isalpha()   # True
```

---

## 3. Control Flow

### if / elif / else

```python
score = int(input())

if score >= 90:
    grade = "A"
elif score >= 75:
    grade = "B"
else:
    grade = "F"

# Ternary
grade = "A" if score >= 90 else "not A"
```

### for loop

```python
services = ["ec2", "s3", "lambda"]

for service in services:
    print(service)

# With index
for i, service in enumerate(services, start=1):
    print(f"{i}: {service}")

# Range
for i in range(0, 10, 2):   # 0 2 4 6 8
    print(i)
```

### while loop

```python
retries = 0
while retries < 3:
    print(f"Attempt {retries + 1}")
    retries += 1
```

### Loop control

```python
for n in range(10):
    if n == 3: continue    # skip
    if n == 7: break       # exit
    print(n)
else:
    print("Done")   # runs only if no break
```

### match statement

```python
match command:
    case "quit":  print("Quitting...")
    case "help":  print("Showing help...")
    case _:       print(f"Unknown: {command}")
```

---

## 4. Functions

Functions are first-class objects — they can be passed around, assigned, and returned.

```python
def add(a: int, b: int) -> int:
    return a + b
```

### Default arguments

```python
def connect(host: str, port: int = 22) -> None:
    print(f"Connecting to {host}:{port}")

connect("192.168.1.1")        # uses port 22
connect("192.168.1.1", 80)    # uses port 80
```

### *args and **kwargs

```python
# *args — extra positional args as a tuple
def order(food: str, *toppings: str) -> None:
    print(f"{food} with:", *toppings)

# **kwargs — extra keyword args as a dict
def log_event(**kwargs) -> None:
    for key, value in kwargs.items():
        print(f"{key}: {value}")

log_event(level="INFO", service="ec2", message="Instance started")
```

### Unpacking into a function call

```python
config = {"host": "10.0.0.1", "port": 443}
connect(**config)   # unpacks dict as keyword args
```

### Lambda

```python
instances = [{"id": "i-3", "cpu": 80}, {"id": "i-1", "cpu": 20}]
instances.sort(key=lambda x: x["cpu"])
# [{"id": "i-1", "cpu": 20}, {"id": "i-3", "cpu": 80}]
```

---

## 5. Data Structures

### List — ordered, mutable

```python
regions = ["us-east-1", "us-west-2", "eu-west-1"]

# Access
regions[0]      # "us-east-1"
regions[-1]     # "eu-west-1"
regions[0:2]    # ["us-east-1", "us-west-2"]

# Modify
regions.append("ap-south-1")
regions.insert(1, "ca-central-1")
regions.remove("us-west-2")     # by value
del regions[0]                  # by index
popped = regions.pop()          # removes & returns last

# Useful
len(regions)
regions.sort()
"us-east-1" in regions   # True
```

### Tuple — ordered, immutable

```python
endpoint = ("https://api.example.com", 443)
host, port = endpoint   # unpacking

# Tuples are hashable → can be dict keys
cache = {("us-east-1", "ec2"): "data"}
```

### Set — unordered, unique elements

```python
active = {"ec2", "s3", "ec2"}   # → {"ec2", "s3"}

active.add("lambda")
active.discard("s3")    # no error if missing

# Set operations — useful for diffing resource lists
deployed = {"ec2", "rds", "s3"}
expected = {"ec2", "s3", "lambda"}
missing  = expected - deployed    # {"lambda"}
extra    = deployed - expected    # {"rds"}
```

### Dictionary — key-value pairs

```python
instance = {"id": "i-abc123", "type": "t3.micro", "state": "running"}

# Access
instance["id"]
instance.get("region", "unknown")   # default if key missing

# Modify
instance["state"] = "stopped"
instance.update({"region": "us-east-1"})
del instance["region"]

# Iterate
for key in instance:                  print(key)
for value in instance.values():       print(value)
for key, value in instance.items():   print(key, value)   # most common

"id" in instance   # True (checks keys)
```

### Comprehensions

```python
instances = [
    {"id": "i-1", "state": "running", "cost": 10},
    {"id": "i-2", "state": "stopped", "cost": 5},
    {"id": "i-3", "state": "running", "cost": 20},
]

# Filter
running = [i for i in instances if i["state"] == "running"]

# Extract field
ids = [i["id"] for i in instances]

# Dict comprehension
id_to_state = {i["id"]: i["state"] for i in instances}

# Generator (lazy, no list in memory)
total_cost = sum(i["cost"] for i in instances)   # 35
```

---

## 6. File Handling

Always use `with` — guarantees the file closes even if an error occurs.

```python
# Read entire file
with open("config.txt", "r", encoding="utf-8") as f:
    content = f.read()

# Line by line (memory-efficient for large logs)
with open("app.log", "r") as f:
    for line in f:
        print(line.strip())

# Write
with open("report.txt", "w", encoding="utf-8") as f:
    f.write("Deployment report\n")
```

### File modes

| Mode | Description |
|------|-------------|
| `"r"` | Read — error if file doesn't exist |
| `"w"` | Write — creates file, overwrites if exists |
| `"a"` | Append — adds to end if exists |
| `"x"` | Create — error if file already exists |
| `"b"` | Binary mode (`"rb"`, `"wb"`) |

### JSON (very common in cloud scripts)

```python
import json

# File → dict
with open("config.json", "r") as f:
    config = json.load(f)

# Dict → file
with open("output.json", "w") as f:
    json.dump(config, f, indent=2)

# String ↔ dict (for API responses)
data = json.loads('{"region": "us-east-1"}')   # str → dict
text = json.dumps(data, indent=2)               # dict → str
```

### pathlib (recommended over os.path)

```python
from pathlib import Path

p = Path("logs/app.log")
p.exists()           # True/False
p.suffix             # ".log"
p.stem               # "app"
p.parent             # Path("logs")
p.read_text()        # read content
p.write_text("hi")   # write content

log_file = Path("logs") / "2024" / "app.log"   # safe path joining
```

---

## 7. Error Handling

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
    print("Success!")       # only if no exception
finally:
    print("Always runs.")   # cleanup
```

### Raising exceptions

```python
def set_port(port: int) -> None:
    if not isinstance(port, int):
        raise TypeError(f"Expected int, got {type(port).__name__}")
    if not (1 <= port <= 65535):
        raise ValueError(f"Invalid port: {port}")
```

### Common exceptions

| Exception | When |
|-----------|------|
| `ValueError` | Right type, wrong value (`int("abc")`) |
| `TypeError` | Wrong type (`"a" + 1`) |
| `KeyError` | Dict key not found |
| `IndexError` | List index out of range |
| `FileNotFoundError` | File doesn't exist |
| `ZeroDivisionError` | Dividing by zero |
| `ImportError` | Module not found |

---

## 8. OOP Basics

> In DevOps you mostly *read* OOP (SDKs like boto3), not write complex hierarchies. Know enough to understand what you're looking at.

```python
class EC2Instance:
    def __init__(self, instance_id: str, instance_type: str) -> None:
        self.instance_id   = instance_id
        self.instance_type = instance_type
        self.state         = "stopped"

    def start(self) -> None:
        self.state = "running"
        print(f"{self.instance_id} is now running.")

    def __repr__(self) -> str:
        return f"EC2Instance(id={self.instance_id!r}, state={self.state!r})"


inst = EC2Instance("i-abc123", "t3.micro")
inst.start()
print(inst)
```

### Inheritance

```python
class AWSResource:
    def __init__(self, resource_id: str, region: str) -> None:
        self.resource_id = resource_id
        self.region      = region

    def describe(self) -> str:
        return f"{self.resource_id} in {self.region}"


class S3Bucket(AWSResource):
    def __init__(self, name: str, region: str) -> None:
        super().__init__(name, region)
        self.name = name

    def describe(self) -> str:
        return f"S3 Bucket: {self.name} ({self.region})"
```

### What to recognize in boto3 code

```python
import boto3

ec2 = boto3.resource("ec2")
instance = ec2.Instance("i-abc123")

instance.start()               # method call on object
print(instance.state["Name"]) # state is a dict: {"Code": 16, "Name": "running"}
print(instance.instance_type)
```

---

## 9. Regular Expressions

```python
import re

log = "2024-12-01 ERROR [ec2] Instance i-abc123 failed to start"

re.search(r"i-[a-z0-9]+", log).group()   # "i-abc123"
re.findall(r"ERROR.+", log)              # list of matches
re.sub(r"\s+", " ", "too   many spaces") # "too many spaces"
re.match(r"^\d{4}-\d{2}-\d{2}", log)    # match at start only
```

### Common patterns

| Pattern | Matches |
|---------|---------|
| `.` | Any character except newline |
| `\d` | Digit |
| `\w` | Word character (`[a-zA-Z0-9_]`) |
| `\s` | Whitespace |
| `^` | Start of string |
| `$` | End of string |
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | Optional (0 or 1) |
| `(...)` | Capture group |

### Compiled patterns (for log files)

```python
instance_re = re.compile(r"i-[a-z0-9]{8,17}")

with open("app.log") as f:
    for line in f:
        ids = instance_re.findall(line)
        if ids:
            print(ids)
```

---

## 10. subprocess

Run shell commands from Python — backbone of cloud automation scripts.

```python
import subprocess

result = subprocess.run(
    ["ls", "-la"],
    capture_output=True,   # capture stdout/stderr
    text=True,             # return as string, not bytes
    check=True             # raise on non-zero exit
)

print(result.stdout)       # command output
print(result.stderr)       # error output
print(result.returncode)   # 0 = success
```

### Reusable wrapper

```python
def run(cmd: list[str]) -> str:
    """Run a command, return stdout. Raises on failure."""
    return subprocess.run(
        cmd, capture_output=True, text=True, check=True
    ).stdout.strip()

branch  = run(["git", "branch", "--show-current"])
version = run(["python", "--version"])
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

### Common patterns

```python
# Check if a tool is installed
docker_installed = subprocess.run(["which", "docker"], capture_output=True).returncode == 0

# Run AWS CLI and parse JSON output
import json
result = subprocess.run(
    ["aws", "ec2", "describe-instances", "--output", "json"],
    capture_output=True, text=True, check=True
)
data = json.loads(result.stdout)
```

> `shell=True` lets you write commands as strings but **never use it with user input** — shell injection risk.

---

## 11. Modules & Packages

```python
import os
import json
from pathlib import Path
from datetime import datetime
import subprocess as sp
```

### os — environment & filesystem

```python
import os

# Env vars
db_host = os.environ.get("DB_HOST", "localhost")   # with default
api_key = os.environ["API_KEY"]                    # KeyError if missing

# Filesystem
os.getcwd()
os.listdir(".")
os.path.exists("config.json")
os.makedirs("logs/2024", exist_ok=True)
```

### sys — runtime info

```python
import sys

sys.argv       # CLI args: ["script.py", "--env", "prod"]
sys.exit(1)    # exit with error code
sys.platform   # "linux", "darwin", "win32"
```

### logging — use this instead of print()

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s"
)

logging.info("Instance started")
logging.warning("CPU above 80%")
logging.error("Failed to connect")
```

> `logging` gives you timestamps, levels, and file output. Use it in any script that runs unattended.

### Virtual environments

```bash
python -m venv .venv
source .venv/bin/activate     # Linux/macOS
deactivate
```

### pip

```bash
pip install boto3
pip install boto3==1.34.0
pip install -r requirements.txt
pip freeze > requirements.txt
```

### Standard library reference

| Module | Use |
|--------|-----|
| `os` | Env vars, filesystem |
| `sys` | Runtime info, exit codes |
| `subprocess` | Run shell commands |
| `pathlib` | File path handling |
| `json` | JSON encode/decode |
| `re` | Regular expressions |
| `logging` | Proper log output |
| `datetime` | Dates and times |
| `argparse` | CLI argument parsing |
| `time` | Sleep, timing |
| `collections` | Counter, defaultdict, deque |
| `unittest` | Built-in testing |

---

## 12. argparse — CLI Scripts

`argparse` turns your scripts into proper CLI tools with flags, help text, and validation — essential for any script someone else (or a pipeline) will run.

```python
import argparse

parser = argparse.ArgumentParser(description="EC2 management script")

# Positional argument (required)
parser.add_argument("instance_id", help="EC2 instance ID")

# Optional flags
parser.add_argument("--region",  default="us-east-1", help="AWS region")
parser.add_argument("--dry-run", action="store_true",  help="Preview only, no changes")
parser.add_argument("--count",   type=int, default=1,  help="Number of instances")

args = parser.parse_args()

print(args.instance_id)   # "i-abc123"
print(args.region)        # "us-east-1"
print(args.dry_run)       # True/False
print(args.count)         # integer
```

```bash
# Usage:
python script.py i-abc123 --region us-west-2 --dry-run
python script.py --help    # auto-generated help text
```

### Subcommands (for multi-action scripts)

```python
parser = argparse.ArgumentParser()
subparsers = parser.add_subparsers(dest="command")

# Define subcommands
start_parser = subparsers.add_parser("start", help="Start an instance")
start_parser.add_argument("instance_id")

stop_parser = subparsers.add_parser("stop", help="Stop an instance")
stop_parser.add_argument("instance_id")

args = parser.parse_args()

if args.command == "start":
    print(f"Starting {args.instance_id}")
elif args.command == "stop":
    print(f"Stopping {args.instance_id}")
```

```bash
python ec2.py start i-abc123
python ec2.py stop  i-abc123
```

---

## 13. Environment Variables & Config

In real DevOps scripts, **secrets and config always come from env vars — never hardcoded**. This is non-negotiable for anything that touches AWS.

```python
import os

# Reading env vars
region     = os.environ.get("AWS_REGION", "us-east-1")   # with default
access_key = os.environ["AWS_ACCESS_KEY_ID"]              # hard fail if missing
secret_key = os.environ.get("AWS_SECRET_ACCESS_KEY")      # None if missing

# Fail early if critical vars are missing
required = ["AWS_ACCESS_KEY_ID", "AWS_SECRET_ACCESS_KEY", "DB_PASSWORD"]
missing  = [var for var in required if not os.environ.get(var)]
if missing:
    raise EnvironmentError(f"Missing required env vars: {missing}")
```

### .env files with python-dotenv

In development you load vars from a `.env` file instead of setting them manually.

```bash
pip install python-dotenv
```

```bash
# .env file (never commit this to git)
AWS_REGION=us-east-1
DB_HOST=localhost
DB_PASSWORD=supersecret
```

```python
from dotenv import load_dotenv
import os

load_dotenv()   # loads .env into os.environ

region = os.environ.get("AWS_REGION")
```

### .gitignore — always add this

```
.env
*.env
.venv/
__pycache__/
```

### Config pattern for scripts

```python
import os
from dataclasses import dataclass

@dataclass
class Config:
    region:      str = os.environ.get("AWS_REGION", "us-east-1")
    bucket_name: str = os.environ.get("S3_BUCKET", "")
    dry_run:     bool = os.environ.get("DRY_RUN", "false").lower() == "true"

config = Config()
print(config.region)
```

---

## 14. requests — HTTP Calls

`requests` is the standard library for HTTP in Python — health checks, webhooks, APIs, Slack notifications.

```bash
pip install requests
```

```python
import requests

# GET
response = requests.get("https://api.example.com/status")
print(response.status_code)    # 200
print(response.json())         # dict if response is JSON
print(response.text)           # raw string

# POST with JSON body
response = requests.post(
    "https://api.example.com/deploy",
    json={"env": "prod", "version": "1.2.3"},
    headers={"Authorization": "Bearer my-token"}
)

# With timeout (always set this in scripts)
response = requests.get("https://api.example.com/health", timeout=5)
```

### Always check the response

```python
response = requests.get("https://api.example.com/data", timeout=10)
response.raise_for_status()   # raises HTTPError if 4xx or 5xx
data = response.json()
```

### Error handling

```python
import requests

try:
    response = requests.get("https://api.example.com", timeout=5)
    response.raise_for_status()
    data = response.json()
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.ConnectionError:
    print("Could not connect")
except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e.response.status_code}")
```

### Health check pattern (common in DevOps)

```python
import logging
import requests

def is_healthy(url: str, timeout: int = 5) -> bool:
    try:
        response = requests.get(url, timeout=timeout)
        return response.status_code == 200
    except requests.exceptions.RequestException:
        return False

if not is_healthy("https://myapp.com/health"):
    logging.error("Health check failed — app may be down")
```

### Sending a Slack notification

```python
def notify_slack(webhook_url: str, message: str) -> None:
    requests.post(webhook_url, json={"text": message}, timeout=5)

notify_slack(os.environ["SLACK_WEBHOOK"], "Deployment to prod complete ✅")
```

---

## 15. boto3 — AWS SDK

boto3 is how you interact with AWS from Python. This is the most important section for your stack.

```bash
pip install boto3
```

### Two interfaces

```python
import boto3

# client — low-level, 1:1 with AWS API, returns raw dicts
client = boto3.client("ec2", region_name="us-east-1")

# resource — higher-level, object-oriented, cleaner for most tasks
ec2 = boto3.resource("ec2", region_name="us-east-1")
```

> Use `client` when you need full API control. Use `resource` for everyday operations — it's cleaner.

### Authentication

boto3 picks up credentials automatically in this order:
1. Environment variables (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`)
2. `~/.aws/credentials` file (set up with `aws configure`)
3. IAM role (when running on EC2 or Lambda — preferred in production)

```bash
aws configure   # sets up ~/.aws/credentials
```

```python
# Explicit (for scripts where you need to control it)
session = boto3.Session(
    aws_access_key_id=os.environ["AWS_ACCESS_KEY_ID"],
    aws_secret_access_key=os.environ["AWS_SECRET_ACCESS_KEY"],
    region_name="us-east-1"
)
ec2 = session.resource("ec2")
```

---

### EC2

```python
import boto3

ec2 = boto3.resource("ec2", region_name="us-east-1")
client = boto3.client("ec2", region_name="us-east-1")

# List all running instances
instances = ec2.instances.filter(
    Filters=[{"Name": "instance-state-name", "Values": ["running"]}]
)
for instance in instances:
    print(instance.id, instance.instance_type, instance.state["Name"])

# Start / stop / terminate
instance = ec2.Instance("i-abc123")
instance.start()
instance.stop()
instance.terminate()

# Describe via client (raw dict response)
response = client.describe_instances(InstanceIds=["i-abc123"])
state = response["Reservations"][0]["Instances"][0]["State"]["Name"]
print(state)   # "running"

# Create an instance
instance = ec2.create_instances(
    ImageId="ami-0c55b159cbfafe1f0",   # Amazon Linux 2 — replace with your region's AMI
    InstanceType="t3.micro",
    MinCount=1,
    MaxCount=1,
    KeyName="my-key-pair",
    TagSpecifications=[{
        "ResourceType": "instance",
        "Tags": [{"Key": "Name", "Value": "my-instance"}]
    }]
)[0]
print(f"Created: {instance.id}")
```

---

### S3

```python
import boto3

s3 = boto3.resource("s3")
client = boto3.client("s3")

# List all buckets
for bucket in s3.buckets.all():
    print(bucket.name)

# Create a bucket (us-east-1 has no LocationConstraint)
client.create_bucket(Bucket="my-new-bucket")
# For other regions:
client.create_bucket(
    Bucket="my-new-bucket",
    CreateBucketConfiguration={"LocationConstraint": "us-west-2"}
)

# Upload a file
s3.Bucket("my-bucket").upload_file("local_file.txt", "s3_key.txt")
# or:
client.upload_file("local_file.txt", "my-bucket", "s3_key.txt")

# Download a file
s3.Bucket("my-bucket").download_file("s3_key.txt", "local_copy.txt")

# List objects in a bucket
bucket = s3.Bucket("my-bucket")
for obj in bucket.objects.all():
    print(obj.key, obj.size)

# Delete an object
s3.Object("my-bucket", "s3_key.txt").delete()

# Read file content directly (without downloading)
obj = s3.Object("my-bucket", "config.json")
content = obj.get()["Body"].read().decode("utf-8")
config = json.loads(content)

# Generate a pre-signed URL (temporary access link)
url = client.generate_presigned_url(
    "get_object",
    Params={"Bucket": "my-bucket", "Key": "file.txt"},
    ExpiresIn=3600   # 1 hour
)
```

---

### IAM

```python
import boto3

iam = boto3.client("iam")

# List users
response = iam.list_users()
for user in response["Users"]:
    print(user["UserName"], user["CreateDate"])

# Create a user
iam.create_user(UserName="deploy-bot")

# Attach a managed policy to a user
iam.attach_user_policy(
    UserName="deploy-bot",
    PolicyArn="arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"
)

# Create access keys for a user
keys = iam.create_access_key(UserName="deploy-bot")
print(keys["AccessKey"]["AccessKeyId"])
print(keys["AccessKey"]["SecretAccessKey"])   # only shown once

# List attached policies for a user
policies = iam.list_attached_user_policies(UserName="deploy-bot")
for p in policies["AttachedPolicies"]:
    print(p["PolicyName"])
```

---

### Error handling in boto3

```python
from botocore.exceptions import ClientError, NoCredentialsError

try:
    s3.Bucket("my-bucket").upload_file("file.txt", "file.txt")
except NoCredentialsError:
    print("AWS credentials not found")
except ClientError as e:
    error_code = e.response["Error"]["Code"]
    if error_code == "NoSuchBucket":
        print("Bucket doesn't exist")
    else:
        print(f"AWS error: {error_code}")
```

### Pagination (for large result sets)

AWS API responses are paginated — never assume you got everything in one call.

```python
# Wrong — may miss results
response = client.list_objects_v2(Bucket="my-bucket")

# Right — use paginator
paginator = client.get_paginator("list_objects_v2")
for page in paginator.paginate(Bucket="my-bucket"):
    for obj in page.get("Contents", []):
        print(obj["Key"])
```

---

### Practical boto3 script template

```python
#!/usr/bin/env python3
"""List all EC2 instances and their states across a region."""

import boto3
import logging
import os
from botocore.exceptions import ClientError, NoCredentialsError

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s"
)

def list_instances(region: str) -> None:
    ec2 = boto3.resource("ec2", region_name=region)
    try:
        for instance in ec2.instances.all():
            logging.info(f"{instance.id} | {instance.instance_type} | {instance.state['Name']}")
    except NoCredentialsError:
        logging.error("AWS credentials not configured")
    except ClientError as e:
        logging.error(f"AWS error: {e.response['Error']['Code']}")

if __name__ == "__main__":
    region = os.environ.get("AWS_REGION", "us-east-1")
    list_instances(region)
```

---

## 16. pytest — Testing Basics

Tests prove your scripts work and don't break when you change things — essential for portfolio projects.

```bash
pip install pytest
```

### Basic test structure

```python
# File must be named test_*.py or *_test.py
# Functions must start with test_

# test_utils.py
from utils import add, set_port
import pytest

def test_add_positive():
    assert add(2, 3) == 5

def test_add_negative():
    assert add(-1, -2) == -3

def test_add_zero():
    assert add(0, 0) == 0
```

```bash
pytest              # run all tests
pytest -v           # verbose output
pytest test_utils.py   # specific file
```

### Testing exceptions

```python
def test_invalid_port():
    with pytest.raises(ValueError):
        set_port(99999)

def test_wrong_type():
    with pytest.raises(TypeError):
        set_port("not-a-number")
```

### Fixtures — reusable setup

```python
import pytest
import boto3
from moto import mock_aws   # pip install moto[s3]  (mock_s3 deprecated in moto v4+)

@pytest.fixture
def s3_bucket():
    """Create a mock S3 bucket for testing."""
    with mock_aws():
        s3 = boto3.resource("s3", region_name="us-east-1")
        bucket = s3.create_bucket(Bucket="test-bucket")
        yield bucket

def test_upload_file(s3_bucket):
    s3_bucket.put_object(Key="test.txt", Body=b"hello")
    obj = s3_bucket.Object("test.txt").get()
    assert obj["Body"].read() == b"hello"
```

### Parametrize — test multiple inputs

```python
@pytest.mark.parametrize("port,expected", [
    (22,    True),
    (443,   True),
    (0,     False),
    (65536, False),
])
def test_valid_port(port, expected):
    if expected:
        set_port(port)   # should not raise
    else:
        with pytest.raises(ValueError):
            set_port(port)
```

---

## 17. CP Essentials

### collections

```python
from collections import Counter, defaultdict, deque

# Counter
nums = [1, 2, 2, 3, 3, 3]
c = Counter(nums)
c[3]                    # 3
c.most_common(2)        # [(3, 3), (2, 2)]

# defaultdict
graph = defaultdict(list)
graph["a"].append("b")

freq = defaultdict(int)
for ch in "hello":
    freq[ch] += 1

# deque — O(1) append/pop from both ends
dq = deque([1, 2, 3])
dq.appendleft(0)   # [0, 1, 2, 3]
dq.popleft()       # 0  (O(1) unlike list.pop(0))
```

### Sorting

```python
students = [("Alice", 90), ("Bob", 75), ("Eve", 85)]
students.sort(key=lambda s: s[1], reverse=True)

top3 = sorted(students, key=lambda s: s[1], reverse=True)[:3]
```

### Useful built-ins

```python
min(3, 1, 4)
max([3, 1, 4])
sum([1, 2, 3])
abs(-5)
pow(2, 10, 1000)          # fast modular exponentiation
divmod(17, 5)             # (3, 2)

any([False, True, False]) # True
all([True, True, True])   # True
zip([1,2,3], ["a","b","c"])
enumerate(["a","b","c"], start=1)

INF = float("inf")
```

### Unpacking

```python
a, b, c = [1, 2, 3]
first, *rest = [1, 2, 3, 4, 5]   # first=1, rest=[2,3,4,5]
a, b = b, a                       # swap
```

### Input patterns

```python
n = int(input())
a, b = map(int, input().split())
nums = list(map(int, input().split()))
grid = [input() for _ in range(n)]
```

---

## Quick Reference

### Truthy & Falsy

```python
# Falsy: False, None, 0, 0.0, "", [], {}, set()
if not my_list:
    print("Empty")
```

### f-string formatting

```python
pi = 3.14159
f"{pi:.2f}"       # "3.14"
f"{1000:,}"       # "1,000"
f"{'hi':>10}"     # "        hi"
f"{42:05d}"       # "00042"
```

### Walrus operator (Python 3.8+)

```python
while chunk := f.read(8192):
    process(chunk)

if (n := len(data)) > 10:
    print(f"Too many items: {n}")
```

---

*References: [Python Docs](https://docs.python.org/3/) · [boto3 Docs](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) · [Real Python](https://realpython.com/) · [pytest Docs](https://docs.pytest.org/)*