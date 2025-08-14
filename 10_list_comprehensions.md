# List/Dict/Set Comprehensions — Expressive and Fast

## 1) Syntax & Semantics
```python
[x*x for x in xs if x%2==0]
{x: len(x) for x in words if x.isalpha()}
{f(x) for x in xs}
```

- Executed in their own scope (no leaked loop variable in Py3).

## 2) Nested Comprehensions
```python
pairs = [(x,y) for x in xs for y in ys if x<y]
```

## 3) Performance Notes
- Faster than equivalent `for` loops building lists in Python.
- Prefer generator expressions when you only iterate once:
```python
sum(x*x for x in data)
```

## 4) Readability
- Break into loops if comprehension gets too complex (rule of thumb: <= 2 clauses).

## 5) Exercises
1. Build a frequency dict with a dict-comprehension (without Counter).
2. Create a comprehension that normalizes emails (lowercase, strip, dedupe).