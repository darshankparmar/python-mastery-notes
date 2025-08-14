# Modules, Packages, pip, and File I/O — Real-World Workflow

## 1) Modules & Packages
- **Module** = a `.py` file; **Package** = a directory with `__init__.py` (namespace packages can omit it).
- Use absolute imports inside packages.

```
mypkg/
    __init__.py
    util.py
    core/
        __init__.py
        algo.py
```

```python
from mypkg.core.algo import solve
```

### `__all__` and Public API
In `__init__.py` export stable symbols:
```python
__all__ = ["solve", "VERSION"]
```

---

## 2) Virtual Environments & pip
```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# Linux/Mac:
source .venv/bin/activate
pip install -U pip
pip install requests==2.*
pip freeze > requirements.txt
pip install -r requirements.txt
```

### Alternatives
- `pipx` for global CLI tools.
- `uv`/`pip-tools`/`poetry` for pinning & lockfiles.

---

## 3) Packaging (Brief)
- Put metadata in `pyproject.toml`.
- Build with `python -m build`; install with `pip install dist/*.whl`.

---

## 4) File I/O (Text, Binary, JSON, CSV, Pathlib)
```python
from pathlib import Path
p = Path("data.txt")
p.write_text("hello", encoding="utf-8")
text = p.read_text(encoding="utf-8")

with p.open("a", encoding="utf-8") as f:
    f.write("\nmore")
```

Binary:
```python
data = b"\x00\x01"
Path("bin.dat").write_bytes(data)
```

JSON & CSV:
```python
import json, csv, pathlib

with open("data.json","w", encoding="utf-8") as f:
    json.dump({"x":1}, f, ensure_ascii=False, indent=2)

with open("data.csv","w", newline="", encoding="utf-8") as f:
    w = csv.writer(f)
    w.writerow(["id","name"])
```

### Robust File Handling
- Always specify encoding for text.
- Use `with` to ensure closure.
- For large files, stream in chunks.

---

## 5) Exercises
1. Build a CLI that reads a CSV and outputs a JSON grouped by a column.
2. Create a small package with `pyproject.toml` and one submodule; install it locally.
3. Write `atomic_write(path, text)` using `tempfile` + `os.replace`.