This is a very detailed explanation of instance variables. I will condense it into a focused, scannable format, highlighting the unique properties and key manipulation techniques for interview readiness.

-----

## 💾 Python Instance Variables (Attributes)

### 1\. **Core Concept & Identity**

| Concept | Detail | Key Property |
| :--- | :--- | :--- |
| **Definition** | Variables whose value **varies from object to object**. | They are the **fields or attributes** of an object. |
| **Uniqueness** | **NOT Shared.** Every object gets its own **separate copy** of the instance variable. |
| **Scope** | Bound to the **object instance**; accessible within instance methods using `self`. |

### 2\. **Declaration & Access**

| Action | Technique/Location | Key Syntax |
| :--- | :--- | :--- |
| **Declaration** | Inside the **Constructor** (`__init__`) using the `self` keyword. | `self.name = name` |
| **Access (Outside)** | Using the object reference and the dot operator (`.`). | `s1.name` |
| **Access (Alternative)** | Using the built-in `getattr()` function. | `getattr(s1, 'name')` |

#### **Code Snippet: Declaration & Access**

```python
class Student:
    def __init__(self, name, age):
        # Declared here
        self.name = name 
        self.age = age

s1 = Student("Jessa", 20)
print(s1.name) # Accessing using dot operator
```

### 3\. **Manipulation & Dynamic Changes**

| Action | Method | Notes |
| :--- | :--- | :--- |
| **Modify Value** | Assign a new value directly using the object reference. | `stud.name = 'Emma'` (Change is *only* for that object). |
| **Dynamically Add** | Assign a new variable to the object reference outside the class. | `stud.marks = 75` (Added only to the specific object `stud`, not the class or other objects). |
| **Dynamically Delete** | Use `del` statement or `delattr()` function. | `del s1.name` OR `delattr(s1, 'name')` (Raises `AttributeError` if accessed afterward). |

### 4\. **Instance Variables Across Classes**

  * **Inheritance:** In a child class, you can access the parent's instance variables using the `self` reference, **provided you call the parent's constructor** using `super().__init__()` in the child's `__init__`.

<!-- end list -->

```python
# Parent constructor call ensures parent attributes are initialized
super().__init__() 
print(self.engine) # Accessing parent's instance variable
```

### 5\. **Listing Instance Variables**

  * **Tool:** The built-in **`__dict__`** attribute of an object.
  * **Result:** Returns a **dictionary** where the keys are the instance variable names and the values are their current values.

<!-- end list -->

```python
s1 = Student(10, 'Jessa')
print(s1.__dict__) # Output: {'roll_no': 10, 'name': 'Jessa'}
```

### 6\. **Naming Conventions**

  * **Public:** `variable_name` (all lower case, words separated by underscore).
  * **Non-Public (Protected):** Should begin with a **single underscore** (`_variable_name`).
  * **Name Mangling (Private):** Two leading underscores (`__variable_name`).

-----

This concludes the notes on Instance Variables. Would you like to cover the last major OOP variable type, **Class Variables**, or compile all your notes?
