
-----

## 🏃 Python Instance Methods (Object Behavior)

### 1\. **Core Concept & Definition**

| Concept | Detail | Key Role |
| :--- | :--- | :--- |
| **Definition** | A method that **uses or modifies** the object's instance variables (object state). | Performs actions on the data unique to a specific object. |
| **Binding** | Bound to the **Object Instance**. |
| **Access** | Can **access and modify** object attributes (`self.name`). Can also access class attributes using `self.__class__`. |
| **Default** | **Any method** created inside a class is an instance method unless explicitly decorated otherwise (`@classmethod`, `@staticmethod`). |

#### 🔑 **The `self` Parameter**

  * **Role:** The **first parameter** in an instance method definition.
  * **Reference:** Refers to the **current object** instance calling the method.
  * **Purpose:** Essential for accessing the object's unique attributes and methods.

### 2\. **Defining & Calling**

  * **Defining:** Use the standard `def` keyword, ensuring `self` is the first parameter.
    ```python
    class Student:
        def show(self): # Instance Method
            print(self.name)
    ```
  * **Calling:** Use the object and the dot operator (`.`).
    ```python
    emma = Student("Jessa", 14)
    emma.show() # Call is made through the object instance
    ```

### 3\. **Modifying & Creating Attributes**

| Action | Location | Mechanism | Notes |
| :--- | :--- | :--- | :--- |
| **Modify State** | Inside an instance method. | Direct assignment using `self`. | `self.age = new_age` |
| **Create New Instance Variable** | Inside any instance method (not just `__init__`). | Assignment using `self`. | `self.marks = marks` (This new attribute is specific to the object whose method was called). |

### 4\. **Dynamic Modification (Runtime)**

Python allows methods to be added or deleted at runtime, demonstrating its dynamic nature.

| Action | Method/Tool | Syntax | Notes |
| :--- | :--- | :--- | :--- |
| **Dynamically Add** | `types.MethodType()` (from the `types` module). | `obj.method = types.MethodType(func, obj)` | The new method is added **only to the specific object**, not the class or other instances. |
| **Dynamically Delete** | **`del` operator** OR **`delattr()`** function. | `del obj.method_name` OR `delattr(obj, 'method_name')` | Deletes the method from the specific object instance. Trying to call it afterward raises `AttributeError`. |

-----

