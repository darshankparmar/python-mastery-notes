# Control Flow: If/Elif/Else, Loops, and Loop Patterns

## 1) Truthiness and Conditional Patterns
```python
if not items:         # empty -> False
    ...
elif len(items) < 10:
    ...
else:
    ...
```

### Ternary and Walrus Operator
```python
status = "OK" if code == 200 else "FAIL"
if (n := len(buf)) > 1024:
    print(f"truncated: {n}")
```

### Structural Pattern Matching (3.10+)
```python
def handler(msg):
    match msg:
        case {"type": "ping"}:
            return "pong"
        case {"type": "data", "payload": [x, y]}:
            return x + y
        case _:
            return None
```

---

## 2) For-Loops, While-Loops, and Iteration Protocols
```python
for i in range(5): ...
for i, x in enumerate(seq, start=1): ...
for a, b in zip(xs, ys): ...

while condition:
    ...
else:
    # executes if loop didn't break
```

### Loop Else Gotcha
`else` on loops runs only if the loop wasn’t terminated by `break`.

---

## 3) Common Loop Idioms
- Accumulation with `sum`, `any`, `all`.
- Use `itertools` for combinatorics, grouping, windows:
```python
import itertools as it
pairs = list(it.pairwise([1,2,3,4]))       # 3.10+
perms = list(it.permutations("abc"))
grouped = {k: list(g) for k, g in it.groupby(sorted(data), key=keyfunc)}
```

### Early Exit
```python
for x in xs:
    if predicate(x):
        found = x; break
else:
    found = None
```

---

## 4) Performance Tips
- Prefer comprehension/generator expressions over building temp lists in loops.
- Avoid repeated attribute lookups in hot loops (bind to local names).

---

## 5) Exercises
1. Write a scanner that finds the first matching element with loop-else.
2. Implement `chunks(iterable, size)` yielding lists of at most `size`.
3. Use pattern matching to decode simple JSON-like messages with variants.