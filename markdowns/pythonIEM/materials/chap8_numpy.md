# Chapter 8 – NumPy

## Table of Contents

* 8.1 Purpose and Learning Outcomes
* 8.2 Why NumPy Matters (Performance and Vectorization)
* 8.3 NumPy Arrays vs Python Lists

* 8.4 Creating Arrays
  * 8.4.1 From Python Data (list/tuple)
  * 8.4.2 Zeros, Ones, Full, Empty
  * 8.4.3 Ranges and Even Spacing (`arange`, `linspace`)
  * 8.4.4 Random Arrays (Reproducibility with Seeds)
  * 8.4.5 Specifying the Data Type Explicitly

* 8.5 Core Array Properties
  * 8.5.1 `dtype` and Type Casting
  * 8.5.2 `shape`, `ndim`, `size`
  * 8.5.3 Views vs Copies (Why It Matters)

* 8.6 Indexing, Slicing, and Selecting Data
  * 8.6.1 1D Indexing and Slices
  * 8.6.2 2D Indexing (Row/Column Access)
  * 8.6.3 Fancy Indexing (Index Arrays)
  * 8.6.4 Boolean Masking (Filtering)

* 8.7 Vectorized Operations (Elementwise Computation)
  * 8.7.1 Arithmetic and Comparisons
  * 8.7.2 Universal Functions (Ufuncs)
  * 8.7.3 Aggregations and Reductions (`sum`, `mean`, …)

* 8.8 Working with Axes (The Most Common Source of Bugs)
  * 8.8.1 `axis=None` vs `axis=0` vs `axis=1`
  * 8.8.2 Practical Industrial Examples (Weekly/Batch Summaries)

* 8.9 Reshaping and Rearranging Arrays
  * 8.9.1 `reshape` (Including `-1`)
  * 8.9.2 Flattening (`ravel`, `flatten`)
  * 8.9.3 Transpose and Axis Permutation (`T`, `transpose`)

* 8.10 Broadcasting (Rules and Safe Usage)
  * 8.10.1 Broadcasting Rules
  * 8.10.2 Common Pitfalls (1D vs 2D Shapes)

* 8.11 Combining and Splitting Arrays
  * 8.11.1 Stacking (`hstack`, `vstack`, `stack`)
  * 8.11.2 Concatenation (`concatenate`)
  * 8.11.3 Splitting (`split`, `hsplit`, `vsplit`)

* 8.12 Basic Linear Algebra (Foundations)
  * 8.12.1 Dot Product and Matrix Multiplication (`dot`, `@`)
  * 8.12.2 Solving Systems and Inverses (Intro)

* 8.13 Mini-Labs (Tech.io Runnable Checkpoints)
  * 8.13.1 Lab A — Production Data Reshape + Weekly Totals
  * 8.13.2 Lab B — Filtering and Outlier Detection via Masks
  * 8.13.3 Lab C — Broadcasting for Normalization

* 8.14 Common Mistakes and Debugging Checklist

* 8.15 Summary and Preparation for pandas

---

## 8.1 Purpose and Learning Outcomes

### Purpose

NumPy (Numerical Python) is the **foundational numerical computing library** in the Python ecosystem. Almost all data-science, scientific-computing, and engineering libraries in Python (including **pandas**, **SciPy**, and **scikit-learn**) are built directly on top of NumPy arrays.

In this course, NumPy serves three critical roles:

1. It introduces **vectorized thinking** (operating on whole datasets at once).
2. It establishes the concept of **multi-dimensional data** (1D, 2D, and beyond).
3. It provides the numerical foundation required to properly understand pandas DataFrames later.

Unlike pure Python lists, NumPy arrays are designed for **performance, memory efficiency, and mathematical correctness**.

---

## 8.2 Why NumPy Matters (Performance and Vectorization)

### The Core Idea: Vectorization

**Vectorization** means expressing computations as operations on entire arrays rather than explicit element-by-element loops.

In numerical and engineering contexts, this leads to:

* Cleaner and more readable code
* Fewer logical errors
* Orders-of-magnitude performance improvements

---

### Python Lists vs NumPy Arrays (Conceptual)

Consider a simple operation: multiplying every value by 2.

#### Using Python lists

```python runnable
values = [1, 2, 3, 4]
result = []

for x in values:
    result.append(x * 2)

print(result)
```

This approach:

* Is verbose
* Requires explicit looping
* Scales poorly for large datasets

---

#### Using NumPy arrays (vectorized)

```python runnable
import numpy as np

values = np.array([1, 2, 3, 4])
result = values * 2

print(result)
```

This approach:

