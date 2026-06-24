# Python Complete Course: The Exhaustive Guide

This guide is an encyclopedic breakdown of every Python topic (excluding OOP, which is covered in a separate masterclass). For each topic, you will find every single subtopic explained in extreme theoretical detail, backed by real-world engineering scenarios and fully implemented code examples.

---

## 1. Variables & Core Data Types

**Theory & Subtopics:**
Python is dynamically typed; you don't explicitly declare types.
*   **Primitives:**
    *   `int`: Whole numbers. Unlimited length in Python.
    *   `float`: Decimal numbers (IEEE 754 double precision).
    *   `complex`: Numbers with real and imaginary parts (e.g., `3 + 4j`).
    *   `bool`: `True` or `False`. Subclasses of integers (`True == 1`, `False == 0`).
*   **Type Checking:** Using `type()` to see the class, and `isinstance()` to check against a type safely.
*   **Type Casting:** Forcing one type into another (e.g., `int("10")`).

**Real-World Example (Scientific Computing & Type Validation):**
A weather application receives raw sensor data as strings. It must validate that the data is numeric, cast it to floats for precise temperature calculation, and use booleans to trigger storm warnings.

**Program Implementation:**
```python
raw_temp_input = "98.6"

# Type Casting
temperature = float(raw_temp_input)

# Complex numbers for advanced mathematics (e.g., electrical engineering phase angles)
phase_angle = 2 + 3j

# Type Checking (Best Practice)
if isinstance(temperature, float):
    print("Valid decimal temperature received.")

# Boolean logic (True is literally 1 under the hood)
is_freezing = temperature <= 32.0
print(f"Is it freezing? {is_freezing}") # False
print(f"Math with bools: {is_freezing + 5}") # Outputs 5 (0 + 5)
```

---

## 2. Operators Deep Dive

**Theory & Subtopics:**
Operators manipulate data. Python has several specific categories:
*   **Arithmetic:** `+`, `-`, `*`, `/` (float division), `//` (floor division - rounds down), `%` (modulo - remainder), `**` (exponentiation).
*   **Assignment:** `=`, `+=`, `-=`, `*=`, etc.
*   **Comparison:** `==`, `!=`, `<`, `>`, `<=`, `>=`. Python supports chained comparisons (`1 < x < 10`).
*   **Logical (Short-Circuiting):** `and`, `or`, `not`. Python evaluates these lazily (short-circuiting).
*   **Identity:** `is`, `is not`. Checks if two variables point to the *exact same memory address* (using `id()`).
*   **Membership:** `in`, `not in`. Checks if a sequence contains a value.
*   **Bitwise:** `&` (AND), `|` (OR), `^` (XOR), `~` (NOT), `<<` (Left Shift). Manipulates data at the binary level.

**Real-World Example (E-Commerce Promotion Engine):**
An online store applies discounts using logical short-circuiting to save database calls, and uses modulo to determine if a user's ID qualifies for a random "every 100th customer" prize.

**Program Implementation:**
```python
user_id = 4500
cart_value = 120.0
is_vip = False

# Arithmetic & Modulo: Every 100th user gets a prize
is_winner = (user_id % 100 == 0)

# Chained Comparison
if 100 <= cart_value <= 500:
    print("Eligible for standard free shipping.")

# Identity vs Equality
list_a = [1, 2, 3]
list_b = [1, 2, 3]
print(list_a == list_b) # True (Values are equal)
print(list_a is list_b) # False (Different objects in memory)

# Membership
allowed_roles = ["admin", "editor"]
print("guest" in allowed_roles) # False

# Bitwise (Used heavily in cryptography and low-level permissions)
# Binary 1010 (10) & 1100 (12) = 1000 (8)
print(10 & 12) 
```

---

## 3. Control Statements

**Theory & Subtopics:**
*   **`if-elif-else` & Nested Ifs:** Standard branching.
*   **Ternary Operator:** A one-line `if-else` statement (`x if condition else y`).
*   **Structural Pattern Matching (`match-case`):** Introduced in Python 3.10, this acts like a super-powered `switch` statement that can unpack data structures.

