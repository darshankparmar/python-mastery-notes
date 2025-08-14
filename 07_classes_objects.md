# Classes & Objects — Data Models and Pythonic OOP

## 1) Data Model Dunder Methods
```python
class Vector:
    def __init__(self, x, y): self.x, self.y = x, y
    def __repr__(self): return f"Vector({self.x}, {self.y})"
    def __add__(self, other): return Vector(self.x+other.x, self.y+other.y)
    def __iter__(self): yield from (self.x, self.y)
    def __eq__(self, other): return tuple(self) == tuple(other)
```
Implement `__hash__` only if immutable.

## 2) Properties
```python
class User:
    def __init__(self, email): self._email = email
    @property
    def email(self): return self._email
    @email.setter
    def email(self, v): 
        if "@" not in v: raise ValueError("bad email")
        self._email = v
```

## 3) Class vs Instance Attributes
- Class attrs shared across instances; be careful with mutable class attributes.

## 4) Dataclasses & Slots
```python
from dataclasses import dataclass

@dataclass(slots=True, frozen=True)
class Point: x: float; y: float
```

`slots` saves memory; `frozen` makes immutable/hashable.

## 5) Static & Class Methods
```python
class Factory:
    @staticmethod
    def add(a,b): return a+b
    @classmethod
    def from_conf(cls, conf): return cls(**conf)
```

## 6) Protocols (Structural Subtyping)
```python
from typing import Protocol
class SupportsClose(Protocol):
    def close(self) -> None: ...
```

## 7) Exercises
1. Create an immutable `Money` type with currency, arithmetic, and normalization.
2. Implement `Polynomial` supporting `+`, `*`, `callable(x)` evaluating value.