* Is concise
* Expresses intent clearly
* Leverages optimized C-level implementations

---

### Why NumPy Is Faster

NumPy arrays:

* Store data in **contiguous memory blocks**
* Use **fixed data types** (`dtype`)
* Delegate heavy computation to **compiled C/Fortran code**

Python lists, by contrast:

* Store references to Python objects
* Require dynamic type checks
* Incur significant overhead per element

---

### A Small Performance Illustration

```python runnable
import numpy as np

n = 1_000_000
arr = np.arange(n)

# Vectorized operation
result = arr * 2
```

Even without measuring time explicitly, this pattern is **dramatically faster** than looping in Python.

---

### Why This Matters for IEM and Engineering Problems

In industrial and engineering settings, data often represents:

* Production quantities over time
* Sensor readings
* Resource utilization matrices
* Cost and performance metrics

Such data naturally forms **arrays and matrices**, not individual scalar values.

NumPy allows you to:

* Model these datasets directly
* Apply mathematical operations correctly
* Scale solutions from small examples to real systems

---

> **Key Takeaway:**
> NumPy is not just a convenience — it is the **numerical backbone** of Python-based engineering and data analysis.

---

## 8.3 NumPy Arrays vs Python Lists

Although NumPy arrays and Python lists may appear similar at first glance, they are **fundamentally different data structures** designed for different purposes.

Understanding these differences is essential. Many common bugs, performance issues, and design mistakes arise from treating NumPy arrays as if they were Python lists.

---

### Conceptual Differences

| Aspect                  | Python List                         | NumPy Array                  |
| ----------------------- | ----------------------------------- | ---------------------------- |
| Primary purpose         | General-purpose container           | Numerical computation        |
| Data types              | Heterogeneous (mixed types allowed) | Homogeneous (single `dtype`) |
| Memory layout           | References to Python objects        | Contiguous memory block      |
| Performance             | Slower for numerical ops            | Optimized for numerical ops  |
| Mathematical operations | Manual looping                      | Vectorized operations        |

A Python list is a **flexible container**.
A NumPy array is a **numerical data structure**.

---

### Type Homogeneity and `dtype`

Python lists can store values of different types:

```python
lst = [1, 2.5, "A", True]
print(lst)
```

NumPy arrays enforce a **single data type** (`dtype`):

```python runnable
import numpy as np

arr = np.array([1, 2.5, 3])
print(arr)
print(arr.dtype)
```

NumPy automatically chooses the **smallest common type** that can represent all elements.

> **Important:**
> Mixed-type numerical data is often a modeling error. NumPy forces you to be explicit.

---

### Memory Layout and Performance Implications

Python lists store **references** to objects scattered throughout memory.

NumPy arrays store raw numerical data in **contiguous memory**, which enables:

* Efficient CPU caching
* Vectorized operations
* Fast interaction with low-level numerical libraries

This design is the primary reason NumPy scales to millions of elements efficiently.

---

### Elementwise Operations: A Critical Difference

Consider elementwise multiplication.

#### Python list behavior

```python
[1, 2, 3] * 2
```

Output:

```text
[1, 2, 3, 1, 2, 3]
```

This performs **list repetition**, not multiplication.

---

#### NumPy array behavior

```python
import numpy as np

np.array([1, 2, 3]) * 2
```

Output:

```text
[2 4 6]
```

This performs **elementwise numerical multiplication**.

> **Key Pitfall:**
> Assuming Python list operators behave numerically leads to silent logical errors.

---

### Mutability and Views (Preview)

Both Python lists and NumPy arrays are mutable, but NumPy introduces an additional concept: **views**.

* A **copy** owns its own data
* A **view** references existing data

This distinction does not exist for Python lists and will be explored in detail later in the chapter.

For now, remember:

> **NumPy operations may modify the original data if a view is returned.**

---

### When to Use Each

Use **Python lists** when:

* Data is heterogeneous
* Size is small
* Flexibility is more important than performance

Use **NumPy arrays** when:

* Data is numerical
* Performance matters
* Mathematical operations are required
* Data represents vectors, matrices, or tensors

---

> **Takeaway:**
> Python lists are containers. NumPy arrays are mathematical objects.

In the next section, we will start working concretely with NumPy by learning the various ways to **create arrays**.

---

## 8.4 Creating Arrays

Creating NumPy arrays correctly is the first practical skill students must master. NumPy provides **multiple array creation mechanisms**, each suited to different use cases. Choosing the right one improves clarity, correctness, and performance.

---

### 8.4.1 From Python Data (list / tuple)

The most straightforward way to create a NumPy array is by converting an existing Python sequence.