**Real-World Example (API Routing & Configurations):**
A backend server inspects incoming HTTP request methods (`GET`, `POST`, `DELETE`). It uses a ternary operator to set default ports, and `match-case` to cleanly route the HTTP methods.

**Program Implementation:**
```python
# Ternary Operator: Clean, one-line assignment
environment = "production"
port = 443 if environment == "production" else 8080

# Match-Case (Python 3.10+)
http_method = "POST"

match http_method:
    case "GET":
        print("Fetching data from database...")
    case "POST" | "PUT": # The '|' acts as an OR operator in match cases
        print("Writing new data to database...")
    case "DELETE":
        print("Removing record...")
    case _: # The underscore is the wildcard/default case
        print("Unknown HTTP method.")
```

---

## 4. Loops Exhaustive

**Theory & Subtopics:**
*   **`for` loops & `range()`:** Iterating over iterables. `range(start, stop, step)` generates numbers on the fly.
*   **`while` loops:** Looping based on a condition.
*   **Loop Control (`break`, `continue`, `pass`):** `break` destroys the loop. `continue` skips the current iteration. `pass` is a null placeholder.
*   **The `else` clause in loops:** A unique Python feature. An `else` block attached to a `for/while` loop executes *only* if the loop finishes naturally (meaning it was NEVER interrupted by a `break`).

**Real-World Example (Cybersecurity Port Scanner):**
A script scans a list of network ports to find an open one. If it finds one, it `break`s. If it scans all ports and finds none open, the loop's `else` clause triggers a secure lockdown.

**Program Implementation:**
```python
ports_to_scan = [80, 443, 8080, 22]
target_port = 22

# For loop with a break and an 'else' clause
for port in ports_to_scan:
    if port == target_port:
        print(f"Vulnerability found on port {port}! Halting scan.")
        break # Exit loop immediately
    print(f"Port {port} is secure.")
else:
    # This ONLY runs if the 'break' statement was NEVER hit
    print("Scan complete: No vulnerabilities found anywhere.")

# Continue statement: Skipping logic
for i in range(1, 6): # 1, 2, 3, 4, 5
    if i == 3:
        continue # Skip the number 3
    print(f"Processing item {i}")
```

---

## 5. Strings Deep Dive

**Theory & Subtopics:**
Strings are immutable arrays of Unicode characters.
*   **Indexing & Slicing:** `string[start:stop:step]`. Negative indexing goes backward.
*   **Concatenation & Multiplication:** `"a" + "b"` and `"a" * 3`.
*   **Raw Strings & Escapes:** `\n` (newline), `\t` (tab). Prefixing with `r` (e.g., `r"C:\folder"`) ignores escape characters (vital for Regex and file paths).
*   **Methods:**
    *   Casing: `.upper()`, `.lower()`, `.title()`, `.capitalize()`.
    *   Searching: `.find()`, `.count()`, `.startswith()`, `.endswith()`.
    *   Modification: `.replace()`, `.strip()`, `.split()`, `.join()`.
*   **Formatting:** Legacy `%s`, `.format()`, and modern `f"{vars}"`.

**Real-World Example (Data Pipeline Parsing):**
A data engineer receives a messy, single-line comma-separated log file. They must use string methods to break it apart, clean the whitespace, and format a clean alert message.

**Program Implementation:**
```python
raw_log = "   ERROR, server_timeout, 10:45PM   "

# Cleaning and Splitting
clean_log = raw_log.strip().lower() # "error, server_timeout, 10:45pm"
log_parts = clean_log.split(", ")   # ['error', 'server_timeout', '10:45pm']

# Slicing: Reversing a string
reversed_word = "Python"[::-1] # "nohtyP"

# Raw strings for file paths (prevents \n from becoming a newline)
windows_path = r"C:\new_folder\test.txt"

# Joining a list back into a string
reconstructed = " | ".join(log_parts)

# f-strings with formatting (e.g., padding, decimals)
cpu_usage = 85.567
print(f"Log: {reconstructed}")
print(f"CPU Usage: {cpu_usage:.1f}%") # Rounds to 1 decimal place: 85.6%
```

---

## 6. Lists & Tuples Exhaustive

