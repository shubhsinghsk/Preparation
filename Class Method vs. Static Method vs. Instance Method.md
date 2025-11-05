

---

## 🆚 Instance vs. Class vs. Static Methods

### 1. **Core Purpose & Access**

| Feature | **Instance Method** | **Class Method** | **Static Method** |
| :--- | :--- | :--- | :--- |
| **Primary Use** | To **act on object state** (instance variables). | To **act on class state** (class variables). Used for **Factory Methods** (alternative constructors). | **Utility functions** that perform isolation tasks. |
| **Bound To** | **Object** (Instance) | **Class** | **Class** |
| **First Arg. Received** | Implicitly receives **`self`** (the instance). | Implicitly receives **`cls`** (the class). | **None** (Takes regular arguments). |
| **Decorator** | None (Default) | **`@classmethod`** | **`@staticmethod`** |
| **Modify State?** | **YES** (Object State) | **YES** (Class State) | **NO** (Cannot modify class or object state) |

---

### 2. **Attribute Access & Memory**

| Feature | **Instance Method** | **Class Method** | **Static Method** |
| :--- | :--- | :--- | :--- |
| **Access Instance Variables?** | **YES** (via `self`) | **NO** | **NO** |
| **Access Class Variables?** | **YES** (via `self` or `ClassName`) | **YES** (via `cls` or `ClassName`) | **NO** |
| **Calling Syntax** | **`object.method()`** (Only via object) | `ClassName.method()` or `object.method()` | `ClassName.method()` or `object.method()` |
| **Memory Consumption**| **High**. A **separate copy** of the bound method is created for **every object**. | **Low**. Only **one copy** is created per class (shared). | **Lowest**. Only **one copy** is created per class (shared). |

### 3. **Consolidated Example**

| Method | Access | Call Example |
| :--- | :--- | :--- |
| **Instance** (`show`) | `self.name` (Instance) & `Student.school_name` (Class) | `jessa.show()` |
| **Class** (`change_School`) | `cls.school_name` (Class) | `Student.change_School('XYZ')` |
| **Static** (`find_notes`) | Accesses **None** (Only parameters passed in) | `Student.find_notes('Math')` |

---