```python
import numpy as np

arr = np.array([1, 2, 3, 4])
print(arr)
print(type(arr))
```

Key observations:

* The input can be a list or a tuple
* The resulting object is a `numpy.ndarray`
* NumPy infers the data type automatically

---

#### Creating Multi-Dimensional Arrays

Nested sequences produce higher-dimensional arrays.

```python
matrix = np.array([[1, 2, 3],
                   [4, 5, 6]])
print(matrix)
print(matrix.shape)
```

Important rules:

* Inner sequences must have the same length
* Irregular (ragged) input usually indicates a modeling error

> **Important:**
> If inner lists differ in length, NumPy may fall back to `dtype=object`, which defeats NumPy’s numerical advantages.

---

### 8.4.2 Zeros, Ones, Full, Empty

When the shape of the data is known in advance, NumPy provides dedicated constructors.

```python
np.zeros((3, 4))
np.ones((2, 2))
np.full((2, 3), 7)
```

These are commonly used for:

* Initialization
* Preallocation for performance
* Placeholder matrices

---

#### The `empty()` Function (Use with Care)

```python
np.empty((2, 3))
```

`empty()` allocates memory **without initializing values**.

> **Warning:**
> The contents of an `empty` array are unpredictable. Use it only when every element will be overwritten.

---

### 8.4.3 Ranges and Even Spacing (`arange`, `linspace`)

NumPy provides two commonly confused functions for generating sequences.

---

#### `arange()` — Step-based sequences

```python
np.arange(0, 10, 2)
```

Produces values starting at 0, incremented by 2, up to (but not including) 10.

> **Note:**
> Floating-point steps with `arange()` can lead to precision issues.

---

#### `linspace()` — Count-based sequences

```python
np.linspace(0, 10, 5)
```

Produces **exactly 5 evenly spaced values** between 0 and 10 (inclusive).

> **Rule of thumb:**
> Use `linspace()` when the number of points matters. Use `arange()` when the step size matters.

---

### 8.4.4 Random Arrays (Reproducibility with Seeds)

Random numbers are common in simulations, sampling, and testing.

```python
np.random.rand(2, 3)
```

This generates values uniformly distributed in the range \[0, 1).

---

#### Reproducibility with Seeds

```python
np.random.seed(42)
np.random.rand(3)
```

Setting a seed ensures:

* Identical results across runs
* Consistent grading and debugging

> **Course Rule:**
> Always set a random seed in assignments unless randomness itself is being evaluated.

---

### 8.4.5 Specifying the Data Type Explicitly

You can override NumPy’s automatic type inference using `dtype`.

```python
np.array([1, 2, 3], dtype=float)
np.zeros((2, 2), dtype=int)
```

Explicit `dtype` control is important when:

* Memory usage matters
* Interfacing with external systems
* Ensuring numerical precision

---

> **Checkpoint:**
> At this stage, you should be able to create 1D and 2D arrays using multiple constructors and understand when each is appropriate.

In the next section, we will examine **array properties** such as `dtype`, `shape`, and `ndim`, which are essential for debugging and correctness.

---

## 8.5 Core Array Properties

### 8.5.1 `dtype` and Type Casting

NumPy arrays are **homogeneous**: every element in an array has the same data type (`dtype`). This is one of the main reasons NumPy is fast and memory-efficient compared to Python lists.

Example:

```python
import numpy as np

arr1 = np.array([1, 2, 3])
arr2 = np.array([1, 2.5, 3])

print(arr1.dtype)   # typically int64
print(arr2.dtype)   # typically float64
```

If any element is a float, NumPy promotes the entire array to float. This is called **type promotion**.

You can specify the data type explicitly when creating arrays:

```python
np.array([1, 2, 3], dtype=float)
np.zeros((3, 3), dtype=np.int32)
```

Type casting allows converting an existing array to a different type:

```python
x = np.array([1.9, 2.1, 3.7])
print(x.astype(int))   # [1 2 3]
```

`astype()` always returns a **new array** (a copy), which means it uses additional memory.

---

### 8.5.2 `shape`, `ndim`, `size`

Every NumPy array has three fundamental structural properties:

```python
m = np.array([[1, 2, 3],
              [4, 5, 6]])

print(m.shape)  # (2, 3)
print(m.ndim)   # 2
print(m.size)   # 6
```

* `shape` tells you how the data is laid out (rows × columns × …)
* `ndim` tells you how many dimensions the array has
* `size` tells you how many total elements exist

These properties are critical for debugging and for using broadcasting, reshaping, and aggregation correctly.

---

### 8.5.3 Views vs Copies (Why It Matters)

