# Python Variables, Numbers, and Strings — From Fundamentals to Mastery

> Goal: Build a precise mental model of Python’s object model, numeric tower, and Unicode-aware strings. Learn idioms, pitfalls, performance considerations, and testing exercises.

---

## 1) Names, Objects, and Bindings (Not “variables” like C/Java)
In Python, **names** bind to **objects** stored on the heap. Assignment **does not copy** the object—it binds the name to an existing object reference.

```python
a = [1, 2, 3]
b = a           # b points to the same list object
b.append(4)
assert a is b   # True
assert a == [1, 2, 3, 4]
```

### Mutability vs. Immutability
- **Immutable**: `int`, `float`, `bool`, `str`, `tuple`, `frozenset`, `bytes` (contents cannot change, new object created on “modification”).
- **Mutable**: `list`, `dict`, `set`, `bytearray`, custom class instances.

### Identity, Equality, and Aliasing
- `is` → identity (same object).
- `==` → value equality (delegates to `__eq__`).
- Aliasing bugs arise when reusing mutable defaults or sharing references.

**Pitfall**: Mutable default arguments
```python
def append_user(user, bucket=[]):  # BAD
    bucket.append(user)
    return bucket

# The same list is reused across calls!
```

**Fix**:
```python
def append_user(user, bucket=None):
    if bucket is None:
        bucket = []
    bucket.append(user)
    return bucket
```

### Copying
- Shallow: `list(x)`, `x.copy()`, `copy.copy(x)` → top-level container copied; nested objects shared.
- Deep: `copy.deepcopy(x)` → recursive copy.

---

## 2) Numbers — The Python Numeric Tower

### Built-ins
- `int`: arbitrary precision; no overflow.
- `float`: IEEE 754 double precision.
- `complex`: `a + bj` with `.real`, `.imag`.
- `bool`: subclass of `int` (`True == 1`).

### Operators & Precedence
```
** > unary + - > * / // % > + - > comparisons > not > and > or
```

```python
7 / 2   # 3.5
7 // 2  # 3 (floor division)
-7 // 2 # -4 (floors toward -inf)
7 % 2   # 1
2 ** 10 # 1024
```

### Decimal and Fraction (for accuracy)
```python
from decimal import Decimal, getcontext
getcontext().prec = 50
Decimal("0.1") + Decimal("0.2") == Decimal("0.3")  # True

from fractions import Fraction
Fraction(1, 3) + Fraction(1, 6)  # Fraction(1,2)
```

### Floating-Point Gotchas
```python
0.1 + 0.2 == 0.3  # False (binary rounding)
abs((0.1 + 0.2) - 0.3) < 1e-9  # tolerant compare
```

### Performance Tips
- Prefer integers where exactness matters.
- Use `math.isclose` for floats.
- Vectorize numeric code with `numpy` for large arrays.

---

## 3) Strings — Unicode First-Class

### Creating & Escapes
```python
s = "Hello\n\tWorld"
raw = r"C:\\path\\file.txt"  # raw string keeps backslashes
multi = """Triple-quoted
multiline string"""
```

### Indexing & Slicing
```python
s = "Python"
s[0], s[-1]        # 'P', 'n'
s[1:4]             # 'yth'
s[::-1]            # 'nohtyP' (reversed copy)
```

### Immutability
Concats create new objects. Prefer `''.join(iterable)` for repeated concatenation.

```python
parts = ["a", "b", "c"]
"".join(parts)  # 'abc'
```

### Common Methods
```python
"  hi  ".strip()
"Spam".lower().startswith("s")
"eggs,ham".split(",")
",".join(["x","y"])
"Pi={:.3f}".format(3.14159)
f"Pi={3.14159:.3f}"
```

### Encoding/Decoding
```python
data = "नमस्ते".encode("utf-8")
text = data.decode("utf-8")
```

**Pitfall**: Decode bytes to text at boundaries; keep internal processing as `str`.

---

## 4) Interlude: Truthiness & Ternaries
```python
# Falsy: 0, 0.0, 0j, "", [], {}, set(), range(0), None, False
msg = "OK" if result else "FAIL"
```

---

## 5) Exercises
1. Write `safe_div(a, b, *, eps=1e-12)` that returns `a/b` but uses `math.isclose(b, 0, abs_tol=eps)` to avoid division by near-zero.
2. Implement `normalize_spaces(s)` that collapses all whitespace runs to a single space without regex.
3. Create `precise_sum(iterable)` using `math.fsum` and compare with built-in `sum` on floats.
4. Implement `slugify(text)` handling Unicode normalization (hint: `unicodedata.normalize`).

---

## 6) Review Checklist
- [ ] Can explain names vs. objects vs. bindings.
- [ ] Can distinguish identity vs. equality.
- [ ] Understand float rounding issues and when to use Decimal/Fraction.
- [ ] Know efficient string building patterns and Unicode rules.