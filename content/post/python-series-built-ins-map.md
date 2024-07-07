+++
date = "2024-06-30"
title = "Exploring Python built-ins: map()"
description = "Does using the map() makes it easier to transform values through iterables?"
slug = "python-map-transforming-values"
authors = "Diogo Pessoa"
tags = ["python", "functional-programming"]
categories = ["coding"]
series = ["python built-ins"]
toc = true
+++

## Summary

Python has the `map()` built-in function, quite useful for iterable objects. By applying a function
and
returning a map object. `(Generally transformed into a list)`

I see this approach often when reading code with a functional programming approach. I've been
meaning to try it out and write about to weight the pros and cons. Then, have a quick
summary as reference when/if using the `map()`.

### Conventions & Code

{{% notice tip "Tip" %}}

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

## practical example

A crossed an actual case where this proved useful (simplified the code readability).

I'm skeptical of contracting the code too much, fearing it makes harder to read and
understand the logic. But in this case it streamlined the code. By extracting the transform into a
function and calling it through the `map()`

Here's the idea:

In this scenario. We're saving a tuple with key and value. Where the actual key is whatever is after
the `#`.

```python

    def _reduce_key_to_pattern(self, key):
        return key.split("#")[1]

    def _build_tuple_from_item(self, item: Tuple[str, Any]) -> Tuple[str, Any]:
        return self._reduce_key_to_pattern(item[0]), item[1]

    def transform_keys(self, string_to_transform: dict) -> list[tuple]:
        """
        Given a dictionary, return a a list of tuples with the chars after "#".
        :param string_to_transform: dict
        :return: list[tuple]
        """
        return list(map(self._build_tuple_from_item, string_to_transform.items()))
```

* when calling the `map()` the `string_to_transform.items()` -> `<class 'dict_items'>`,

The map, implicitly calls the self._build_tuple_from_item. Then, passing each item in the iterable
and appending it to the returning list.

A few other ways of writing the same iteration in python. Now, at this point it all sounds as
syntactic Sugar. However, when decorating the run with a timer, I didn't see a massive benefit in
execution. `(the map() actually it was the slowest, in this example)`

The list comprehension, is probably the most "pythonic" way of approaching this. However, since I'm
a
fan of a good debugging session. The traditional looping was the most beneficial to me. There wasn't
a massive increase in execution time and the debugger is much more human-readable. Not to mention,
for a programmer coming from a different language, it's way more obvious what's going on in the
loop.  
`(One could argue about space completixy, since I'm creating an extra variable)`

**Warning:** This wasn't meant to be the most efficient code `(not a Big O analysis)`, just playing
with the `map()` function.

## Exploring the alternatives


### Using a "traditional loop"

```python
...
result = []
for item in string_to_transform.items():
    result.append((self._reduce_key_to_pattern(item[0]), item[1]))
return result
```

### Using a "list comprehension":

```python
...
return [(self._reduce_key_to_pattern(item[0]), item[1]) for item in string_to_transform.items()]
...
```

## Test runs with @timeit_decorator and debugger screenshots

### Loop and Append

![for_loop_debugger.png](/images/map_python/for_loop_debugger.png)

* Test output

```python
Running Test: test_transform_keys_with_loop
transform_keys_with_loop_and_append took 0.000003 seconds to execute
```

### List comprehension

![List_comprehension_debugger.png](/images/map_python/List_comprehension_debugger.png)

* Test output

```python
Running Test: test_transform_keys_list_comprehension
transform_keys_with_comprehension took 0.000003 seconds to execute
```

### map()

![map_debugger.png](/images/map_python/map_debugger.png)

```python
Running Test: test_transform_keys_map
transform_keys took 0.000067 seconds to execute
```

## Conclusion

Wait for it… It depends. :unamused:

I'd definitely use the `map()` function in some use cases where the transformation is simple, for
example, transforming a value `map(lambda x: x ** 2, numbers)` through an iterable. But, it isn't
immediately obvious when
writing the method how the whole thing will behave `(or maybe, I'm just unexperienced)`. I still
rely on
too much on the debugger `(and testing re-runs)` to understand the logic.

Another point to think about is the implicit logic. I imagine future me, trying to read through the
code without a docstring and the lack of debugger visibility. :cursing_face:

All in all, it's a good tool to have in the toolbox. Understand how it works and when coupling it
with filter(), and reduce(). It can speed up the development process. **But**, it's not for
beginners.

## References

1. Map: Built-in Functions (no date) Python documentation. Available
   at: https://docs.python.org/3/library/functions.html#map (Accessed: 1 June 2024).
2. Lambda-expressions. Available
   at: https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions (Accessed:
   19 June 2024).
