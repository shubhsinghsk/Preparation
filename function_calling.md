These detailed notes are structured for deep revision, focusing on the core principles and technical implementation of OpenAI Function Calling in Python.

## 1\. Core Concept: The LLM as an Intent Parser 🧠

Function calling is fundamentally about treating the Large Language Model (LLM) not just as a text generator, but as a **highly sophisticated intent parser and argument extractor**.

  * **Goal:** To reliably connect the LLM to external, deterministic tools (APIs, databases, custom Python functions) to perform actions outside its knowledge scope.
  * **The Bridge:** The LLM does **not** execute the code. It analyzes the user's prompt and the function's description, then outputs a **standardized JSON object** detailing the function name and the arguments to use.
  * **The Loop:** Your application code is responsible for receiving this JSON, executing the actual Python function, and sending the result back to the LLM for final response generation. This process requires a minimum of **two API calls**.

-----

## 2\. Defining the Tool: The Schema is King 👑

The model learns how and when to use a function entirely from the **JSON Schema** you provide. The quality and clarity of this schema are paramount.

### A. Python Function Definition

This is your actual, executable code. It must have clear type hints and docstrings for your own maintainability, though the LLM ignores these directly.

```python
def get_stock_price(ticker: str, date: str = "today") -> float:
    # This is the executable logic
    ...
```

### B. The OpenAI JSON Schema Contract

This is the structured description passed to the `tools` parameter in the API call.

| JSON Field | Purpose | Key Revision Points |
| :--- | :--- | :--- |
| **`type`** | Must be `"function"`. | Specifies the type of tool being defined. |
| **`function`** | Contains the detailed definition of the function. | |
| **`name`** | The exact name of the Python function (`get_stock_price`). | Must match the executable Python function name exactly. |
| **`description`** | **Crucial Context** for the LLM. | Must clearly state the function's *purpose* and *when* it should be used (e.g., "Use this tool to find the current stock price given a ticker symbol."). |
| **`parameters`** | An object describing all inputs. Must have `type: "object"`. | |
| **`properties`** | Defines each argument the function accepts. | Each property must include a `type` (`string`, `number`, `boolean`) and a clear `description` for argument extraction. |
| **`enum` (Optional)** | A list of acceptable string values for a parameter. | Helps constrain the model's output (e.g., `["celsius", "fahrenheit"]`). |
| **`required`** | A list of parameter names that **must** be extracted from the user's prompt. | If the model cannot extract a required parameter, it will either try to ask the user for it or attempt to proceed without calling the function, depending on the model/prompt context. |

-----

## 3\. The Execution Flow: The Two-Call Dance 💃

Function calling is a conversation flow where the tool results are fed back into the chat history.

### Step 1: Initial Request (The Intent)

1.  **You:** Send the user's prompt and the `tools` schema list to the LLM.
2.  **LLM's Response:**
      * **If LLM doesn't need a tool:** It returns a final text message.
      * **If LLM needs a tool:** The response contains a `message` object with a **`tool_calls`** array (a list of one or more function call objects). The main `content` field is often empty.
3.  **Key Check:** Your code must check `if response.choices[0].message.tool_calls:`

### Step 2: Tool Execution (Your Code)

1.  **Parse:** Iterate through the `tool_calls` array. For each call:
      * Retrieve the `function.name`.
      * Parse the `function.arguments` (a JSON string) into a Python dictionary.
2.  **Execute:** Call your actual Python function using the extracted arguments.
3.  **Capture:** Store the function's return value (e.g., `"The stock price is $150.00"`).

### Step 3: Second Request (The Result)

1.  **Append:** Add two new types of messages to the conversation history:
      * The model's original function request message (the one with `tool_calls`).
      * A new message with `role: "tool"`. The `content` is the output from your executed Python function. **Crucially, this message must also include the corresponding `tool_call_id` and `name` of the function.**
2.  **Final Call:** Send the updated message history back to the LLM (without the `tools` schema, as the LLM now has all the info it needs).
3.  **LLM's Final Response:** The LLM uses the tool output (`role: "tool"`) to generate a natural, user-friendly final text response.

-----

## 4\. Advanced Control and Debugging 🛠️

### A. The `tool_choice` Parameter

This allows you to override the model's default behavior:

| `tool_choice` Value | Effect | When to Use |
| :--- | :--- | :--- |
| `"auto"` (Default) | The model decides whether to call a tool or reply with text. | Conversational agents, general use. |
| `"none"` | Forces the model **not** to call any function; it must reply with text. | When you know the user's question is purely informational/creative. |
| `{"type": "function", "function": {"name": "func_name"}}` | **Forces** the model to call a specific function. | When you are certain a tool must be used (e.g., a "Convert Units" button). |

### B. Handling Failure

  * **Execution Failure (Code Error):** If your Python function throws an error, you must send an error message (as a string) back to the LLM in the `role: "tool"` message. The LLM can then inform the user that the action failed.
    ```json
    {
      "role": "tool", 
      "content": "Error: Could not connect to API to get data.", 
      ...
    }
    ```
  * **Argument Extraction Failure:** If the model struggles to extract an argument, it will often try to ask the user a follow-up question (e.g., "Which city did you mean?"). Your code must be ready to handle a text-only response from the LLM in the first step.

### C. Multiple/Parallel Calls

The `tool_calls` object is a list, meaning a modern model (like `gpt-4o`) can request **multiple, simultaneous function calls** in a single turn. Your execution logic should always loop through the `tool_calls` array, execute all functions, and return all results in the second API call.
