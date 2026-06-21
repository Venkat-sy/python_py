# 🐍 Python Mastery: The Detailed Hands-On Guide

This guide is an extensive, hands-on deep dive into Python. It is designed to take you beyond the basics with comprehensive, practical examples for real-world application.

---

## 1. Variables, Memory, & Advanced Data Types

Python is dynamically typed and handles memory automatically. Variables are essentially references (pointers) to objects in memory.

### Strings: Methods & Slicing
Strings are immutable, meaning they cannot be changed after creation.
```python
text = "  Python is awesome!  "

# String Methods
cleaned = text.strip()               # "Python is awesome!"
uppercase = cleaned.upper()          # "PYTHON IS AWESOME!"
replaced = cleaned.replace("!", ".") # "Python is awesome."
words = cleaned.split(" ")           # ['Python', 'is', 'awesome!']
joined = "-".join(words)             # "Python-is-awesome!"

# String Slicing [start:stop:step]
phrase = "Data Science"
print(phrase[0:4])   # "Data"
print(phrase[-7:])   # "Science" (Negative indexing starts from the end)
print(phrase[::-1])  # "ecneicS ataD" (Reverses the string)
```

### Mutability vs Immutability (Shallow vs Deep Copy)
Lists and Dictionaries are mutable. If you assign them to a new variable, they point to the exact same memory location.
```python
import copy

original_list = [[1, 2], [3, 4]]

# Assignment (Same memory reference)
ref_list = original_list

# Shallow Copy (Copies top level, but inner lists are still references)
shallow = original_list.copy()

# Deep Copy (Completely independent copy)
deep = copy.deepcopy(original_list)

original_list[0][0] = 99

print(ref_list[0][0])  # 99 (Affected)
print(shallow[0][0])   # 99 (Affected because inner lists are references)
print(deep[0][0])      # 1  (Unaffected!)
```

---

## 2. Deep Control Flow & Iteration

### Truthy & Falsy Values
In Python, you don't always need explicitly boolean `True`/`False`.
*   **Falsy**: `0`, `""` (empty string), `[]`, `{}`, `None`, `False`
*   **Truthy**: Any non-zero number, non-empty string, list, etc.

```python
data = []
if not data: # Elegant way to check if a list is empty
    print("No data available.")
```

### Enumerate and Zip
Avoid `range(len(list))` when you need the index. Use `enumerate`.
```python
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]

# Using Enumerate
for index, name in enumerate(names, start=1):
    print(f"Person {index}: {name}")

# Using Zip (combining lists)
for name, age in zip(names, ages):
    print(f"{name} is {age} years old.")
```

### Structural Pattern Matching (Python 3.10+)
Similar to `switch-case` in other languages but much more powerful.
```python
def http_status(status):
    match status:
        case 200 | 201:
            return "Success"
        case 404:
            return "Not Found"
        case 500:
            return "Server Error"
        case _: # Default case
            return "Unknown Status"

print(http_status(404)) # Not Found
```

---

## 3. Data Structures: The Standard Library

### List Sorting and Custom Keys
```python
employees = [
    {"name": "Alice", "salary": 70000},
    {"name": "Bob", "salary": 50000},
    {"name": "Charlie", "salary": 120000}
]

# Sort by salary ascending
employees.sort(key=lambda x: x["salary"])
print(employees)

# Or use sorted() to return a new list
highest_paid = sorted(employees, key=lambda x: x["salary"], reverse=True)
```

### Advanced Sets (Math Operations)
Sets are incredibly fast for math-like operations.
```python
dev_ops = {"Alice", "Bob", "Eve"}
sys_admins = {"Bob", "Charlie"}

print(dev_ops.intersection(sys_admins)) # {'Bob'}
print(dev_ops.union(sys_admins))        # {'Alice', 'Bob', 'Charlie', 'Eve'}
print(dev_ops.difference(sys_admins))   # {'Alice', 'Eve'}
```

### The `collections` Module
Built-in advanced data structures.
```python
from collections import Counter, defaultdict

# 1. Counter: Automatically count occurrences
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
word_counts = Counter(words)
print(word_counts)           # Counter({'apple': 3, 'banana': 2, 'cherry': 1})
print(word_counts.most_common(1)) # [('apple', 3)]

# 2. DefaultDict: Never throw a KeyError again
# If a key doesn't exist, it defaults to the type provided (e.g., int gives 0, list gives [])
grouped_data = defaultdict(list)
grouped_data["group_A"].append("Alice")
print(grouped_data["group_A"]) # ['Alice']
print(grouped_data["group_B"]) # [] (Instead of KeyError)
```

