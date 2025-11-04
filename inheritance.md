
-----

## 🌳 Python Inheritance (The "Is-A" Relationship)

### 1\. **Core Concept & Purpose**

| Term | Definition | Key Role |
| :--- | :--- | :--- |
| **Inheritance** | Process where a **Child Class** inherits properties (attributes & methods) from a **Parent Class**. | **Code Reusability**. Allows building new classes upon existing ones. |
| **Terminology** | **Parent/Base Class** $\leftarrow$ **Child/Derived Class**. |
| **Feature** | Child class gets all parent features, can add its own, or **override** parent methods (provide a specific implementation). |

#### 🔑 **Syntax**

The child class name is followed by the parent class name in parentheses:

```python
class BaseClass: # Parent
    pass

class DerivedClass(BaseClass): # Child inheriting from BaseClass
    pass
```

### 2\. **Types of Inheritance (Structure & Chain)**

Python supports the following five types:

| Type | Structure Diagram | Description |
| :--- | :--- | :--- |
| **Single** | `A` $\rightarrow$ `B` | One Child inherits from one Parent (1:1). |
| **Multiple** | `A, B` $\rightarrow$ `C` | One Child inherits from **Multiple** Parents. |
| **Multilevel** | `A` $\rightarrow$ `B` $\rightarrow$ `C` | A **chain** of inheritance where `C` inherits from `B`, and `B` inherits from `A`. |
| **Hierarchical** | `A` $\rightarrow$ `B`, `A` $\rightarrow$ `C` | **Multiple** Child classes inherit from a **Single** Parent. |
| **Hybrid** | Combination of two or more types (e.g., Multilevel + Multiple). |

-----

### 3\. **Accessing Parent Class Methods**

#### 3.1. **`super()` Function (Crucial for Interviews)**

  * **What it is:** Returns a **temporary object** of the parent class.
  * **Purpose:** Allows the child class to call or access parent class methods (especially `__init__`) **without** explicitly naming the parent class.
  * **Benefits:**
      * Promotes **code reusability**.
      * Works correctly in both **Single** and **Multiple** inheritance.

<!-- end list -->

```python
class Parent:
    def method(self):
        return 'Parent Data'

class Child(Parent):
    def method(self):
        # Calls Parent's method()
        parent_data = super().method() 
        print(f"Child uses {parent_data}")
```

-----

### 4\. **Utility Functions**

#### 4.1. **`issubclass(class, classinfo)`**

  * **Role:** Checks if the first argument (`class`) is a subclass of the second argument (`classinfo`).
  * **Return Value:** Returns `True` or `False`.
  * **`classinfo`:** Can be a single class, type, or a **tuple** of classes/types.

<!-- end list -->

```python
# Example:
issubclass(Employee, Company)     # True, if Employee inherits Company
issubclass(Employee, (list, Company)) # True, if Employee inherits list OR Company
```

-----

