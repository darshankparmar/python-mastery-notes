# Functions & Lambda — Signatures, Closures, and Functional Patterns

## 1) Defining Functions & Annotations
```python
def add(x: int, y: int) -> int:
    return x + y

def greet(name: str, *, excited: bool = False) -> str:
    return f"Hi {name}{'!' if excited else ''}"
```

### Parameter Kinds
- Positional-only: `def f(a, b, /, c)`
- Positional-or-keyword: `def f(a, b)`
- Keyword-only: `def f(*, c)`
- Variadic: `*args`, `**kwargs`

Use `inspect.signature(f)` to introspect.

---

## 2) Default Arguments — The Trap
```python
def push(x, bucket=[]):  # BAD
    bucket.append(x)
    return bucket
```
Use `None` sentinel pattern (see File 1).

---

## 3) Closures & Scoping (LEGB)
```python
def outer():
    x = 0
    def inner():
        nonlocal x
        x += 1
        return x
    return inner
counter = outer()
counter(), counter()  # 1, 2
```

### Global vs Nonlocal
- `global` binds module-level name.
- `nonlocal` binds nearest enclosing scope.

---

## 4) Lambdas
- Single-expression, return value is expression.
- Great with `sorted`, `map`, `filter`, but don’t overuse.

```python
sorted(users, key=lambda u: (u["age"], u["name"]))
```

---

## 5) Higher-Order Functions
```python
def compose(f, g):
    return lambda x: f(g(x))
```

`functools` gems:
```python
from functools import partial, lru_cache, reduce, singledispatch

inc = partial(int, base=16)
@lru_cache(maxsize=1024)
def fib(n): ...

@singledispatch
def dump(x): print("obj", x)

@dump.register
def _(x: list): print("list", x)
```

---

## 6) Type Hints for Better APIs
Use `typing.Callable`, `Protocol`, `TypeVar`, `ParamSpec`, `Concatenate` to type higher-order functions in modern Python.

---

## 7) Exercises
1. Implement `memoize` decorator using closures without `lru_cache`.
2. Build `compose(*funcs)` that composes N callables left-to-right.
3. Write a `retry` higher-order function that returns a wrapped callable with retry/backoff.