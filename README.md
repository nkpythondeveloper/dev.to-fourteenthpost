# Python’s Multiple Inheritance and Method Resolution Order (MRO)!

## 🚀 Introduction

Python supports multiple inheritance, meaning a class can inherit from multiple parent classes. While this provides flexibility, it also introduces complexity—which method is called when multiple parents define the same method?

That’s where Method Resolution Order (MRO) comes into play!

In this post, we’ll explore:

✅ What is multiple inheritance?

✅ How Python determines which method to call using MRO

✅ The C3 linearization (MRO algorithm)

✅ Using super() effectively in multiple inheritance

✅ Common pitfalls and best practices

## 1️⃣ Understanding Multiple Inheritance

In single inheritance, a class inherits from one parent:

```python

class Parent:
    def greet(self):
        print("Hello from Parent!")

class Child(Parent):
    pass

c = Child()
c.greet()  # Output: Hello from Parent!

```

✅ Simple and predictable—Python looks up greet() in Child, then in Parent.

However, with multiple inheritance, the lookup path becomes more complex:

```python

class A:
    def show(self):
        print("A")

class B:
    def show(self):
        print("B")

class C(A, B):  # Multiple inheritance
    pass

c = C()
c.show()  # Output: A

```
🤔 Why is A.show() called instead of B.show()?

Python follows MRO (Method Resolution Order) to determine which method is called first.

## 2️⃣ What is MRO (Method Resolution Order)?

MRO defines the order in which base classes are searched when executing a method.

Python uses the C3 linearization (also called the "C3 MRO"), which ensures:

    A class appears before its parents.
    Parents maintain their relative order.
    A method is only called once (even with multiple paths).

🔹 Finding MRO using __mro__ or mro()

```bash

print(C.mro())  
# [<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>]

```
or

```bash

print(C.__mro__)  

```

✅ Observation:

    C → A → B → object
    A.show() is called because A comes before B in the MRO.

## 3️⃣ The Diamond Problem in Multiple Inheritance

Consider this classic "Diamond Problem":

```python

class A:
    def show(self):
        print("A")

class B(A):
    def show(self):
        print("B")

class C(A):
    def show(self):
        print("C")

class D(B, C):  
    pass

d = D()
d.show()

```

🤔 Which show() method gets called?

🔹 Check the MRO:

```bash

print(D.mro())  
# [D, B, C, A, object]

```

🔹 Output:

```bash

B

```
✅ Python resolves conflicts using the C3 MRO algorithm, ensuring each class is called only once in the correct order.

## 4️⃣ Using super() in Multiple Inheritance

🔹 Why use super()?

    It ensures all parent classes get initialized properly.
    It follows the MRO order, avoiding duplicate calls.

✅ Correct Way to Use super() in Multiple Inheritance

```python

class A:
    def __init__(self):
        print("A init")
        super().__init__()

class B(A):
    def __init__(self):
        print("B init")
        super().__init__()

class C(A):
    def __init__(self):
        print("C init")
        super().__init__()

class D(B, C):
    def __init__(self):
        print("D init")
        super().__init__()

d = D()

```
🔹 Output:

```bash

D init  
B init  
C init  
A init  

```

✅ Why is this important?

    super().__init__() ensures that all parent constructors run exactly once following the MRO.
    If we used A.__init__(self), A would be called multiple times, breaking consistency.


## 5️⃣ Common Pitfalls and Best Practices

❌ Pitfall 1: Forgetting to Call super()

```python

class A:
    def __init__(self):
        print("A init")

class B(A):
    def __init__(self):
        print("B init")

class C(A):
    def __init__(self):
        print("C init")

class D(B, C):
    def __init__(self):
        print("D init")
        super().__init__()  # Only calls B's init

d = D()

```

🔹 Output:

```bash

D init  
B init  

```

🔹 Problem:

    A is never initialized because B.__init__() doesn’t call super().
    Always use super() to ensure all parent classes are properly initialized.

✅ Best Practices for Multiple Inheritance

✔️ Always use super() instead of calling parent methods explicitly.

✔️ Check MRO using Class.mro() to understand method resolution.

✔️ Use multiple inheritance wisely—prefer composition over deep inheritance trees.

✔️ Avoid unnecessary complexity—only use multiple inheritance when needed.


## 6️⃣ Summary

✔️ Python supports multiple inheritance, but method lookup follows MRO (C3 linearization).
✔️ Use Class.mro() or __mro__ to check the resolution order.
✔️ The "Diamond Problem" is resolved using MRO, ensuring methods are called only once.
✔️ Use super() in multiple inheritance to call all parent classes properly.
✔️ Avoid unnecessary complexity—prefer composition where possible.

## 🚀 What’s Next?

Next, we'll explore Python's "super()" in depth—how it works and best practices for using it in real-world projects!

## 💬 Got questions? Drop them in the comments! 🚀
