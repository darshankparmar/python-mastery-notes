# Decorators — Metaprogramming with Functions and Classes

## 1) First Principles
A decorator is a callable that takes a function/class and returns a new callable/class.

```python
def log_calls(fn):
    def wrapper(*args, **kwargs):
        print(f"→ {fn.__name__}({args}, {kwargs})")
        out = fn(*args, **kwargs)
        print(f"← {out!r}")
        return out
    return wrapper

@log_calls
def add(a,b): return a+b
```

## 2) Preserving Metadata
```python
from functools import wraps
def deco(fn):
    @wraps(fn)
    def w(*a, **k): return fn(*a, **k)
    return w
```

## 3) Parameterized Decorators
```python
def retry(tries=3):
    def deco(fn):
        @wraps(fn)
        def w(*a, **k):
            import time, random
            for i in range(tries):
                try: return fn(*a, **k)
                except Exception as e:
                    if i == tries-1: raise
                    time.sleep(2**i * 0.1)
        return w
    return deco
```

## 4) Class Decorators and Dataclass
```python
def add_repr(cls):
    def __repr__(self): return f"{cls.__name__}({self.__dict__})"
    cls.__repr__ = __repr__
    return cls

@add_repr
class C: 
    def __init__(self,x): self.x = x
```

## 5) Method Decorators & Descriptors
```python
class cached_property:
    def __init__(self, fn): self.fn = fn; self.attr = f"_{fn.__name__}_cache"
    def __get__(self, obj, owner):
        if obj is None: return self
        if not hasattr(obj, self.attr):
            setattr(obj, self.attr, self.fn(obj))
        return getattr(obj, self.attr)
```

## 6) Pitfalls & Testing
- Decorators can hide side-effects; use `wraps` and docstrings.
- Keep decorators orthogonal (single responsibility).

## 7) Exercises
1. Implement `timeit` decorator that records run time to a logger.
2. Write an `authorize(role)` decorator that checks a context object.
3. Build `validate(schema)` that validates kwargs and raises `TypeError`.