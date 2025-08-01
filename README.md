# Super_Function-

# Understanding `super()` in Python: A Guide to Single and Multiple Inheritance

This project explains and demonstrates the use of the `super()` function in Python, focusing on **why it’s necessary**, especially in **multiple inheritance**, and how it helps maintain proper method resolution using **MRO (Method Resolution Order)**.

---

## 🚀 Why Do We Need `super()`?

In **single inheritance**, calling a parent method directly works fine. But in **multiple inheritance**, this can create problems like **skipping methods** or **calling the same method multiple times**.

### What is MRO (Method Resolution Order)?
MRO defines the order of method lookup when using inheritance. Python uses C3 linearization to compute the MRO.

### ❗ Problem Without `super()`:
- Hardcoded method calls can **break the MRO chain**.
- Leads to **duplicated** or **missed method executions**.
- Not maintainable in complex hierarchies.

---

## ✅ Single Inheritance Examples

### Without `super()` (Direct Call)

```python
class Parent:
    def greet(self):
        print("Hello from Parent")

class Child(Parent):
    def greet(self):
        print("Hello from Child")
        Parent.greet(self)  # Direct parent call

child = Child()
child.greet()

```

## Output
```
Hello from Child
Hello from Parent
```

With **super()**

```
class Parent:
    def greet(self):
        print("Hello from Parent")

class Child(Parent):
    def greet(self):
        print("Hello from Child")
        super().greet()

child = Child()
child.greet()

```

## Output 

```

Hello from Child
Hello from Parent

```


 ### In single inheritance, both approaches work


 ##  Multiple Inheritance Examples
 
 #### Without super() (Incorrect MRO Handling)

 ```

 class A:
    def greet(self):
        print("Hello from A")

class B:
    def greet(self):
        print("Hello from B")

class C(A, B):
    def greet(self):
        print("Hello from C")
        A.greet(self)  # Manual call, B is skipped

class D(C):
    def greet(self):
        print("Hello from D")
        C.greet(self)

d = D()
d.greet()

```

## Output

```

Hello from D
Hello from C
Hello from A

```

### Problem: Class B is skipped completely, and MRO is ignored.


##  With super() (Correct MRO Handling)

```
class A:
    def greet(self):
        print("Hello from A")
        super().greet()

class B:
    def greet(self):
        print("Hello from B")

class C(A, B):
    def greet(self):
        print("Hello from C")
        super().greet()

class D(C):
    def greet(self):
        print("Hello from D")
        super().greet()

d = D()
d.greet()

```

## Output 

```
Hello from D
Hello from C
Hello from A
Hello from B

```


### All classes are called once in correct MRO order.







