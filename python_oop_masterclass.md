# The Complete Python OOP Masterclass

Object-Oriented Programming (OOP) in Python allows you to model real-world concepts as code blocks called **Classes**. Below is an exhaustive breakdown of every OOP topic, complete with programming examples.

---

## 1. Classes, Objects, and the Constructor (`__init__`)
A **Class** is a blueprint. An **Object** is a physical instance built from that blueprint. The `__init__` method is the constructor—it is automatically called when an object is created to set its initial state.

```python
class Dog:
    # Constructor: Initializes the object's attributes
    def __init__(self, name, breed):
        self.name = name   # 'self' refers to the specific object being created
        self.breed = breed
    
    # Instance method
    def bark(self):
        return f"{self.name} says Woof!"

# Creating objects (instantiation)
dog1 = Dog("Buddy", "Golden Retriever")
dog2 = Dog("Max", "German Shepherd")

print(dog1.bark()) # Buddy says Woof!
print(dog2.name)   # Max
```

---

## 2. Instance Variables vs. Class Variables
*   **Instance Variables:** Unique to each object (defined inside `__init__` using `self`).
*   **Class Variables:** Shared among *all* instances of a class (defined outside any method).

```python
class Employee:
    company_name = "TechCorp"  # Class Variable (Shared by all employees)
    
    def __init__(self, name, salary):
        self.name = name       # Instance Variable (Unique to the employee)
        self.salary = salary

emp1 = Employee("Alice", 70000)
emp2 = Employee("Bob", 80000)

print(emp1.company_name) # TechCorp
print(emp2.company_name) # TechCorp

# If we change the class variable, it updates for EVERYONE
Employee.company_name = "GlobalTech"
print(emp1.company_name) # GlobalTech
```

---

## 3. The Three Types of Methods
1.  **Instance Methods:** Take `self` as the first parameter. They can modify object state and access class state.
2.  **Class Methods:** Take `cls` as the first parameter (using `@classmethod`). They can modify class state but not object state.
3.  **Static Methods:** Take neither `self` nor `cls` (using `@staticmethod`). They are just regular functions that happen to live inside a class for organization.

```python
class MathOperations:
    multiplier = 2
    
    def __init__(self, value):
        self.value = value
        
    # Instance Method
    def multiply_value(self):
        return self.value * self.multiplier
        
    # Class Method (Often used as alternative constructors)
    @classmethod
    def change_multiplier(cls, new_multiplier):
        cls.multiplier = new_multiplier
        
    # Static Method (Utility function)
    @staticmethod
    def add(x, y):
        return x + y

# Using the methods
calc = MathOperations(10)
print(calc.multiply_value())          # 20
MathOperations.change_multiplier(5)   # Affects the class
print(calc.multiply_value())          # 50
print(MathOperations.add(5, 7))       # 12
```

---

## 4. Encapsulation (Access Modifiers & Properties)
Encapsulation prevents external code from accidentally changing internal data. Python uses naming conventions to define access:
*   `public_var`: Accessible everywhere.
*   `_protected_var`: Internal use, but technically still accessible.
*   `__private_var`: Name-mangled. Very hard to access from outside.

To safely read/write private variables, we use the `@property` decorator (Getters and Setters).

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.__balance = balance # Private variable
        
    # Getter
    @property
    def balance(self):
        return self.__balance
        
    # Setter
    @balance.setter
    def balance(self, amount):
        if amount < 0:
            raise ValueError("Balance cannot be negative.")
        self.__balance = amount

account = BankAccount("John", 1000)
print(account.balance) # 1000 (Uses getter)
account.balance = 1500 # Uses setter
# account.balance = -500 # Throws ValueError
```

---

## 5. Inheritance
Inheritance allows a child class to inherit properties and methods from a parent class.

**Single Inheritance:**
```python
class Animal:
    def eat(self):
        return "I am eating"

class Cat(Animal): # Inherits from Animal
    def meow(self):
        return "Meow!"

