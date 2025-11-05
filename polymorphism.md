

-----

## ✨ Polymorphism in Python

### 1\. **Core Concept & Definition**

| Concept | Detail | Key Result |
| :--- | :--- | :--- |
| **Polymorphism** | From Greek: "Poly" (many) and "morphism" (forms). The ability of an object or method to **take many forms** or **perform the same action in many different ways.** | Allows a single interface (e.g., a function or method name) to be used for different underlying data types or classes. |
| **Runtime Check**| When a polymorphic method is called, Python checks the **object's type** at runtime to determine which specific implementation to execute. |

#### **Example: Built-in Polymorphism**

The built-in function `len()` is polymorphic:

  * `len('string')` returns the character count.
  * `len([1, 2, 3])` returns the item count.

-----

### 2\. **Implementing Polymorphism**

Polymorphism is primarily implemented in Python through **Method Overriding**, **Duck Typing**, and **Operator Overloading**.

#### 2.1. **Method Overriding (Polymorphism with Inheritance)**

  * **Mechanism:** A child class re-implements (overrides) a method inherited from its parent class, providing a specialized version.
  * **Result:** When the method is called via a child object, the child's version executes; when called via a parent object, the parent's version executes.
  * **Advantage:** Allows different subclasses to extend the base class functionality while maintaining a common method signature (`max_speed()` in `Vehicle` vs. `Car`).

#### 2.2. **Polymorphism with Functions & Duck Typing**

  * **Mechanism:** Defining multiple classes that are **not related by inheritance** but implement the **same method names**. A function can accept any of these objects and call that common method.
  * **Duck Typing:** "If it walks like a duck and quacks like a duck, it is a duck." Python doesn't care about the object's class type, only that it has the method being called.
  * **Example:** A `car_details(obj)` function can successfully call `obj.fuel_type()` on both a `Ferrari` object and a `BMW` object, as long as both classes define `fuel_type()`.

-----

### 3\. **Operator Overloading (Polymorphism with Magic Methods)**

  * **Concept:** Changing the default behavior of an operator (like `+`, `*`, `==`) when used with custom objects.
  * **Mechanism:** Python implements operators using special methods called **Magic Methods** (or Dunder methods). To overload an operator, you must redefine (override) the corresponding magic method in your class.

| Operator | Magic Method | Default Behavior | Overloaded Behavior Example |
| :--- | :--- | :--- | :--- |
| `+` (Addition) | **`__add__(self, other)`** | Arithmetic addition (for numbers) or concatenation (for lists/strings). | Add the `pages` attribute of two `Book` objects. |
| `*` (Multiplication) | **`__mul__(self, other)`** | Arithmetic multiplication. | Calculate total salary by multiplying `Employee.salary` by `Timesheet.days`. |
| `==` (Equality) | **`__eq__(self, other)`** | Checks if two objects are the same in memory. | Check if two `Book` objects have the same number of pages. |

-----

### 4\. **Method Overloading (Unsupported in Python)**

  * **Definition:** Calling the same method with a **different number or type of parameters** (e.g., `area(a)` vs. `area(a, b)`).
  * **Python Behavior:** Python does **not natively support** method overloading. It will only recognize the **latest defined method** and ignore earlier definitions, leading to an error if the number of arguments doesn't match the last definition.
  * **Workaround:** Achieve similar functionality using **default arguments** or checking the number of arguments inside a single function definition.

<!-- end list -->

```python
# Polymorphic workaround using default arguments
def area(self, a, b=0):
    if b > 0:
        # Calculate rectangle area
        pass
    else:
        # Calculate square area
        pass
```

-----

