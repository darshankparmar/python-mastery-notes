# Inheritance — Composition, ABCs, and Multiple Inheritance

## 1) Prefer Composition
Only inherit when you truly model an "is-a" relationship and need polymorphism.

## 2) Abstract Base Classes
```python
from abc import ABC, abstractmethod

class Storage(ABC):
    @abstractmethod
    def get(self, key: str) -> bytes: ...
```

## 3) Multiple Inheritance & MRO
- Python uses C3 linearization (MRO). Inspect with `Class.__mro__`.
- Use cooperative `super()`.
```python
class A: 
    def __init__(self): super().__init__(); print("A")
class B: 
    def __init__(self): super().__init__(); print("B")
class C(A, B):
    def __init__(self): super().__init__(); print("C")
```

## 4) Mixins
- Provide focused behavior (e.g., `ReprMixin`) and require well-documented expectations.

## 5) Exercises
1. Create `Serializer` ABC with `to_bytes/from_bytes`; implement JSON and MsgPack variants.
2. Design a `TimedCache` using mixin for TTL eviction with cooperative `super()`.