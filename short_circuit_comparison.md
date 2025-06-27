# Short-Circuit Evaluation: C++ vs Python

## Python DOES Have Short-Circuit Evaluation!

### Python's `and` Operator - Short-Circuit Behavior:
```python
# Python's 'and' works EXACTLY like C++'s '&&'

# Example 1: Basic short-circuit with 'and'
def expensive_function():
    print("This won't be called!")
    return True

result = False and expensive_function()  # expensive_function() never called
print(result)  # False

# Example 2: Demonstrating step-by-step evaluation
def first_check():
    print("First condition checked")
    return False

def second_check():
    print("Second condition checked")  # This will NEVER print
    return True

result = first_check() and second_check()
# Output: "First condition checked"
# second_check() is never called because first_check() returned False

# Example 3: When both get evaluated
def first_true():
    print("First condition: True")
    return True

def second_false():
    print("Second condition: False")
    return False

result = first_true() and second_false()
# Output: 
# "First condition: True"
# "Second condition: False"
# Both get evaluated because first was True

# Example 2: Safe attribute access
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None

head = None  # or head = Node(1)

# This works safely in Python too!
if head is not None and head.next is not None:
    print(head.next.val)  # Only runs if both conditions are true

# Example 3: Linked list traversal (Python equivalent)
def find_cycle(head):
    fast = head
    slow = head
    
    # Short-circuit prevents AttributeError
    while fast is not None and fast.next is not None:
        fast = fast.next.next
        slow = slow.next
        if fast == slow:
            return True
    return False
```

### C++ `&&` vs Python `and` - IDENTICAL Behavior:

| **Aspect** | **C++** | **Python** |
|------------|---------|-------------|
| **Operator** | `&&` | `and` |
| **Short-circuit?** | ✅ Yes | ✅ Yes |
| **Logic** | If first is false, stop | If first is falsy, stop |
| **Example** | `false && anything` | `False and anything` |
| **Result** | `anything` never evaluated | `anything` never evaluated |

**Practical Examples:**

| **C++** | **Python** |
|---------|-------------|
| `ptr != nullptr && ptr->next != nullptr` | `node is not None and node.next is not None` |
| `false && expensive_call()` | `False and expensive_call()` |
| `true && check_second()` | `True and check_second()` |
| Prevents segmentation fault | Prevents AttributeError |

### Python Short-Circuit Examples:
```python
# 1. 'and' stops at first falsy value
result1 = False and print("Won't print")  # False, print never called
result2 = True and "second value"         # "second value"
result3 = 0 and "won't reach"            # 0 (falsy)

# 2. 'or' stops at first truthy value  
result4 = True or print("Won't print")    # True, print never called
result5 = False or "second value"         # "second value" 
result6 = None or 42                      # 42

# 3. Practical use cases
def safe_divide(a, b):
    return b != 0 and a / b  # Returns False if b is 0, otherwise returns division

def get_nested_value(data):
    # Safe nested access
    return data and data.get('user') and data['user'].get('name')

# 4. List/string checking
def process_list(lst):
    if lst and len(lst) > 0 and lst[0] > 10:
        return lst[0] * 2
    return 0

# 5. Function call chaining
def validate_user(user):
    return (user and 
            hasattr(user, 'email') and 
            '@' in user.email and
            len(user.email) > 5)
```

### Key Differences:

**C++:**
- **Pointers can be `nullptr`** → accessing them crashes the program
- **Manual memory management** → need to check before dereferencing
- **Compile-time types** → mistakes caught at compilation

**Python:**
- **Objects can be `None`** → accessing attributes raises `AttributeError`
- **Automatic memory management** → no manual pointer management
- **Runtime types** → mistakes caught at runtime

### Why It Feels Different:

```python
# Python - this feels "safer" because:
if node and node.next:
    print(node.next.value)

# vs C++ - this feels more explicit about danger:
if (node != nullptr && node->next != nullptr) {
    cout << node->next->value;
}
```

**The key insight:** Both languages have short-circuit evaluation, but Python's automatic memory management makes pointer-related crashes less common, so you might not notice the short-circuit behavior as much!

### Python Gotchas with Short-Circuit:
```python
# These all use short-circuit evaluation:

# 1. Default value assignment
name = user.name or "Anonymous"  # If user.name is falsy, use "Anonymous"

# 2. Conditional function calls
debug_mode and print("Debug info")  # Only prints if debug_mode is True

# 3. Multiple conditions
if user and user.is_active and user.has_permission('read'):
    show_content()

# 4. Avoiding exceptions
result = data and data.get('key') and data['key'].upper()
```

**Bottom line:** Python definitely has short-circuit evaluation! It's just that Python's memory safety makes the crashes less dramatic than C++'s segmentation faults. 