# Exceptions — Design, Handling, and Robust Programs

## 1) Philosophy
- EAFP: Easier to Ask Forgiveness than Permission. Try/except is idiomatic when races exist.
- Use exceptions for exceptional cases, not normal control flow.

## 2) Basics
```python
try:
    x = 1 / y
except ZeroDivisionError as e:
    log(e)
else:
    commit()
finally:
    cleanup()
```

## 3) Custom Exceptions
```python
class DomainError(Exception): pass
class ConfigError(DomainError): pass
```

## 4) Chaining & Context
```python
try:
    parse()
except ValueError as e:
    raise ConfigError("bad config") from e
```

## 5) Granularity
- Catch the narrowest exception you can handle.
- Never use bare `except:` unless re-raising.

## 6) Resource Safety
- Context managers (`with`) ensure deterministic cleanup.
```python
from contextlib import contextmanager

@contextmanager
def opened(path, mode):
    f = open(path, mode, encoding="utf-8")
    try:
        yield f
    finally:
        f.close()
```

## 7) Retry/Backoff
Use `time.sleep`/exponential backoff, but cap retries and surface root causes.

## 8) Exercises
1. Write `retry(fn, tries=3, delay=0.1, backoff=2)`.
2. Implement a `Timeout` context manager using `signal` on Unix (document Windows caveat).