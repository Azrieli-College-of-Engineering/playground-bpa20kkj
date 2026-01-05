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
* 8.5 Core Array Properties](#85-core-array-properties)

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