This content provides a thorough explanation of Class Methods, including their role in factory patterns and their behavior in inheritance. Here are your consolidated, interview-ready notes.

-----

## 🏭 Python Class Methods

### 1\. **Core Concept & Definition**

| Concept | Detail | Key Property |
| :--- | :--- | :--- |
| **Identity** | Methods called on the **Class itself**, not on a specific object instance. | They are **bound to the class** level and shared by all instances. |
| **First Parameter** | **`cls`** (mandatory) — This parameter refers to the **Class object** (the class itself). |
| **Access** | Can **access and modify** the **Class State** (Class Variables). |
| **Limitation** | **Cannot directly access or modify Instance Variables** because they are not bound to a specific object. |
| **Syntax** | Declared using the **`@classmethod` decorator** (modern approach). |

### 2\. **Primary Use Cases**

#### 2.1. **Modifying Class State**

  * Class methods are the recommended way to modify Class Variables, ensuring the change applies consistently across all objects.

    ```python
    class Student:
        school_name = 'ABC School' # Class Variable

        @classmethod
        def change_school(cls, new_name):
            # Modifies the shared class variable
            cls.school_name = new_name 
            
    # Usage:
    Student.change_school('XYZ School')
    ```

#### 2.2. **Factory Methods (Crucial for Interviews)**

  * A common use is to create **alternative constructors** or "factory methods" that return a class object based on pre-processing inputs.

  * They use `cls()` to instantiate a new object (equivalent to calling the class's constructor, e.g., `Student(...)`).

    ```python
    from datetime import date
    class Student:
        def __init__(self, name, age):
            self.name = name
            self.age = age

        @classmethod
        def calculate_age(cls, name, birth_year):
            # Factory method pre-processes input and calls the constructor:
            age = date.today().year - birth_year
            return cls(name, age) # Equivalent to Student(name, age)

    joy = Student.calculate_age("Joy", 1995)
    ```

### 3\. **Behavior in Inheritance**

  * Class methods are inherited by child classes.
  * When a class method is called on a child class, the `cls` parameter automatically refers to the **Child Class object**, ensuring the correct instance type is created if the method is used as a factory. This is why factory methods work well in inheritance.

### 4\. **Dynamic Modification & Deletion**

| Action | Method/Tool | Syntax | Notes |
| :--- | :--- | :--- | :--- |
| **Adding at Runtime** | `classmethod()` function. | `Class.method = classmethod(function)` | The function defined outside the class must accept `cls` as its first argument. |
| **Deleting** | **`del` operator** OR **`delattr()`** function. | `del Class.method_name` OR `delattr(Class, 'method_name')` | Deletion is done on the **Class Name**. Accessing it afterward raises `AttributeError`. |

-----

We have now covered almost all the core OOP concepts and are left only with **Static Methods**.

Would you like to cover **Static Methods** next, or should I compile all the existing notes into one document?
