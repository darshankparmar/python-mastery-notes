# Iterators & Generators — Lazy, Composable Data Pipelines

## 1) Iterator Protocol
- `__iter__` returns iterator; `__next__` yields next or raises `StopIteration`.

## 2) Generators
```python
def countdown(n):
    while n:
        yield n
        n -= 1
```

### Generator Methods
- `.send(value)`, `.throw(exc)`, `.close()`.

### Yield From (Delegation)
```python
def flatten(list_of_lists):
    for sub in list_of_lists:
        yield from sub
```

## 3) Combinators via `itertools`
- `islice`, `takewhile`, `dropwhile`, `accumulate`, `tee`, `groupby`.
```python
from itertools import islice, accumulate
list(islice(range(1000), 5))      # first 5
list(accumulate([1,2,3,4]))       # prefix sums
```

## 4) Coroutines (PEP 342) vs `async def`
Generators with `send` are coroutines-lite; prefer `async`/`await` for I/O.

## 5) Performance
- Lazy pipelines reduce memory.
- Beware of tee() duplicating data buffers.

## 6) Exercises
1. Implement `window(iterable, size)` yielding overlapping windows.
2. Build `tail(path, n=10)` like Unix `tail -f` using generators.
3. Write a push-based coroutine that computes running average via `.send()`.