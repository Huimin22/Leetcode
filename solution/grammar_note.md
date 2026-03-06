#### About zip(s, t)
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

#### About call self.function

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


#### About defaultdict

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
