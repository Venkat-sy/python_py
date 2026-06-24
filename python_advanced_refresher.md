# Python Advanced Refresher: The "I Forgot" Deep Dive

This guide is designed specifically for returning developers. It strips away beginner explanations and focuses heavily on minute details, underlying mechanics, and complex programming patterns to jog your memory.

---

## 1. Memory, Identity, & Scoping (LEGB)

### Identity vs Equality
*   `==` checks if the *values* are equal.
*   `is` checks if they point to the *exact same memory address* (`id(a) == id(b)`).

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b) # True (Values match)
print(a is b) # False (Different objects in memory)
print(a is c) # True (Same object)

# String/Integer Interning
# Python pre-caches small integers (-5 to 256) and short strings.
x = 256
y = 256
print(x is y) # True (Cached)

x = 257
y = 257
print(x is y) # False (Usually, depending on the REPL vs Script execution)
```

### The LEGB Scope Rule
Python resolves variables in this order: **L**ocal, **E**nclosing, **G**lobal, **B**uilt-in.

```python
x = "Global"

def outer():
    x = "Enclosing"
    def inner():
        # nonlocal x  # Un-commenting this binds 'x' to the Enclosing scope
        # global x    # Un-commenting this binds 'x' to the Global scope
        x = "Local"
        print(x)
    inner()
    print(x)

outer() 
# Outputs: 
# Local
# Enclosing
```

---

## 2. Advanced Data Structures & Comprehensions

### Unpacking & the Asterisk `*` Operator
```python
# Extended Unpacking
head, *body, tail = [1, 2, 3, 4, 5]
print(body) # [2, 3, 4]

# Merging Dictionaries (Python 3.5+)
dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
merged = {**dict1, **dict2} # {'a': 1, 'b': 3, 'c': 4} -> Notice 'b' is overwritten by dict2

# Python 3.9+ Merge Operator
merged_modern = dict1 | dict2
```

### Deep Comprehensions
```python
# List comprehension with nested loops and conditionals
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened_evens = [num for row in matrix for num in row if num % 2 == 0] 
# [2, 4, 6, 8]

# Dictionary Comprehension with conditional logic
names = ['Alice', 'Bob', 'Charlie']
name_lengths = {name: len(name) for name in names if len(name) > 3}
# {'Alice': 5, 'Charlie': 7}
```

---

## 3. First-Class Functions, Closures, & Decorators

Functions are objects. You can pass them around.

### Closures
A closure happens when a nested function captures and remembers the local variables from its enclosing scope, even after the outer function has finished executing.
```python
def make_multiplier(factor):
    def multiply(n):
        return n * factor # 'factor' is captured
    return multiply

times_two = make_multiplier(2)
print(times_two(10)) # 20
```

### Advanced Decorators (with arguments and metadata preservation)
```python
from functools import wraps
import time

def retry(max_attempts=3):
    def decorator(func):
        @wraps(func) # Preserves the original function's name and docstring
        def wrapper(*args, **kwargs):
            attempts = 0
            while attempts < max_attempts:
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    attempts += 1
                    print(f"Attempt {attempts} failed: {e}")
                    time.sleep(1)
            raise Exception("All retries failed")
        return wrapper
    return decorator

@retry(max_attempts=2)
def unstable_network_call():
    # Simulate failure
    raise ConnectionError("Network timeout")

# unstable_network_call() # Will retry twice then fail
```

---

## 4. Object-Oriented Programming: Under the Hood

### `__new__` vs `__init__`
*   `__new__` actually *creates* the object instance and returns it.
*   `__init__` merely *initializes* the already-created instance.

```python
class Singleton:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            # Create the object if it doesn't exist
            cls._instance = super().__new__(cls)
        return cls._instance # Return the exact same object every time

    def __init__(self, value=None):
        if value:
            self.value = value

s1 = Singleton("First")
s2 = Singleton("Second")

print(s1 is s2)    # True! They are the exact same object.
print(s1.value)    # "Second" (s2's __init__ overwrote the value)
```

### MRO (Method Resolution Order) & `super()`
Python uses the **C3 Linearization** algorithm to determine the order classes are checked in multiple inheritance.

```python
class A: pass
class B(A): pass
class C(A): pass
class D(B, C): pass

print(D.__mro__) 
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
```

### Memory Optimization with `__slots__`
By default, Python objects store instance attributes in a dictionary (`__dict__`). This takes up a lot of RAM. `__slots__` explicitly tells Python to allocate exactly enough space for specific attributes, preventing the creation of `__dict__`.
```python
class Point:
    __slots__ = ['x', 'y'] # Denies dynamic attribute creation
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(1, 2)
# p.z = 3 # AttributeError: 'Point' object has no attribute 'z'
```

---

## 5. Iterators, Generators, & `yield from`

### The Iterator Protocol
An object is iterable if it implements `__iter__()`. It is an iterator if it implements both `__iter__()` and `__next__()`.

```python
class CustomRange:
    def __init__(self, start, end):
        self.current = start
        self.end = end

    def __iter__(self):
        return self

    def __next__(self):
        if self.current >= self.end:
            raise StopIteration # Tells the for-loop to stop
        val = self.current
        self.current += 1
        return val

for i in CustomRange(1, 4):
    print(i) # 1, 2, 3
```

### Generators & `yield from`
Generators are syntactic sugar for Iterators. `yield from` delegates part of its operations to another generator.

```python
def sub_generator():
    yield 'A'
    yield 'B'

def main_generator():
    yield 1
    yield from sub_generator() # Pauses main, exhausts sub, then resumes
    yield 2

print(list(main_generator())) # [1, 'A', 'B', 2]
```

---

## 6. Context Managers (The `with` statement)

Context managers ensure resource cleanup (like closing files or database connections). You can build them via Classes or via Generators using `contextlib`.

### Class-Based Context Manager
```python
class ManagedFile:
    def __init__(self, filename):
        self.filename = filename

    def __enter__(self):
        print("Opening file...")
        self.file = open(self.filename, 'w')
        return self.file # This is bound to the 'as' variable

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Closing file...")
        self.file.close()
        # Returning True suppresses any exceptions that happened inside the block

with ManagedFile('test.txt') as f:
    f.write('Hello')
```

### Generator-Based (More concise)
```python
from contextlib import contextmanager

@contextmanager
def managed_file_gen(filename):
    print("Opening file...")
    f = open(filename, 'w')
    try:
        yield f # Execution pauses here and hands control back to the 'with' block
    finally:
        print("Closing file...")
        f.close()

with managed_file_gen('test2.txt') as f:
    f.write('World')
```

---

## 7. Advanced Function Toolkit

### Map, Filter, Reduce
While comprehensions are preferred, these functional tools are still prevalent.
```python
from functools import reduce

data = [1, 2, 3, 4, 5]

# Map: Apply function to all
squared = list(map(lambda x: x**2, data)) # [1, 4, 9, 16, 25]

# Filter: Keep if true
evens = list(filter(lambda x: x % 2 == 0, data)) # [2, 4]

# Reduce: Accumulate
# (((1+2)+3)+4)+5
total_sum = reduce(lambda acc, current: acc + current, data) # 15
```

### Type Hinting (Python 3.5+)
Python is dynamic, but Type Hints provide static analysis (used by mypy/IDEs).
```python
from typing import List, Dict, Optional, Callable

# Takes a List of Strings, returns an Optional Integer (int or None)
def process_items(items: List[str]) -> Optional[int]:
    if not items:
        return None
    return len(items)

# Callable: Takes an int, returns a bool
def execute_callback(cb: Callable[[int], bool], value: int):
    return cb(value)
```
