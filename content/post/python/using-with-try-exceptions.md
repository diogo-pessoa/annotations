---
title: "Note to self: Why `with` matters (and when to add `try/except`)"
date: 2026-02-05
draft: true
toc: true
tags:
  - python
  - error-handling
series:
  - diary-of-a-flawed-pythonista
---

### Summary 
When working with resources like **files, sockets, locks, and DB connections**, use `with` because it guarantees *
*cleanup happens no matter what**—even if there’s an exception, an early `return`, etc.

`with` uses a **context manager**:

* `__enter__()` sets things up and returns the usable object.
* `__exit__()` runs on exit and handles teardown (close/release/rollback).

Think of `with` as a built-in `try/finally`:

* **`with` ≈ `try/finally` for cleanup**
* It ensures: file descriptors close, socket closes, locks release, transactions finish safely.

### Key takeaway

`with` does **not** replace `try/except`.

* You **don’t need** `try/finally` around `with` just to close things.
* You **might still want** `try/except` around (or inside) `with` to **handle errors**:

    * log with context,
    * retry/backoff,
    * return a fallback,
    * wrap/translate low-level exceptions into domain-specific errors.

### Practical patterns

1. **Let it raise** (caller handles it):

```python
with open(path) as f:
    data = f.read()
```

2. **Catch + log + re-raise** (add context, keep stack trace):

```python
try:
    with open(path) as f:
        data = f.read()
except OSError:
    log.exception("Failed reading %s", path)
    raise
```

3. **Catch inside** if only part of the block is “optional” or failure-tolerant:

```python
with open(path) as f:
    try:
        return parse(f.read())
    except ValueError:
        return None
```

### Rule of thumb

* Default to `with` for anything that needs deterministic release.
* Add `try/except` only when you have a **decision** to make (log, retry, fallback, translate error).

## References

* Python documentation. (2026). 8. Compound statements. [online] Available at: https://docs.python.org/3/reference/compound_stmts.html#with [Accessed 5 Feb. 2026].
* GeeksforGeeks (2019). with statement in Python. [online] GeeksforGeeks. Available at: https://www.geeksforgeeks.org/python/with-statement-in-python/ [Accessed 5 Feb. 2026].