# Lists, Dicts, Sets, Tuples — Data Structures Deep Dive

## 1) Lists (Dynamic Arrays)
- Ordered, mutable, heterogeneous.
- Amortized O(1) append; O(n) insert at front/middle.

```python
arr = [3, 1, 4]
arr.append(1)
arr.extend([5, 9])
arr.insert(0, 2)     # O(n)
arr.remove(1)        # first match, ValueError if not found
arr.pop()            # last item
arr.sort()           # in-place
sorted_arr = sorted(arr, reverse=True, key=abs)
```

**Slicing** returns a copy (shallow):
```python
a = [1,2,3,4]
b = a[:]
assert b is not a and b == a
```

### List as Stack / Queue
- Stack: `append`, `pop`
- Queue: prefer `collections.deque` for O(1) popleft

### Comprehensions (preview)
```python
squares = [x*x for x in range(10) if x%2==0]
```

---

## 2) Tuples (Immutable Records)
- Immutable, hashable if items are hashable.
- Ideal for fixed-length records, dictionary keys.
```python
pt = (10, 20)
x, y = pt           # unpacking
a, *rest = (1,2,3,4)
```

**Named tuples / dataclasses** for semantics:
```python
from collections import namedtuple
Point = namedtuple("Point", "x y")
p = Point(3,4); p.x
```

---

## 3) Dictionaries (Hash Maps)
- Average O(1) lookup/insert; preserve **insertion order** (CPython 3.7+).
```python
user = {"id": 1, "name": "Ada"}
user["email"] = "ada@x.com"
user.get("age", 0)
user.setdefault("roles", []).append("admin")
for k, v in user.items(): ...
```

### Merging & Views
```python
a = {"x":1, "y":2}
b = {"y":20, "z":3}
c = a | b                # 3.9+
a |= b                   # in-place
keys = a.keys()          # dynamic view
```

### Hashability & Custom Keys
Keys must be hashable (have stable `__hash__` and `__eq__`).

**Pitfall**: lists are unhashable; use tuples/frozensets.

---

## 4) Sets (Unordered Unique Collections)
- Membership tests are O(1).
```python
s = {1,2,3}
s.add(2)
s.update([3,4])
s.discard(99)   # no error
s.remove(1)     # KeyError if absent
```
Operations:
```python
a | b, a & b, a - b, a ^ b
```

Use `frozenset` as immutable, hashable set.

---

## 5) Memory & Performance
- Prefer tuples over lists for fixed data (smaller footprint).
- Pre-size lists when possible (via comprehensions) to reduce reallocations.
- Dicts: avoid repeatedly growing large dicts in tight loops; consider `defaultdict`/`Counter`.

```python
from collections import defaultdict, Counter
dd = defaultdict(list)
for k, v in rows:
    dd[k].append(v)

counts = Counter(words)
counts.most_common(10)
```

---

## 6) Advanced Patterns
- **Dict comprehension with filtering**:
```python
index = {u["id"]: u for u in users if u.get("active")}
```
- **Set comprehension** to dedupe complex expressions.
- **Unpacking merges**:
```python
merged = {**defaults, **overrides}
```

---

## 7) Exercises
1. Implement `stable_unique(seq)` that preserves order of first occurrence.
2. Build an inverted index: given `[(doc_id, text), ...]`, map each word to the set of doc_ids.
3. Given nested lists, write an iterative `flatten` that avoids recursion limits.