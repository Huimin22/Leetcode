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