**Theory & Subtopics:**
*   **Lists (`[]`):** Mutable. Dynamic arrays.
    *   *Methods:* `.append()` (add one), `.extend()` (merge lists), `.insert()`, `.remove()` (by value), `.pop()` (by index/remove last), `.clear()`, `.sort()`, `.reverse()`.
    *   *Copying:* `list.copy()` creates a shallow copy. `import copy; copy.deepcopy(list)` copies nested lists.
    *   *List Comprehensions:* `[expression for item in iterable if condition]`.
*   **Tuples (`()`):** Immutable. Hashable (can be used as dictionary keys).
    *   *Packing/Unpacking:* Extracting multiple values instantly.
    *   *Singleton Tuple:* Requires a comma: `(5,)`.

**Real-World Example (Stock Market Ticker & Immutable Coordinates):**
A trading bot uses a List to track the last 5 stock prices (constantly appending and popping). It uses Tuples to define fixed geographical server locations (which must never be accidentally overwritten).

**Program Implementation:**
```python
# LISTS
stock_prices = [100, 102, 101, 105]
stock_prices.append(108)         # Add to end
stock_prices.insert(0, 99)       # Insert at beginning
popped_price = stock_prices.pop() # Removes and returns 108

# List Comprehension: Square all even prices
even_squares = [price ** 2 for price in stock_prices if price % 2 == 0]

# TUPLES
server_locations = ("NYC", "LON", "TOK")
# server_locations[0] = "SFO" # TypeError! Immutable.

# Tuple Unpacking (Swapping variables instantly)
a, b = 10, 20
a, b = b, a # a is now 20, b is 10

# Returning multiple values from a function creates a Tuple automatically
def get_status():
    return 200, "OK" # Returns (200, "OK")

code, message = get_status() # Unpacking
```

---

## 7. Dictionaries & Sets

**Theory & Subtopics:**
*   **Dictionaries (`{k:v}`):** Hash maps. Keys must be immutable (strings, ints, tuples). Fast O(1) lookups.
    *   *Methods:* `.get(key, default)`, `.keys()`, `.values()`, `.items()`, `.update()`, `.pop()`.
    *   *Dict Comprehensions:* `{k: v for k,v in iterable}`.
*   **Sets (`{}`):** Unordered collections of UNIQUE elements. Highly optimized for math operations.
    *   *Math:* Union (`|`), Intersection (`&`), Difference (`-`), Symmetric Difference (`^`).

**Real-World Example (Social Media Analytics):**
A platform uses Dictionaries to store user profile data (O(1) fast lookup). It uses Sets to find mutual friends between two users instantly via Intersection operations.

**Program Implementation:**
```python
# DICTIONARIES
user_profile = {"id": 1, "username": "admin"}
user_profile.update({"email": "admin@sys.com", "role": "super"})

# Safe retrieval
phone = user_profile.get("phone", "No phone provided")

# Iterating over key-value pairs
for key, value in user_profile.items():
    print(f"{key}: {value}")

# Dict Comprehension: Create a dict mapping numbers to their cubes
cubes = {x: x**3 for x in range(1, 4)} # {1: 1, 2: 8, 3: 27}

# SETS (Removing duplicates & Math)
alice_friends = {"Bob", "Charlie", "David"}
bob_friends = {"Charlie", "David", "Eve"}

print(f"Mutual Friends: {alice_friends & bob_friends}") # Intersection: {'Charlie', 'David'}
print(f"All Unique Friends: {alice_friends | bob_friends}") # Union
print(f"Only Alice's Friends: {alice_friends - bob_friends}") # Difference: {'Bob'}
```

---

## 8. Functions & Scope

**Theory & Subtopics:**
*   **Parameters vs Arguments:** Parameters are variables in the definition; arguments are the actual values passed.
*   **Positional vs Keyword Arguments:** `func(10, b=20)`.
*   **Default Arguments Trap:** NEVER use mutable objects (like `[]` or `{}`) as default arguments; they persist across function calls. Use `None` instead.
*   **`*args` & `**kwargs`:** `*args` packs remaining positional arguments into a Tuple. `**kwargs` packs keyword arguments into a Dictionary.
*   **Scope & `global`:** Variables inside functions are Local. The `global` keyword allows modifying global variables from inside a function.
*   **Lambda Functions:** Anonymous, single-expression functions: `lambda x: x + 1`.