kitty = Cat()
print(kitty.eat())  # Inherited
print(kitty.meow()) # Specific to Cat
```

**The `super()` Function:**
Used to call methods (especially `__init__`) from the parent class.
```python
class Person:
    def __init__(self, name):
        self.name = name

class Student(Person):
    def __init__(self, name, student_id):
        super().__init__(name) # Executes the Parent's constructor
        self.student_id = student_id
```

---

## 6. Multiple Inheritance & MRO (Method Resolution Order)
Python allows a class to inherit from multiple parents. If two parents have the same method, Python uses MRO to decide which one to call (from left to right).

```python
class Flyer:
    def move(self): return "Flying"
    
class Swimmer:
    def move(self): return "Swimming"

class FlyingFish(Flyer, Swimmer):
    pass

fish = FlyingFish()
print(fish.move()) # Flying (Because Flyer is listed first)
print(FlyingFish.mro()) # Shows the resolution order: [FlyingFish, Flyer, Swimmer, object]
```

---

## 7. Polymorphism (Overriding and Duck Typing)
Polymorphism means "many forms". It allows different classes to be treated as if they are the same type.

**Method Overriding:** A child class replaces the parent's method.
```python
class Bird:
    def speak(self): return "Chirp"

class Duck(Bird):
    def speak(self): return "Quack" # Overrides Bird's speak

print(Duck().speak()) # Quack
```

**Duck Typing:** Python doesn't care what type an object is, as long as it has the method being called.
```python
class Car:
    def drive(self): print("Driving a car")

class Boat:
    def drive(self): print("Sailing a boat") # Not related to Car

def start_trip(vehicle):
    vehicle.drive() # It just expects a .drive() method to exist

start_trip(Car())
start_trip(Boat())
```

---

## 8. Abstraction (Abstract Base Classes)
Abstraction hides complex implementation and only shows the necessary features. In Python, we use the `abc` module to force child classes to implement specific methods.

```python
from abc import ABC, abstractmethod

class Shape(ABC): # Inherits from Abstract Base Class
    @abstractmethod
    def area(self):
        pass # No implementation here

class Square(Shape):
    def __init__(self, side):
        self.side = side
        
    def area(self): # MUST be implemented, otherwise Square cannot be instantiated
        return self.side * self.side

# shape = Shape() # TypeError! Cannot instantiate abstract class
sq = Square(4)
print(sq.area()) # 16
```

---

## 9. Magic Methods (Dunder Methods & Operator Overloading)
Double Underscore (`__`) methods allow your classes to interact with built-in Python operators (like `+`, `-`, `==`, `str()`).

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        
    # Defines what is returned when print(object) is called
    def __str__(self):
        return f"Point({self.x}, {self.y})"
        
    # Operator Overloading for the '+' symbol
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
        
    # Operator Overloading for the '==' symbol
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

p1 = Point(1, 2)
p2 = Point(3, 4)

print(p1)           # Point(1, 2)
print(p1 + p2)      # Point(4, 6)
print(p1 == p2)     # False
```

---

## 10. Composition vs Inheritance
*   **Inheritance** represents an "IS-A" relationship (A Dog *is an* Animal).
*   **Composition** represents a "HAS-A" relationship (A Car *has an* Engine). Composition is often preferred to avoid deeply tangled inheritance trees.

```python
class Engine:
    def start(self):
        return "Engine starting..."

class Car:
    def __init__(self):
        self.engine = Engine() # Composition: Car contains an Engine object
        
    def turn_key(self):
        return self.engine.start()

my_car = Car()
print(my_car.turn_key()) # Engine starting...
```

---

## 11. Dataclasses (Modern Python 3.7+)
If you only need a class to store data, `dataclass` automatically writes the `__init__`, `__repr__`, and `__eq__` methods for you.

```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    age: int
    is_active: bool = True # Default value

user1 = User("Alice", 28)
user2 = User("Alice", 28)

print(user1)         # User(name='Alice', age=28, is_active=True)
print(user1 == user2) # True (Automatically checks value equality)
```
