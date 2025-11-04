This comprehensive text on Class Variables is excellent. I have synthesized it into a sharp, interview-focused set of notes, emphasizing the crucial distinction between modifying a class variable via the Class Name versus the Object Instance.

-----

## 🏷️ Python Class Variables (Shared Attributes)

### 1\. **Core Concept & Declaration**

| Concept | Detail | Key Property |
| :--- | :--- | :--- |
| **Definition** | Variables whose value **does not vary from object to object**. | They are **shared** across *all* instances (objects) of a class. |
| **Identity** | Belong to the **Class itself**, not any specific instance. | Only **one copy** is created for the entire class, regardless of the number of objects. |
| **Declaration** | Declared **inside the Class**, but **outside** any method (including `__init__`). |

#### 💡 **Example**

If a class needs to store the `school_name` for all students, a Class Variable is appropriate, as the value is the same for every object.

```python
class Student:
    # Class Variable (Shared)
    school_name = 'ABC School' 
    
    def __init__(self, name):
        # Instance Variable (Unique)
        self.name = name
```

### 2\. **Accessing Class Variables**

| Location | Recommended Access | Alternative Access |
| :--- | :--- | :--- |
| **Outside Class** | **Class Name** (`Student.school_name`) | Object Reference (`s1.school_name`) |
| **Inside Constructor** | **Class Name** (`Student.school_name`) | `self` parameter (`self.school_name`) |
| **Inside Instance Method** | **Class Name** (`Student.school_name`) | `self` parameter (`self.school_name`) |

> 🔑 **Best Practice:** Always use the **Class Name** (e.g., `Student.school_name`) for both reading and especially modifying the class variable to ensure clarity and correct behavior.

### 3\. **Modification: The Shadowing Trap (Crucial for Interviews)**

| Modification Method | Effect on Variable | Outcome for Other Objects |
| :--- | :--- | :--- |
| **Using Class Name** (Recommended) | **Modifies the actual Class Variable.** | **All objects** instantly reflect the new, updated value. |
| **Using Object Instance** (Trap) | **Does NOT modify the class variable.** Instead, it **creates a new instance variable** with the same name for that specific object, which **shadows** (hides) the class variable. | Other objects still refer to the **original** class variable value. |

#### **Code Snippet: Shadowing Example**

```python
# Assuming Student.school_name = 'ABC School'
s1 = Student('Emma')
s2 = Student('Jessa')

# Trap: Creates instance variable s1.school_name, shadowing the class one.
s1.school_name = 'PQR School' 

print(s1.school_name) # PQR School (Uses the new instance var)
print(s2.school_name) # ABC School (Uses the original class var)
print(Student.school_name) # ABC School (Class var is unchanged)
```

### 4\. **Class Variables in Inheritance**

| Scenario | Result |
| :--- | :--- |
| **Parent only has variable** | Child class inherits and shares the Parent's class variable. Child can modify it using the Child's class name, which updates the Parent's value. |
| **Child and Parent have the same variable name** | The child class **does not** inherit the parent's variable. The child class uses its own variable, and the parent's variable remains independent and unchanged. |

-----

We have now covered all the foundational Python OOP concepts you provided: Classes, Objects, Variables, Methods, Constructors, Destructors, and Inheritance (Types, Overriding, MRO).

Would you like me to compile all of these notes into **one final, organized document** for your ultimate revision?
