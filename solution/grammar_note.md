## About Division

* `a / b` → floating-point division
* `a // b` → floor division (toward -∞)
* `int(a / b)` → truncate toward zero (closer to 0)

## About Dictionary Traversal Default

### 1️⃣ Dictionary / Set Comprehension

**Syntax template**:

```python
# dict
{key_expr: value_expr for var in iterable if condition}

# set
{expr for var in iterable if condition}

# list
[expr for var in iterable if condition]
```

**Principles**:

* Left side → what you generate (key/value or element)
* Right side → iteration path
* Multi-level for → outer loop first, inner loop after
* Optional `if` for filtering

**Example**:

```python
# Initialize graph with each letter as key and empty set as value
graph = {c: set() for word in words for c in word}

# Equivalent expanded version:
graph = {}
for word in words:
    for c in word:
        if c not in graph:
            graph[c] = set()
```

---

### 2️⃣ Dictionary Traversal Default Behavior

```python
d = {'a': 1, 'b': 2, 'c': 3}

for x in d:           # iterates over keys
for x in d.keys():    # also keys
for v in d.values():  # iterates over values
for k, v in d.items():# iterates over key/value pairs
```

* Example in context:

```python
indegree = {c: 0 for c in graph}  # traverses keys by default, value = 0
```


### 3️⃣ `set()` vs `list()`

* `set()` → automatic deduplication, O(1) membership check
* `list()` → allows duplicates, preserves order
* Use `set()` in `graph[c]` to avoid duplicate edges:

```python
graph[c] = set()
if neighbor not in graph[c]:
    graph[c].add(neighbor)
```


## Container vs Syntax 

1. **Container vs Syntax**

   | Symbol | Container Type | Example / Purpose                                      |
   | ------ | -------------- | ------------------------------------------------------ |
   | `[]`   | list           | `[key, value]` stores elements                         |
   | `()`   | tuple          | `(a, b)` is a tuple, also used in **unpacking syntax** |

2. **Unpacking Rules**

   * **Single-layer unpacking** (element itself is list/tuple):

     ```python
     bucket = [[1,10],[2,20]]
     for k, v in bucket:  # No parentheses needed
         print(k, v)
     ```

     ✅ Output:

     ```
     1 10
     2 20
     ```

   * **Multi-layer unpacking** (enumerate + inner list/tuple):

     ```python
     bucket = [[1,10],[2,20]]
     for i, (k, v) in enumerate(bucket):  # Parentheses match inner structure
         print(i, k, v)
     ```

     ✅ Output:

     ```
     0 1 10
     1 2 20
     ```

     * Parentheses tell Python: “element is a container, unpack its two elements”
     * Writing `for i, k, v in enumerate(bucket)` would raise an error, because `enumerate` returns `(i, element)` → two items only.

---

#### Quick Memory Trick

* **Phrase**:

> **“Outer enumerate uses parentheses, inner container can be any type; parentheses for unpacking, square brackets for storing.”**

* **Visualized**:

```python
enumerate(bucket) → (index, element)
element = [key, value]  # list or tuple
for i, (k, v) in enumerate(bucket)
```

* **Simplified cheat**:

  * `[ ]` → storage
  * `( )` → unpacking


## About zip(s, t)

### 1️⃣ `zip(s, t)`

* `zip()` is a Python built-in function.
* It pairs elements from two iterables (`s` and `t`) by position.
* Returns an iterator of tuples `(s_char, t_char)`.

**Example:**

```python
s = "egg"
t = "add"
list(zip(s, t))  # [('e', 'a'), ('g', 'd'), ('g', 'd')]
```

* First iteration: `('e', 'a')`
* Second iteration: `('g', 'd')`
* Third iteration: `('g', 'd')`

---

### 2️⃣ `for cs, ct in zip(s, t)`

* Uses **tuple unpacking**.
* Each iteration assigns:

  * `cs` = current character from `s`
  * `ct` = current character from `t`

```python
for cs, ct in zip(s, t):
    print(cs, ct)
```

**Output:**

```
e a
g d
g d
```

---

### 3️⃣ Why use `zip`

* Traverse two strings simultaneously.
* Guarantees `cs` and `ct` are at the same position.
* Suitable for checking one-to-one mapping.

---

## About call self.function

`self` is **required when a class method needs to access its own attributes or call other methods**.

Specific cases:

1. **Access instance attributes**

```python
class A:
    def __init__(self, x):
        self.x = x
    def print_x(self):
        print(self.x)   # must use self to access instance variable
```

2. **Call another method in the class**

```python
class A:
    def f1(self):
        return 1
    def f2(self):
        return self.f1()  # must use self to call another class method
```

3. **Cases where self is not needed**

* Local or nested functions
* Local variables
* Directly passed parameters

In your example, `helper` is a function defined inside the method, not a class method, so no `self` is needed.

If `helper` were defined as a **class method**, you would need `self.helper(...)`.



## Understanding

* **Class methods are not global functions**
  If you write `invertTree(...)` inside a method, Python will first look in the **local scope**, then in the **global scope**.
  Since the function is defined inside the class, it is **not in local or global scope** → results in an error.

* **Adding `self.` changes this**

  * `self` points to the **current object**
  * The current object is an instance of `Solution()`, which **owns the `invertTree` method**
  * Writing `self.invertTree(...)` tells Python to look in the **object’s method list** → found → call succeeds

* **Purpose of recursive call**

  * Each recursion uses the **same object `self`** to call its method
  * Since the object already has the `invertTree` method, recursion works correctly

💡 **Analogy**:

* **Without `self`** → “I want a function called `invertTree`” → not found in local/global → error
* **With `self`** → “I want to use the `invertTree` method the object already has” → found → call succeeds

## About defaultdict

```python
from collections import defaultdict

# defaultdict auto-creates empty list for new keys
graph = defaultdict(list)

graph[0].append(1)  # 0 → 1
graph[0].append(2)  # 0 → 2

print(graph)         # {0: [1, 2]}
print(graph[1])      # []  auto-created empty list
```

* Key = node
* Value = list of neighbors
* Missing key → auto empty list

defaultdict(int)  → missing key becomes 0
defaultdict(list) → missing key becomes []
defaultdict(set)  → missing key becomes set()



**Summary:**

* `zip(s, t)` → pairs elements by position.
* `for cs, ct in zip(s, t)` → get corresponding characters each iteration for mapping checks.
