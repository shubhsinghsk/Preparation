
-----

## 🐍 Python OOP: Classes & Objects (Enhanced Review)

### 1\. **Core Concepts: Class & Object**

| Concept | Definition (What) | Role (Why) | Code Snippet |
| :--- | :--- | :--- | :--- |
| **Class** | **Blueprint** or template (User-defined data structure). | Binds data (attributes) and functions (methods) into a **single unit**. | `python\nclass Person:\n    # Code defines state & behavior\n    pass\n` |
| **Object** | An **instance** of a class. A real-life entity. | Used to **perform actions** by accessing the class's attributes and methods. | `python\njessa = Person() # Instantiation\n` |

#### 🧠 **Object Properties** (State and Behavior)

  * **State (Attributes):** Variables that reflect the properties of the object (e.g., `name`, `age`).
  * **Behavior (Methods):** Functions that represent what the object can do and can **modify its state** (e.g., `walk()`, `change_age()`).

### 2\. **Attributes (Variables): Instance vs. Class**

| Attribute Type | Binding/Scope | Sharing/Uniqueness | Declaration Location |
| :--- | :--- | :--- | :--- |
| **Instance Variable** | **Bound to the Object** | **Unique** to each object. Each object gets its own copy. | Typically inside the **`__init__()`** method (using `self.variable`). |
| **Class Variable** | **Bound to the Class** | **Shared** by all objects. Only one copy exists for the class. | Declared **inside the class**, but **outside** of any method. |

#### **Code Example: Attributes**

```python
class Student:
    # Class Variable (Shared by ALL students)
    school_name = 'Tech Academy'

    def __init__(self, name, roll_no):
        # Instance Variables (Unique to EACH student object)
        self.name = name
        self.roll_no = roll_no

# Access:
s1 = Student("Alice", 101)
print(s1.name)            # Alice (Instance access)
print(Student.school_name) # Tech Academy (Class access)
```

-----

## 3\. **Methods: The Three Types (Crucial for Interviews)**

### 3.1. **Instance Method**

| Characteristic | Detail |
| :--- | :--- |
| **Binding** | Bound to the **Object**. |
| **First Arg** | **`self`** (mandatory) - refers to the object instance. |
| **Access** | Can access and modify **both** Instance Variables and Class Variables. |
| **Purpose** | Used to access or modify the **Object's state**. |

```python
class Car:
    def __init__(self, speed):
        self.speed = speed
        
    # Instance Method Example
    def accelerate(self, increment):
        # Accesses and modifies the instance state (self.speed)
        self.speed += increment 
        print(f"Current speed: {self.speed} mph")
```

### 3.2. **Class Method**

| Characteristic | Detail |
| :--- | :--- |
| **Binding** | Bound to the **Class**. |
| **First Arg** | **`cls`** (mandatory) - refers to the class itself. |
| **Access** | Can access and modify **Class Variables** (Cannot access instance variables). |
| **Purpose** | Used to access or modify the **Class's state**. Often used as **Factory Methods** to create objects in flexible ways. |
| **Syntax** | Uses the **`@classmethod`** decorator. |

```python
class Item:
    discount_rate = 0.10 # Class Variable

    @classmethod
    def change_discount(cls, new_rate):
        # Accesses and modifies the class variable (cls.discount_rate)
        cls.discount_rate = new_rate
        print(f"Discount updated to {cls.discount_rate}")

# Usage:
Item.change_discount(0.25) # Called on the Class
```

### 3.3. **Static Method**

| Characteristic | Detail |
| :--- | :--- |
| **Binding** | Bound to the **Class**, but has **no knowledge** of the class or object state. |
| **First Arg** | **None** (takes normal arguments). |
| **Access** | **Cannot** access or modify any Class or Instance Variables. |
| **Purpose** | General **utility methods** that logically belong to the class but do not interact with its data. Performs tasks in **isolation**. |
| **Syntax** | Uses the **`@staticmethod`** decorator. |

```python
class MathUtils:
    @staticmethod
    def add(x, y):
        # Performs a calculation without using any class/instance data
        return x + y

# Usage:
result = MathUtils.add(5, 3) 
print(f"Result: {result}") # Called on the Class
```

-----

## 4\. **Instantiation and Construction**

  * **Instantiation:** The process of creating an object.
  * **Constructor (`__init__`)**: A special method used to **initialize** the newly created object's state (its instance variables).

<!-- end list -->

```python
# Constructor Example
class Dog:
    # __init__ method is the constructor
    def __init__(self, name, age):
        self.name = name  # Initialize instance variable
        self.age = age    # Initialize instance variable

dog1 = Dog("Buddy", 3) # __init__ is called here
```


-----



### 2\. **Creating & Initializing**

  * **Class Creation:** Use the `class` keyword.
    ```python
    class ClassName:
        """Docstring for class description"""
        # Attributes and methods go here
    ```
  * **Object Creation (Instantiation):**
    ```python
    obj_name = ClassName(arguments)
    ```
  * **Constructor:** The special method that **initializes** the object.
      * `__new__`: Internally **creates** the object.
      * `__init__()`: Implements the constructor to **initialize** the object's state (i.e., instance variables).


## 🚮 Clean Up & Placeholders

  * **`del` Keyword:**
      * `del obj_name.attribute`: **Deletes an object property** (attribute).
      * `del obj_name`: **Deletes the entire object**.
  * **`pass` Statement:**
      * A **null statement** (No Operation/NOP).
      * Used as a **placeholder** for an empty block (class, function, loop) where code will be added later, since an empty block is not allowed by the Python interpreter.

-----



pass Statement:

A null statement (No Operation/NOP).

Used as a placeholder for an empty block (class, function, loop) where code will be added later, since an empty block is not allowed by the Python interpreter.
-----