---

## 4. Functions as First-Class Citizens

### Closures & Decorators
In Python, functions can be passed as arguments, returned, and nested.

**Closures**: A function that remembers the values in its enclosing scope.
```python
def multiplier(factor):
    def multiply(number):
        return number * factor
    return multiply

double = multiplier(2)
triple = multiplier(3)

print(double(5)) # 10
print(triple(5)) # 15
```

**Decorators**: They wrap a function to extend its behavior.
```python
import time

def authenticate(func):
    def wrapper(user, *args, **kwargs):
        if user.get("is_admin"):
            return func(user, *args, **kwargs)
        else:
            raise PermissionError("User is not admin")
    return wrapper

@authenticate
def delete_database(user):
    print("Database deleted!")

admin_user = {"name": "Alice", "is_admin": True}
guest_user = {"name": "Bob", "is_admin": False}

delete_database(admin_user) # Works
# delete_database(guest_user) # Raises PermissionError
```

---

## 5. The Ultimate Object-Oriented Programming (OOP) Masterclass

### The Four Pillars of OOP
Python supports all four pillars of OOP: Encapsulation, Inheritance, Polymorphism, and Abstraction.

**1. Encapsulation (Access Modifiers)**
Python doesn't have strict `public/private` keywords. Instead, it uses naming conventions:
*   `_variable`: Protected (Internal use, but accessible).
*   `__variable`: Private (Name mangled, harder to access).

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner          # Public
        self._currency = "USD"      # Protected
        self.__balance = balance    # Private (Name Mangled)

    def get_balance(self):
        return self.__balance       # Accessing private variable within the class

account = BankAccount("Alice", 1000)
print(account.owner)                # Alice
print(account._currency)            # USD (Discouraged, but works)
# print(account.__balance)          # AttributeError!
print(account._BankAccount__balance)# 1000 (How to access name-mangled variables)
```

**2. Inheritance & Method Resolution Order (MRO)**
Classes can inherit from other classes. Python supports *Multiple Inheritance*.
```python
class Animal:
    def speak(self): return "..."

class Mammal(Animal):
    def feed_young(self): return "Feeding milk"

class Dog(Mammal):
    def speak(self):
        return "Woof!"

dog = Dog()
print(dog.speak())       # Woof! (Overrides Animal's speak)
print(dog.feed_young())  # Inherited from Mammal

# Multiple Inheritance & MRO
class Flyer:
    def move(self): return "Flying"

class Swimmer:
    def move(self): return "Swimming"

class Duck(Flyer, Swimmer):
    pass

# MRO determines which 'move' method is called
print(Duck.mro()) # [Duck, Flyer, Swimmer, object]
print(Duck().move()) # Flying (Because Flyer is first in MRO)
```

**3. Polymorphism & Duck Typing**
"If it walks like a duck and quacks like a duck, it's a duck." You don't need to explicitly declare interfaces.
```python
class Bird:
    def fly(self): print("Flap flap")

class Airplane:
    def fly(self): print("Vroom vroom")

# Polymorphic function
def make_it_fly(entity):
    entity.fly() # We don't care about the type, only that it has a 'fly' method

make_it_fly(Bird())
make_it_fly(Airplane())
```

**4. Abstraction (Abstract Base Classes)**
Used to enforce that subclasses must implement specific methods.
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * (self.radius ** 2)

# shape = Shape() # TypeError: Can't instantiate abstract class
circle = Circle(5)
print(circle.area()) # 78.5
```

### Magic Methods (Dunder Methods)
Magic methods allow you to define how your objects interact with built-in Python syntax (like `+`, `str()`, etc).
```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        # Called when using print()
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):
        # Allows you to use the '+' operator between objects
        return Vector(self.x + other.x, self.y + other.y)

    def __eq__(self, other):
        # Allows you to use '=='
        return self.x == other.x and self.y == other.y

v1 = Vector(2, 4)
v2 = Vector(3, -1)
v3 = v1 + v2

print(v3) # Vector(5, 3)
print(v1 == Vector(2, 4)) # True
```

### Class Methods and Static Methods
*   **Instance Methods**: Take `self`, can modify object state.
*   **Class Methods**: Take `cls`, can modify class state, often used as alternative constructors.
*   **Static Methods**: Take neither, behave like plain functions inside a class namespace.

