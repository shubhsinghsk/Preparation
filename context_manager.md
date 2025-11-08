

-----

## 🛡️ Context Managers: The Resource Guard

### 1\. **Core Concept**

  * **Role:** A **Resource Guard** that guarantees a resource is **properly set up** and, critically, **properly cleaned up** (released), even if errors occur.
  * **Syntax:** Implemented using the **`with` statement**.
    ```python
    with resource_setup as var:
        # Use resource here
    # Resource is automatically cleaned up here
    ```

### 2\. **The Context Manager Protocol (Class-Based)**

Any object used with a `with` statement must implement two "Magic Methods":

| Method | Purpose (Phase) | When Called | `as` Variable Value |
| :--- | :--- | :--- | :--- |
| **`__enter__(self)`** | **Setup** (Acquires the resource). | When the `with` statement starts. | The value it **returns**. |
| **`__exit__(self, exc_type, exc_val, exc_tb)`** | **Cleanup** (Releases the resource). | When the `with` block finishes (normally or by exception). | N/A |

#### **Handling Exceptions in `__exit__`**

  * The three arguments (`exc_type`, `exc_val`, `exc_tb`) contain details if an exception occurred in the `with` block; otherwise, they are `None`.
  * **Suppressing Errors:** If `__exit__` returns **`True`**, the exception is **suppressed** (the program continues execution after the `with` block).
  * **Re-raising Errors:** If `__exit__` returns **`False`** or **`None`** (the default), the exception is **re-raised** after the cleanup code finishes.

-----

### 3\. **The `@contextmanager` Decorator (Generator-Based)**

The standard library's `contextlib` module provides a cleaner, more concise way to create context managers using a generator function.

| Generator Code Location | Context Manager Phase | Equivalent Method |
| :--- | :--- | :--- |
| **Code before `yield`** | **Setup Phase** (Resource Acquisition) | `__enter__` |
| **Value after `yield`** | **Resource Return** | Return value of `__enter__` (bound to `as` variable). |
| **Code after `yield`** | **Cleanup Phase** (Resource Release) | `__exit__` |

#### **Template Structure**

```python
from contextlib import contextmanager

@contextmanager
def simple_cm(name):
    # 1. SETUP PHASE (equivalent to __enter__)
    print(f"Entering {name}")
    try:
        yield 'resource_value' # Resource returned to 'as' variable
    except Exception as e:
        # Optional: Handle specific exception before cleanup
        print(f"Error caught: {e}")
        raise # Re-raise error after cleanup
    finally:
        # 2. CLEANUP PHASE (equivalent to __exit__)
        print(f"Exiting {name}")
```

-----

### 4\. **Key Use Cases**

Context Managers are essential for ensuring resource integrity:

  * **File Handling:** Automatically closing files (`with open(...)`).
  * **Concurrency:** Acquiring and releasing **locks** or semaphores for thread safety.
  * **State Management:** Guaranteeing a temporary configuration change (e.g., setting a variable, locale, or precision) is **reverted** when the block exits.
  * **Database:** Connecting to and ensuring the **closing** or **committing/rolling back** a transaction.

-----
## 🛡️ Context Managers: Two Methods for Resource Management

Context Managers ensure resources are **properly acquired and released** using the `with` statement. The protocol requires two methods: `__enter__` (setup) and `__exit__` (cleanup).

-----

## 1\. Method 1: Building with a Class

A custom context manager is created by defining a class that explicitly implements the Context Manager Protocol.

### A. The Protocol Methods

| Method | Role | Return Value |
| :--- | :--- | :--- |
| **`__enter__(self)`** | **Setup Phase** (Acquire resource, e.g., open a connection). | The **resource object** (bound to the `as` variable). |
| **`__exit__(self, exc_type, exc_val, exc_tb)`** | **Cleanup Phase** (Release resource, e.g., close connection). | *Controls exception handling:* Return **`True`** to **suppress** an exception; return **`None`** or **`False`** to **re-raise** it after cleanup. |

### B. `__exit__` Exception Breakdown

The three arguments provide details on any exception raised inside the `with` block:

  * **`exc_type`**: The exception class (e.g., `ValueError`, or `None` if successful).
  * **`exc_val`**: The exception instance/value.
  * **`exc_tb`**: The traceback object.

### C. Example: Connection Manager

```python
class ConnectionManager:
    # ... __init__ ...
    
    def __enter__(self):
        # 1. SETUP: Acquire connection
        print(f"Connecting to {self.db_name}...")
        self.conn = f"Connection object for {self.db_name}"
        return self.conn 

    def __exit__(self, exc_type, exc_val, exc_tb):
        # 2. CLEANUP: Close connection
        if exc_type:
            print(f"ERROR: Exception {exc_type.__name__} occurred.")
        
        print(f"Closing connection for {self.db_name}.")
        return None # Ensures the exception is re-raised
```

-----

## 2\. Method 2: Building with `@contextmanager`

The `@contextmanager` decorator (from the `contextlib` module) simplifies creating context managers using a single generator function. The logic is implicitly split by the `yield` statement.

### A. The Generator Pattern

| Code Location | Equivalent Method | Phase |
| :--- | :--- | :--- |
| **Before `yield`** | `__enter__` | **Setup Phase** |
| **Value After `yield`** | Return value of `__enter__` | Resource returned to `as` variable. |
| **Code After `yield`** | `__exit__` | **Cleanup Phase** (Runs after the block finishes or on exception). |

### B. Example: File Locker/Closer

```python
from contextlib import contextmanager
import time

@contextmanager
def file_locker(filename, mode='r'):
    # 1. SETUP PHASE (Acquire resource)
    f = open(filename, mode)
    print(f"Locking and opening file: {filename}")
    
    try:
        # 2. YIELD: Pass resource to 'as' variable and pause execution
        yield f 
        
    except Exception as e:
        # Optional: Handle error before guaranteed cleanup
        print(f"File error detected: {e.__class__.__name__}. Cleaning up...")
        raise # Re-raise error
        
    finally:
        # 3. CLEANUP PHASE (Release resource)
        f.close()
        print(f"File closed and lock released for: {filename}")
```

-----

## 3\. File I/O Fundamentals (Common CM Use)

The `with open(...)` context manager is standard for file handling.

### A. File Modes

| Mode | Purpose | Behavior |
| :--- | :--- | :--- |
| **`'r'`** | Read (Default) | Fails if file doesn't exist. Cursor at start. |
| **`'w'`** | Write | Creates file if needed. **TRUNCATES** existing content. |
| **`'a'`** | Append | Creates file if needed. Cursor placed at the **end** of the file. |
| **`'r+'`** | Read and Write | Opens for both, cursor at start. |

### B. Key File Methods

  * **`f.read()`**: Reads **all** content into a single string.
  * **`f.readline()`**: Reads a file **line by line** (returns one line string).
  * **`f.readlines()`**: Reads **all** lines into a single **list of strings**.
  * **`f.write(string)`**: Writes the string to the file.
  * **Looping (`for line in f:`)**: **Most memory-efficient** way to read large files, yielding one line at a time.