**Real-World Example (Cloud Infrastructure Provisioning):**
A cloud script deploys virtual machines. It requires a mandatory OS type, but can accept an infinite number of optional configuration tags (`**kwargs`).

**Program Implementation:**
```python
# The Mutable Default Argument Trap (How to do it correctly)
def add_item(item, inventory=None):
    if inventory is None:
        inventory = [] # Creates a fresh list every time
    inventory.append(item)
    return inventory

# *args and **kwargs
def deploy_server(os_type, *packages, **configurations):
    print(f"Deploying {os_type}...")
    print(f"Installing packages (Tuple): {packages}")
    
    for config_key, config_val in configurations.items():
        print(f"Applying Config -> {config_key}: {config_val}")

deploy_server("Ubuntu", "nginx", "docker", memory="16GB", region="us-east-1")

# Lambda Function: Usually used inside sorting or filtering
data = [{"name": "Alice", "age": 30}, {"name": "Bob", "age": 25}]
data.sort(key=lambda person: person["age"]) # Sorts dictionary by age
```

---

## 9. Exception Handling

**Theory & Subtopics:**
*   **`try`:** Code that might crash.
*   **`except SpecificError`:** Catching exact errors (e.g., `ValueError`, `KeyError`) rather than a bare `except:`, which catches everything including system exits.
*   **`else`:** Runs ONLY if the `try` block succeeds without errors.
*   **`finally`:** Runs unconditionally. Used for resource cleanup.
*   **`raise`:** Manually triggering an exception.
*   **Custom Exceptions:** Creating specific error classes by inheriting from `Exception`.

**Real-World Example (Database Transactions):**
When transferring money between bank accounts, if a database failure occurs halfway through, the code must catch the `DatabaseError`, roll back the transaction, and `finally` close the connection.

**Program Implementation:**
```python
# Custom Exception
class InsufficientFundsError(Exception):
    pass

def withdraw(balance, amount):
    try:
        if type(amount) is not int:
            raise TypeError("Amount must be an integer.")
        if amount > balance:
            raise InsufficientFundsError("You are too broke for this.")
        
        new_balance = balance - amount
        
    except TypeError as e:
        print(f"Data Error: {e}")
    except InsufficientFundsError as e:
        print(f"Business Logic Error: {e}")
    else:
        # Only runs if NO exceptions occurred
        print(f"Success! Withdrew ${amount}. New balance: ${new_balance}")
        return new_balance
    finally:
        # Always runs
        print("Transaction record closed.")

withdraw(100, 150) # Triggers InsufficientFundsError
```

---

## 10. File Handling & Operating System Interaction

**Theory & Subtopics:**
*   **`open(filename, mode)` Modes:** 
    *   `r`: Read.
    *   `w`: Write (overwrites).
    *   `a`: Append.
    *   `r+`: Read and Write.
    *   `b`: Binary mode (e.g., `rb`, `wb` for images/PDFs).
*   **Context Managers (`with`):** Guarantees the file is closed automatically, even if an exception crashes the program during reading.
*   **Reading:** `.read()` (whole file), `.readline()` (one line), `.readlines()` (list of lines).

**Real-World Example (Processing Application Logs):**
A cybersecurity script opens a massive `access.log` file. It reads it line-by-line (to save RAM) and appends any IP addresses showing suspicious activity into a new `blocked_ips.txt` file.

**Program Implementation:**
```python
# Writing initial data
with open("system.log", "w") as file:
    file.write("INFO: System started\n")
    file.write("ERROR: Unauthorized access from 192.168.1.50\n")

# Reading line-by-line and Appending to another file
with open("system.log", "r") as source_file:
    # Opening a second file simultaneously
    with open("blocked.txt", "a") as target_file:
        for line in source_file:
            if "ERROR" in line:
                # Extract the IP and append it
                ip = line.split("from ")[-1]
                target_file.write(f"BLOCKED: {ip}")

# Verification
with open("blocked.txt", "r") as check_file:
    print(check_file.read())
```
