Here are your detailed revision notes on Python basics and environment setup, formatted for quick and effective review:

-----

## 🐍 Python Basics: The Core Superpowers

### 1\. Data Types

These define the kind of data you're working with.

| Type | Description | Key Feature | Example |
| :--- | :--- | :--- | :--- |
| **`int`** | Whole numbers. | Used for counting and basic math. | `x = 42` |
| **`float`** | Numbers with a decimal point. | Used for precise calculations (e.g., currency, measurements). | `price = 19.99` |
| **`str`** | Textual data (sequence of characters). | **Immutable** (cannot be changed after creation). | `status = "Connected"` |
| **`bool`** | Logical value: **`True`** or **`False`**. | The result of all comparisons and conditionals. | `is_valid = (age > 18)` |
| **`list`** | Ordered, changeable collection. | **Mutable** (items can be added, removed, or changed). | `users = ["Tom", "Sue"]` |
| **`dict`** | Unordered collection of **`key: value`** pairs. | Optimized for quick lookup by key. | `config = {"port": 8000, "debug": True}` |

-----

### 2\. Conditionals (`if`, `elif`, `else`)

This is how your code makes **decisions**.

  * **Logic Flow:** Python checks conditions in order. It executes the block corresponding to the **first `True`** condition and then skips the rest.
  * **Keywords:**
      * **`if`**: The initial test.
      * **`elif`** (Else If): An intermediate test if the preceding `if`/`elif` conditions were `False`. You can have many `elif` blocks.
      * **`else`**: The final fallback; executes only if *all* preceding conditions were `False`.
  * **Indentation is Key:** Code blocks under `if`, `elif`, and `else` must be **indented** (usually 4 spaces).

<!-- end list -->

```python
score = 85
if score >= 90:
    print("Grade A")
elif score >= 80: # This runs only if score < 90
    print("Grade B")
else:
    print("Needs work")
```

-----

### 3\. Loops (`for`, `while`)

This is your **automation** tool for repetitive tasks.

#### **`for` Loop (Iteration)**

  * **Use Case:** Ideal when you know the number of iterations or you need to process every item in a collection (list, dictionary, string).
  * **The `range()` Function:** Often used to loop a specific number of times. `range(5)` generates numbers 0, 1, 2, 3, 4.

<!-- end list -->

```python
# Iterating over a list
for item in ["API", "DB", "Server"]:
    print(f"Working on: {item}")
```

#### **`while` Loop (Condition-based)**

  * **Use Case:** Ideal when you need to repeat an action *as long as* a certain condition remains true (e.g., waiting for user input, retrying a connection).
  * **Safety Tip:** You **must** include code inside the loop that eventually makes the condition `False` to avoid an **infinite loop**.

<!-- end list -->

```python
retry_count = 0
while retry_count < 3:
    print("Attempting to connect...")
    retry_count += 1 # Increments the count to ensure the loop stops
```

-----

### 4\. Functions (`def`)

The power of **reusability** and code **organization**.

  * **Definition:** Use the **`def`** keyword to define a function.
  * **Arguments:** Values passed into the function (the inputs).
  * **`return` Statement:** Sends a value *back* from the function to the calling code. If `return` is omitted, the function implicitly returns `None`.

<!-- end list -->

```python
def calculate_tax(base_price, tax_rate=0.05):
    """Calculates the total price including tax.""" # Docstring for documentation
    total = base_price * (1 + tax_rate)
    return total

# Calling the function
final_price = calculate_tax(100) # Returns 105.0
```

-----

## 💻 Development Environment Setup

A professional setup is half the battle won.

### 1\. Python Installation & PATH

  * **Install:** Get the official installer from [python.org](https://www.python.org/).
  * **The "PATH":** **Critical step** on Windows. Check the box **"Add Python to PATH"** during installation. This allows you to run `python` (or `python3`) commands directly from any command line folder.

### 2\. Code Editor

  * **VS Code (Visual Studio Code):** The most popular choice. It's lightweight, fast, and has excellent Python extensions for debugging and code hints.

### 3\. Virtual Environment (`venv`)

This is non-negotiable for backend development.

  * **Analogy:** A `venv` is a self-contained backpack for a specific project. It holds all the necessary Python libraries (`packages`) and versions for *that one project* without interfering with any other project on your machine.
  * **Isolation:** Prevents **dependency conflicts** (e.g., Project A needs an old version of Flask; Project B needs the newest).
  * **Process:**
    1.  **Create:** In your project folder, run: `python -m venv venv`
    2.  **Activate:** This command depends on your operating system and shell:
          * **Linux/macOS:** `source venv/bin/activate`
          * **Windows (PowerShell):** `.\venv\Scripts\activate`
    3.  **Check:** When active, your command line prompt will show `(venv)` at the beginning.
