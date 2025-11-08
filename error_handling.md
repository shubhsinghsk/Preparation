

````markdown
# 3. Error and Exception Handling: Your Robustness Toolkit 🛡️

You're moving back to a foundational, yet critical, skill: **Error and Exception Handling**. This is essential for building **resilient backend applications** that can gracefully recover from unexpected issues instead of crashing.

Think of this as your code's **Resilience Strategy** 🛡️.

In Python, an **Exception** occurs when a valid piece of code encounters an unexpected situation during execution (e.g., trying to access a file that doesn't exist). This is what you must anticipate and handle.

---

## A. The Core Structure: `try` and `except`

The primary mechanism is the `try...except...else...finally` block, which allows you to separate monitored code from error recovery code.

| Block | Purpose | Action Flow |
| :--- | :--- | :--- |
| `try` | Contains code that is monitored for exceptions (the potential danger zone). | Executed first. If an exception occurs, execution immediately jumps to the matching `except` block. |
| `except ExceptionType as e:` | Contains code that handles a specific exception. | Executed only if a matching exception is raised in the `try` block. The exception instance is often assigned to a variable (`e`) for logging or printing. |

### Example: Handling Specific Errors

```python
try:
    # 1. Monitored Code
    num1 = int(input("Enter numerator: "))
    num2 = int(input("Enter denominator: "))
    result = num1 / num2
except ValueError:
    # 2. Handles non-integer input
    print("Error: Input must be a valid integer.")
except ZeroDivisionError:
    # 3. Handles division by zero
    print("Error: Cannot divide by zero.")
except Exception as e:
    # 4. General catch-all (discouraged, but useful for unknown errors)
    print(f"An unexpected error occurred: {e}")
````

-----

## B. Extending the Structure: `else` and `finally`

These optional blocks provide a clean way to handle code that should run only on success or code that must always run.

| Block | Purpose | Execution Timing |
| :--- | :--- | :--- |
| `else` | Contains code that **ONLY** runs if the `try` block completes successfully (no exceptions were raised). | Executed after `try` is complete, but before `finally`. |
| `finally` | Contains code that is **ALWAYS** executed, regardless of whether an exception occurred, was handled, or if the block finished normally. | Executed last. Crucial for **resource cleanup**. |

### Example: Guaranteed Cleanup (`finally`)

This is the preferred structure for File I/O or Database Connections when a Context Manager (`with`) is not used.

```python
f = None # Initialize file handle outside try
try:
    f = open("data.txt", "r")
    content = f.read()
    # If the file read succeeds...
except FileNotFoundError:
    print("Data file not found.")
else:
    # Only runs if 'try' succeeded
    print(f"Successfully read file with length: {len(content)}")
finally:
    # GUARANTEES execution, ensuring the file is closed
    if f:
        f.close()
        print("Resource cleanup complete: File closed.")
```

-----

## C. Creating Custom Exceptions

Custom exceptions allow you to define and handle specific errors related to your application's **business logic** (e.g., "User is not authorized").

  * **Define:** Create a new class that inherits from the built-in `Exception` class.
  * **Raise:** Use the `raise` keyword to signal the custom error when your business rules are violated.

<!-- end list -->

```python
# 1. Custom Exception Definition
class InsufficientPermissionsError(Exception):
    """Raised when a user attempts an action without the necessary role."""
    def __init__(self, user, required_role):
        # Pass a custom message to the base Exception class
        super().__init__(f"User {user} requires role '{required_role}' for this action.")

# 2. Using the Custom Exception
def check_admin_action(user_role):
    if user_role != 'admin':
        # 3. Raise the exception
        raise InsufficientPermissionsError("guest_user", 'admin')
    return True

# 4. Handling the Custom Exception
try:
    check_admin_action('user')
except InsufficientPermissionsError as e:
    # Targeted handling for YOUR specific error
    print(f"SECURITY ALERT: {e}")
```

### Why Custom Exceptions are Powerful:

They increase **readability** and enable **targeted handling**. Code catching `InsufficientPermissionsError` is much clearer than code catching a generic `Exception`.

```

Would you like to explore a specific example of error handling in a common backend task, like handling an API request failure?
```
