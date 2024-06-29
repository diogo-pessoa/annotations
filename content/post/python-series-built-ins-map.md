+++
date = "2024-06-29"
title = "Playing with built-ins: map()"
description = "How map() makes it easier to transform values through iterables."
slug = "python-map-transforming-values"
authors = "Diogo Pessoa"
tags = ["python", "functional-programming"]
categories = ["coding"]
series = ["python built-ins"]
toc = true
+++

## Summary

Python map function are built-in functions that are used to process
iterables.

I see this approach often when reading code with a functional programming approach.

I've been meaning to try it out and write about to weight the pros and cons. Then, have a quick
summary as reference when using.

### Conventions & Code

{{% notice tip "Tip" %}}

- [Conventions](https://diogo-pessoa.github.io/posts/conventions)
- [My code implementation](https://github.com/diogo-pessoa/coding-exercises/blob/main/functional-programming/MapTransform.py)
  {{% /notice %}}

## The Map function

The Map Function applies a given function to all items in an input list. It returns
a `map Object`.

I found the function call extra useful. .map(function, iterable). Pair that
with [lambda expression](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions).
It's possible to write succinct code.

```python
    def test_mapping_built_in(self):
        numbers = [1, 2, 3, 4]
        squared = map(lambda x: x ** 2, numbers)
        print(type(squared))
        print(list(squared))
        print(squared.__doc__)
```

```python
    <class 'map'>
    [1, 4, 9, 16]
    map(func, *iterables) --> map object

    Make an iterator that computes the function using arguments from
    each of the iterables.  Stops when the shortest iterable is exhausted
```

### practical example

A crossed an actual case where this proved useful (simplified the code readability).

I'm normally skeptical of contracting the code too much, fearing it makes harder to read and
understand the logic. But in this case it streamlined the code. By extracting the transform into a
function and calling it through the `map()`

Here's the idea:

In this scenario. We're saving a tuple with key and value. Where the actual key is whatever is after
the `#`.

```python

    def _reduce_key_to_pattern(self, key):
        return key.split("#")[1]

    def transform_keys(self, string_to_transform: dict) -> list[tuple]:
        """
        Given a dictionary, return a a list of tuples with the chars after "#".
        :param string_to_transform: dict
        :return: list[tuple]
        """
        return list(map(lambda x: (self._reduce_key_to_pattern(x[0]), x[1]), string_to_transform.items()))
```

## References

1. Map: Built-in Functions (no date) Python documentation. Available
   at: https://docs.python.org/3/library/functions.html (Accessed: 1 June 2024).
2. Lambda-expressions. Available
   at: https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions (Accessed:
   19 June 2024).
3. functools.reduce — Higher-order functions and operations on callable objects (no
   date) Python documentation. Available
   at: https://docs.python.org/3/library/functools.html#functools.reduce (
   Accessed: 19
   June 2024).