```python
import datetime

class Employee:
    raise_amount = 1.04 # Class variable

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    @classmethod
    def set_raise_amount(cls, amount):
        cls.raise_amount = amount

    @classmethod
    def from_string(cls, emp_str):
        # Alternative constructor
        name, salary = emp_str.split("-")
        return cls(name, float(salary))

    @staticmethod
    def is_workday(day):
        # Does not need self or cls
        if day.weekday() == 5 or day.weekday() == 6:
            return False
        return True

emp1 = Employee.from_string("John-70000")
print(emp1.name) # John
```

### The `@property` Decorator
Allows you to define methods that can be accessed like attributes, giving you control over getters, setters, and deleters.
```python
class Person:
    def __init__(self, first, last):
        self.first = first
        self.last = last

    @property
    def email(self):
        return f"{self.first}.{self.last}@email.com".lower()

    @property
    def fullname(self):
        return f"{self.first} {self.last}"

    @fullname.setter
    def fullname(self, name):
        first, last = name.split(" ")
        self.first = first
        self.last = last

p = Person("John", "Doe")
print(p.email) # john.doe@email.com

# Uses the setter
p.fullname = "Jane Smith"
print(p.email) # jane.smith@email.com
```

---

## 6. Modern File & Data Handling

### The `pathlib` Module
The modern, object-oriented way to handle file paths (better than `os.path`).
```python
from pathlib import Path

# Create a path object
folder_path = Path("my_project")
folder_path.mkdir(exist_ok=True) # Create folder safely

file_path = folder_path / "config.txt" # '/' operator joins paths cleanly

# Write and read directly
file_path.write_text("API_KEY=12345")
print(file_path.read_text())

# Iterate over all .txt files
for txt_file in folder_path.glob("*.txt"):
    print(txt_file.name)
```

### JSON and CSV Handling
```python
import json
import csv

# JSON Handling
data = {"user_id": 101, "permissions": ["read", "write"]}

# Write JSON to file
with open("data.json", "w") as f:
    json.dump(data, f, indent=4) # indent makes it pretty

# Read JSON
with open("data.json", "r") as f:
    loaded_data = json.load(f)
    print(loaded_data["permissions"])

# CSV Handling
# Writing CSV
with open("users.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["Name", "Age"])
    writer.writerows([["Alice", 25], ["Bob", 30]])

# Reading CSV as a Dictionary
with open("users.csv", "r") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["Name"], "is", row["Age"])
```

---

## 7. Advanced Exception Handling

### Creating Custom Exceptions
Sometimes built-in exceptions aren't descriptive enough for your domain.
```python
class InsufficientFundsError(Exception):
    """Raised when an account has insufficient funds for a transaction."""
    pass

class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError(f"Cannot withdraw {amount}. Balance is {self.balance}.")
        self.balance -= amount
        return self.balance

account = BankAccount(100)
try:
    account.withdraw(150)
except InsufficientFundsError as e:
    print(f"Transaction failed: {e}")
```

### Exception Chaining (`raise from`)
Useful when you catch an exception but want to raise a different one while preserving the traceback.
```python
def query_database():
    try:
        # Simulate a database failure
        1 / 0
    except ZeroDivisionError as e:
        # Raise a custom error but link it to the original issue
        raise ConnectionError("Failed to connect to the database") from e
```

---

## 8. Generators and Itertools

Generators evaluate lazily (one at a time), which saves massive amounts of memory.

### Generator Expressions
Like list comprehensions, but with parentheses.
```python
import sys

# List comprehension: Computes entirely in memory immediately
my_list = [x * x for x in range(10000)]
print(sys.getsizeof(my_list)) # ~80,000 bytes

# Generator expression: Computes on the fly
my_gen = (x * x for x in range(10000))
print(sys.getsizeof(my_gen)) # ~100 bytes (Massive savings!)

# Getting values from a generator
print(next(my_gen)) # 0
print(next(my_gen)) # 1
```

### The `itertools` Module
Advanced iteration tools.
```python
import itertools

# Infinite counting
# for i in itertools.count(10, 2): # 10, 12, 14, 16...

# Combinations and Permutations
items = ["A", "B", "C"]
print(list(itertools.combinations(items, 2))) # [('A', 'B'), ('A', 'C'), ('B', 'C')]
print(list(itertools.permutations(items, 2))) # Order matters: ('A', 'B') & ('B', 'A')

# Chain (Flattening lists)
list1 = [1, 2]
list2 = [3, 4]
chained = list(itertools.chain(list1, list2)) # [1, 2, 3, 4]
```
