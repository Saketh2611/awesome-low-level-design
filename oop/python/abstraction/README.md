# Abstraction in Python

## Introduction

**Abstraction** is one of the four fundamental principles of Object-Oriented Programming (OOP). It allows you to hide **implementation details** while exposing only the necessary parts of an object. This helps in reducing complexity and increasing maintainability.

Abstraction in Python is mainly achieved using:
1. **Abstract Classes**
2. **Interfaces (via Abstract Base Classes - ABCs)**

## **What is Abstraction?**

**Abstraction** means showing only the **essential details** and hiding the **implementation**. It allows programmers to focus on **what an object does** rather than **how it does it**.

### **Key Benefits of Abstraction**
- **Reduces complexity**: Hides unnecessary implementation details.
- **Increases code reusability**: Encourages the reuse of abstracted logic.
- **Enhances security**: Protects internal object details from unintended modifications.
- **Improves maintainability**: Makes code easier to manage and update.

---

## **1. Abstraction Using Abstract Classes**

An **abstract class** in Python is a class that **cannot be instantiated**. It is used to define common behavior that multiple subclasses should implement.

Python provides the `ABC` module to define abstract classes.

### **Example: Abstract Class in Python**

```python
from abc import ABC, abstractmethod

# Abstract class
class Vehicle(ABC):
    def __init__(self, brand):
        self.brand = brand
    
    @abstractmethod
    def start(self):
        pass  # Abstract method (must be implemented by subclasses)
    
    def display_brand(self):
        print(f"Brand: {self.brand}")

# Subclass implementing the abstract method
class Car(Vehicle):
    def start(self):
        print("Car is starting...")

# Usage
my_car = Car("Toyota")
my_car.display_brand()
my_car.start()
```

### **Output:**
```
Brand: Toyota
Car is starting...
```

**Why Use Abstract Classes?**
- Allows defining common behavior that subclasses must implement.
- Enables partial abstraction (can have both abstract and concrete methods).
- Prevents direct instantiation of base classes.

---

## **2. Abstraction Using Interfaces (Abstract Base Classes - ABCs)**

An **interface** in Python can be implemented using **Abstract Base Classes (ABCs)**, which enforce method implementation.

### **Example: Interface in Python**

```python
from abc import ABC, abstractmethod

# Defining an interface
class Animal(ABC):
    @abstractmethod
    def make_sound(self):
        pass

# Implementing the interface in Dog class
class Dog(Animal):
    def make_sound(self):
        print("Dog barks")

# Implementing the interface in Cat class
class Cat(Animal):
    def make_sound(self):
        print("Cat meows")

# Usage
my_dog = Dog()
my_dog.make_sound()

my_cat = Cat()
my_cat.make_sound()
```

### **Output:**
```
Dog barks
Cat meows
```

**Why Use Interfaces?**
- Promotes **full abstraction** (hides all implementation details).
- Provides a standard way for different classes to implement behaviors.

---

## **Abstract Class vs Interface: Key Differences**

| Feature | Abstract Class | Interface (ABCs) |
|---------|---------------|------------------|
| Methods | Can have abstract and concrete methods | Only abstract methods |
| Fields | Can have instance variables | Should not have instance variables |
| Constructor | Can have constructors | Cannot have constructors |
| Multiple Inheritance | Supported | Supported |
| Access Modifiers | Can have private, protected, public members | Methods are public by default |

---

## **Real-World Example: Payment System**

Abstraction is widely used in real-world applications, such as payment processing.

### **Example: Payment System with Abstraction**

```python
from abc import ABC, abstractmethod

# Abstract class for Payment
class Payment(ABC):
    def __init__(self, amount):
        self.amount = amount
    
    @abstractmethod
    def pay(self):
        pass

# Implementing payment methods
class CreditCardPayment(Payment):
    def pay(self):
        print(f"Paid {self.amount} using Credit Card")

class PayPalPayment(Payment):
    def pay(self):
        print(f"Paid {self.amount} using PayPal")

# Usage
payment = CreditCardPayment(150.75)
payment.pay()

payment = PayPalPayment(200.50)
payment.pay()
```

### **Output:**
```
Paid 150.75 using Credit Card
Paid 200.50 using PayPal
```

**Why Use Abstraction in Payment Systems?**
- Allows multiple payment methods without modifying existing code.
- Improves maintainability and scalability.
- Provides a **common contract** for different payment types.

**✅ What happens if you remove ABC and @abstractmethod?**

If you rewrite your code like this:

# No ABC, no abstractmethod
```
class Payment:
    def __init__(self, amount):
        self.amount = amount
    
    def pay(self):
        pass

class CreditCardPayment(Payment):
    def pay(self):
        print(f"Paid {self.amount} using Credit Card")

class PayPalPayment(Payment):
    def pay(self):
        print(f"Paid {self.amount} using PayPal")
```
👉 Your program will still run perfectly fine.
No error at all.
```
Output:

Paid 150.75 using Credit Card
Paid 200.5 using PayPal
```

So you might think:

“Then why use ABC at all?”

That’s the key part 👇

***🚨 Problem WITHOUT abstract class***

Now suppose someone does this:
```
payment = Payment(100)
payment.pay()
```

What happens?

**✔ It runs**
**✔ But pay() does nothing (empty method)**

This is dangerous in real systems, because:

Payment is meant to be a concept, not a real payment

But Python allows creating it

Silent bugs appear

## Example real bug:
```
def process_payment(payment: Payment):
    payment.pay()   # expects real payment

process_payment(Payment(500))   # OOPS
```

No error → but payment not executed ❌

**✅ With ABC + @abstractmethod**

Your original code:
```
class Payment(ABC):
    @abstractmethod
    def pay(self):
        pass
```

Now try:
```
payment = Payment(100)
```

Python error:
```
TypeError: Can't instantiate abstract class Payment with abstract method pay
```

✔ Prevents invalid objects
✔ Enforces design contract
✔ Forces subclasses to implement pay()

***🎯 Key Concept: Interface Enforcement***

ABC ensures:

“Any payment type MUST implement pay()”

***Without ABC → optional***
***With ABC → mandatory***

🧠 Real-world analogy

Without ABC:

You create “Vehicle”

**Forget to implement drive()**

**Program still runs 🚨**

With ABC:

***Python forces all vehicles to implement drive()***

Safe architecture ✅

✅ When ABC is important

Use abstract classes when:

You define a base concept

Base class should NOT be instantiated

Methods must be implemented

Designing frameworks / large systems

Examples:

Payment systems

ML model base classes

Database connectors

API clients

✔ Final answer

👉 If you remove ABC and @abstractmethod:

Code still runs

No enforcement of method implementation

Base class can be instantiated

Bugs possible

Weaker design

👉 With ABC:

Cannot instantiate base class

Must implement methods

Safer architecture

Better OOP design

