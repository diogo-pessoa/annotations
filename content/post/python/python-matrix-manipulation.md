---
title: "Python Matrix Manipulation"
date: 2024-07-03
description: "Python Matrix Manipulation exercises from OCW Introduction to Linear Algebra"
featured: false
draft: false
toc: true
# menu: main
usePageBundles: false
codeMaxLines: 10 
codeLineNumbers: false 
figurePositionShow: true 
categories:
  - "coding" 
  - "Linear Algebra"
tags:
  - Python
  - Matrix
series:
  - diary-of-a-flawed-pythonista
---

**Warning:** Note to self, this is in-progress and being updated ad-hoc, whenever I get a chance to
work on it.

## Summary

Matrix manipulation is something I need to refresh everytime I have to use it. It's not
something one uses every day.

**Matrix:** 2D array (matrix) with given dimensions (rows and columns).

This note is a hope to capture the exercises and learning from
Prof. Gilbert Strang OCW
course [Introduction to Linear Algebra](https://ocw.mit.edu/courses/18-06-linear-algebra-spring-2010/)

Bringing together two topics I enjoy Matrices and Python.

- **My implementation:**
    - [Matrix](https://github.com/diogo-pessoa/coding-exercises-for-interviews/blob/main/dataStructures/matrix/Matrix.py)
    - [Matrix Tests](https://github.com/diogo-pessoa/coding-exercises-for-interviews/blob/main/dataStructures/matrix/test_Matrix.py)

  The idea is to add other Matrices types to explore other scenarios. Matrix is just the initial
  super class

## Observations

#### Rectangular Matrix

A matrix where each row has the same number of columns. This is a basic requirement for
matrix operations like addition and multiplication to be correctly defined.

_first-time using:_ `all()` - Built-in function that validate is all elements in
iterable are
true [python Docs](https://docs.python.org/3.11/library/functions.html#all)

```python
  if not all(len(row) == len(data[0]) for row in data):
    raise ValueError("All rows must have the same number of columns")
```

#### Symmetric (a square) Matrix

A square matrix that is equal to its transpose. In other words, a matrix `A` is
symmetric
if `A = A^T`. This means that the elements of the matrix are symmetric with respect to
the main diagonal.

#### Types of Matrices

- **Square Matrix:** A matrix with the same number of rows and columns.
- **Diagonal Matrix:** A square matrix where all the elements outside the main diagonal
  are zero.
- **Identity Matrix:** A diagonal matrix where all the elements on the main diagonal are
    1.
- **Zero Matrix:** A matrix where all the elements are zero.
- **Row Matrix:** A matrix with a single row and multiple columns.
- **Column Matrix:** A matrix with a single column and multiple rows.
- **Scalar Matrix:** A diagonal matrix where all the elements on the main diagonal are
  equal.
- **Symmetric Matrix:** A square matrix that is equal to its transpose.
- **Triangular Matrix:** A matrix where all the elements above or below the main
  diagonal are zero.
- **Sparse Matrix:** A matrix where most of the elements are zero.
- **Unit Matrix:** A square matrix where all the elements are zero, except for the main
  diagonal, which is 1.
- **Orthogonal Matrix:** A square matrix whose transpose is equal to its inverse.
- **Hermitian Matrix:** A square matrix that is equal to its conjugate transpose.
- **Skew-Symmetric Matrix:** A square matrix where the transpose is equal to the
  negative of the original matrix.

## Exercises

### Exercise 1: Create a 2D Array (Matrix)

Write a function to create a and fill it with a given value.

```python
def create_matrix(rows, cols, value):
    return [[value for _ in range(cols)] for _ in range(rows)]

# Example usage:
rows = 3
cols = 4
value = 0
matrix = create_matrix(rows, cols, value)
print(matrix)
```

### Exercise 2: Matrix Addition

Write a function to add two matrices of the same dimensions.

```python
def add_matrices(matrix1, matrix2):
    rows = len(matrix1)
    cols = len(matrix1[0])
    result = [[0 for _ in range(cols)] for _ in range(rows)]
    
    for i in range(rows):
        for j in range(cols):
            result[i][j] = matrix1[i][j] + matrix2[i][j]
    
    return result

# Example usage:
matrix1 = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

matrix2 = [
    [9, 8, 7],
    [6, 5, 4],
    [3, 2, 1]
]

result = add_matrices(matrix1, matrix2)
print(result)
```

### Exercise 3: Matrix Multiplication

Write a function to multiply two matrices.

```python
def multiply_matrices(matrix1, matrix2):
    rows_matrix1 = len(matrix1)
    cols_matrix1 = len(matrix1[0])
    rows_matrix2 = len(matrix2)
    cols_matrix2 = len(matrix2[0])
    
    if cols_matrix1 != rows_matrix2:
        raise ValueError("Cannot multiply matrices: number of columns in the first matrix must equal number of rows in the second matrix.")
    
    result = [[0 for _ in range(cols_matrix2)] for _ in range(rows_matrix1)]
    
    for i in range(rows_matrix1):
        for j in range(cols_matrix2):
            for k in range(cols_matrix1):
                result[i][j] += matrix1[i][k] * matrix2[k][j]
    
    return result

# Example usage:
matrix1 = [
    [1, 2],
    [3, 4],
    [5, 6]
]

matrix2 = [
    [7, 8, 9],
    [10, 11, 12]
]

result = multiply_matrices(matrix1, matrix2)
print(result)
```

### Exercise 4: Transpose of a Matrix

Write a function to transpose a matrix.

```python
def transpose_matrix(matrix):
    rows = len(matrix)
    cols = len(matrix[0])
    
    result = [[0 for _ in range(rows)] for _ in range(cols)]
    
    for i in range(rows):
        for j in range(cols):
            result[j][i] = matrix[i][j]
    
    return result

# Example usage:
matrix = [
    [1, 2, 3],
    [4, 5, 6]
]

result = transpose_matrix(matrix)
print(result)
```

## References

1. [Matrix (Wikipedia)](https://en.wikipedia.org/wiki/Matrix_(mathematics))
2. [Introduction to Linear Algebra](https://math.mit.edu/~gs/linearalgebra/ila6/indexila6.html)
3. [OCW Introduction to Linear Algebra](https://ocw.mit.edu/courses/18-06-linear-algebra-spring-2010/)
