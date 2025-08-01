# Understanding Method Resolution Order (MRO) with `super()` in Python

This Report demonstrates how Python resolves method calls in **multiple inheritance** when parent classes define methods with the **same name but different implementations**. It focuses on the behavior of the `super()` function and how it interacts with the **MRO (Method Resolution Order)**.

---

## 🚀 Scenario: `Human` and `Mammal` Have Different `eat()` Methods

### Example 1: Basic Inheritance Without `super()`

```python
class Human:
    def eat(self):
        print("Human eats with utensils")

class Mammal:
    def eat(self):
        print("Mammal eats instinctively")

class Employee(Human, Mammal):
    pass

e = Employee()
e.eat()
```

### Output:

```
Human eats with utensils
```

### Why?

* Python uses **MRO**: `Employee -> Human -> Mammal -> object`
* `Human.eat()` is found first and executed.

### MRO Check:

```python
print(Employee.__mro__)
# (<class '__main__.Employee'>, <class '__main__.Human'>, <class '__main__.Mammal'>, <class 'object'>)
```

---

## ✅ Example 2: All Classes Use `super()`

```python
class Human:
    def eat(self):
        print("Human eats")
        super().eat()

class Mammal:
    def eat(self):
        print("Mammal eats")
        super().eat()

class Employee(Human, Mammal):
    def eat(self):
        print("Employee eats")
        super().eat()

e = Employee()
e.eat()
```

### Output:

```
Employee eats
Human eats
Mammal eats
```

### MRO Chain:

`Employee -> Human -> Mammal -> object`

* Each `super().eat()` calls the **next method in MRO**.
* `object` has no `eat()` so chain ends safely.

---

## ❌ Example 3: Direct Parent Method Calls (Not Recommended)

```python
class Human:
    def eat(self):
        print("Human eats")

class Mammal:
    def eat(self):
        print("Mammal eats")

class Employee(Human, Mammal):
    def eat(self):
        print("Employee eats")
        Human.eat(self)
        Mammal.eat(self)

e = Employee()
e.eat()
```

### Output:

```
Employee eats
Human eats
Mammal eats
```

### Problem:

* Manual calls **bypass MRO**.
* Can cause **duplicate** or **missed method calls** in complex hierarchies.

---

## ✨ Best Practice: Cooperative Multiple Inheritance with `super()`

Using `super()` in **all classes** ensures a **clean MRO chain**:

```python
class Human:
    def eat(self):
        print("Human eats")
        super().eat()

class Mammal:
    def eat(self):
        print("Mammal eats")
        super().eat()

class Employee(Human, Mammal):
    def eat(self):
        print("Employee eats")
        super().eat()

e = Employee()
e.eat()
```

### Output:

```
Employee eats
Human eats
Mammal eats
```

---

## 🔎 Summary Table

| Inheritance Order         | Method Called First | MRO Chain                          |
| ------------------------- | ------------------- | ---------------------------------- |
| `Employee(Human, Mammal)` | `Human.eat()`       | Employee → Human → Mammal → object |
| Manual method call        | Manual, no MRO      | Risk of duplication/skipping       |
| With `super()`            | Clean, respects MRO | Each method called once in order   |

---

