---
draft: true
date: 2024-07-13
title: "Big O for Python lists"
slug: "Python-lists-BigO"
authors: "Diogo Pessoa"
codeMaxLines: 10 
codeLineNumbers: false 
categories:
  - coding
tags:
  - python
  - in-hindsight
series:
  - diary-of-a-flawed-pythonista

---

# Summary

Big O of python list operations.

## Operations

### append() an element to the end of a list O(1).

Constant time complexity because the operation does not depend on the size of the list.

```python
    arr = [1, 2, 3, 4, 5]
    arr.append(6)
    print(arr) # [1, 2, 3, 4, 5, 6]
```

### pop() an element from the end of a list O(1).

Constant time complexity because the operation does not depend on the size of the list.

```python
    arr = [1, 2, 3, 4, 5]
    arr.pop()
    print(arr) # [1, 2, 3, 4]
```

### pop(x) an element from a specific index O(n).

Linear time complexity because the operation depends on the size of the list.
`[Revised]` Why? because we need to shift all the elements to the left. after removing
the element.

```python
    arr = [1, 2, 3, 4, 5]
    arr.pop(2)
    print(arr) # [1, 2, 4, 5]
```

### Insert() an element at a specific index O(n).

Linear time complexity because the operation depends on the size of the list.
`[Revised]` Why? because it needs to shift all elements after insertion.
even if we insert in the middle of the list, the operation will take O(n) time because
we need to shift all the elements to the right.

and the notation is `O(n)` because the half is a constant and we drop constants in Big O
notation.

```python
    arr = [1, 2, 3, 4, 5]
    arr.insert(2, 10)
    print(arr) # [1, 2, 10, 3, 4, 5]
```

### Search for an element by value in a list O(n).

Linear time complexity because the operation depends on the size of the list. We don't
know where the value is.

```python
    arr = [1, 2, 3, 4, 5]
    print(arr.index(3)) # 2
```

### Get by index in a list O(1).

We're accessing the element by index, so it's a constant time operation.

```python
    arr = [1, 2, 3, 4, 5]
    print(arr[2]) # 3
```