Many NumPy operations return a **view** of the data rather than a copy. A view shares the same underlying memory. Modifying a view also modifies the original array.

```python
arr = np.arange(10)
sub = arr[2:6]   # this is often a view
sub[:] = -1

print(arr)
# array([ 0,  1, -1, -1, -1, -1,  6,  7,  8,  9])
```

If you need an independent copy, use `.copy()` explicitly:

```python
arr = np.arange(10)
sub = arr[2:6].copy()
sub[:] = -1

print(arr)
# original array is unchanged
```

Understanding the difference between **views** and **copies** prevents subtle and dangerous bugs when manipulating subsets of data.

---

## 8.6 Indexing, Slicing, and Selecting Data

Indexing and slicing are how you **read, modify, and extract data** from NumPy arrays. Mastering this section is critical, because incorrect indexing silently produces wrong results.

---

### 8.6.1 1D Indexing and Slices

For a one-dimensional array, indexing behaves similarly to Python lists, but slicing returns **views** (not copies) in most cases.

```python
import numpy as np

arr = np.array([10, 20, 30, 40, 50])

print(arr[0])    # 10
print(arr[-1])   # 50
print(arr[1:4])  # [20 30 40]
print(arr[:3])   # first 3 elements
print(arr[::2])  # every second element
```

Slicing does **not** copy data by default:

```python
sub = arr[1:4]
sub[:] = 999
print(arr)      # original array changed
```

Use `.copy()` if you need an independent array.

---

### 8.6.2 2D Indexing (Row/Column Access)

For 2D arrays (matrices), use `arr[row, col]` syntax.

```python
m = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])

print(m[0, 1])     # element at row 0, col 1 → 2
print(m[1])        # full row 1 → [4 5 6]
print(m[:, 2])     # full column 2 → [3 6 9]
print(m[0:2, 1:3]) # submatrix
```

Rows come first, columns second: `array[row, column]`.

---

### 8.6.3 Fancy Indexing (Index Arrays)

Fancy indexing uses arrays (or lists) of indices. This **creates a copy**, not a view.

```python
arr = np.array([10, 20, 30, 40, 50])
idx = [0, 2, 4]
print(arr[idx])  # [10 30 50]
```

For 2D arrays:

```python
m = np.arange(1, 10).reshape(3, 3)
rows = [0, 2]
cols = [1, 2]

print(m[rows, cols])   # pairs: (0,1) and (2,2)
```

This selects element-by-element pairs. To get a rectangular region:

```python
print(m[rows][:, cols])
```

---

### 8.6.4 Boolean Masking (Filtering)

Boolean masking is the most powerful data-selection technique in NumPy.

```python
arr = np.array([3, 10, -2, 7, 0])
mask = arr > 0
print(mask)        # [ True True False True False]
print(arr[mask])   # [3 10 7]
```

Combine conditions (parentheses required):

```python
print(arr[(arr >= 0) & (arr <= 7)])
```

Boolean masks allow **data filtering without loops**, which is essential for high-performance analytics.

---

## 8.7 Vectorized Operations (Elementwise Computation)

Vectorization means applying operations to **entire arrays at once** instead of looping through elements one by one. This is one of NumPy’s greatest strengths and the basis of high-performance numerical computing.

---

### 8.7.1 Arithmetic and Comparisons

NumPy overloads standard arithmetic and comparison operators so that they work **element-by-element** on arrays.

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([10, 20, 30])

print(a + b)   # [11 22 33]
print(a * b)   # [10 40 90]
print(a ** 2)  # [1 4 9]
print(a > 1)   # [False True True]
```

These operations are vectorized and therefore much faster than Python loops.

---

### 8.7.2 Universal Functions (Ufuncs)

NumPy provides a large collection of optimized **universal functions** (ufuncs) that operate element-wise.

```python
x = np.array([0.0, 1.0, 4.0])
print(np.sqrt(x))
print(np.exp(x))
print(np.log(x + 1))
```

Ufuncs handle broadcasting, type conversion, and numerical stability automatically.

---

### 8.7.3 Aggregations and Reductions (`sum`, `mean`, …)

Aggregation functions reduce many values into a single summary.

```python
m = np.array([[1, 2, 3],
              [4, 5, 6]])

print(m.sum())
print(m.mean())
print(m.min(), m.max())
```

When working with multi-dimensional arrays, aggregations can be applied along a specific axis:

```python
print(m.sum(axis=0))  # column sums
print(m.sum(axis=1))  # row sums
```

Understanding how aggregation works across axes is essential for data analysis and will be expanded in the next section.

---
