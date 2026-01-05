# Chapter 7 – Installing Python Libraries
<!--
## Table of Contents

- [7.1 Purpose of This Chapter](#71-purpose-of-this-chapter)
- [7.2 Python Libraries and the Python Ecosystem](#72-python-libraries-and-the-python-ecosystem)
- [7.3 Package Management in Python](#73-package-management-in-python)
- [7.4 Installing Python Libraries (General)](#74-installing-python-libraries-general)
- [7.5 Virtual Environments (Strongly Recommended)](#75-virtual-environments-strongly-recommended)
- [7.6 Required Libraries for This Course](#76-required-libraries-for-this-course)
- [7.7 Reproducibility with `requirements.txt`](#77-reproducibility-with-requirementstxt)
- [7.8 pandas (Spoiler): `append()` vs `concat()`](#78-pandas-spoiler-append-vs-concat)
- [7.9 Summary](#79-summary)
-->
---

## 7.1 Purpose of This Chapter <a name="71-purpose-of-this-chapter"></a>

This chapter establishes the **software environment foundation** required for the remainder of the course. Before working with NumPy, pandas, and matplotlib, students must understand:

* What Python libraries are and how they are distributed
* How package management works in modern Python
* How to install libraries **correctly and reproducibly** on their own machines
* Why **version control of dependencies** matters in academic and industrial contexts

This chapter is **conceptual and practical**. By the end, every student should be able to set up a working Python environment and understand the implications of library versions.

---

## 7.2 Python Libraries and the Python Ecosystem <a name="72-python-libraries-and-the-python-ecosystem"></a>

### What Is a Python Library?

A **Python library** is a collection of reusable code packaged to solve a specific category of problems. Libraries allow developers to:

* Avoid reinventing solutions
* Build on tested and optimized implementations
* Focus on problem-solving rather than infrastructure

Python’s strength as a language is strongly tied to its **ecosystem of libraries**.

---

### Common Categories of Python Libraries

| Category               | Examples            | Typical Use                          |
| ---------------------- | ------------------- | ------------------------------------ |
| Scientific Computing   | NumPy, SciPy        | Numerical operations, linear algebra |
| Data Analysis          | pandas              | Tabular data manipulation            |
| Visualization          | matplotlib, seaborn | Graphs, charts, plots                |
| Machine Learning       | scikit-learn        | Models, evaluation                   |
| Web Development        | Flask, Django       | Backend services                     |
| Automation / Scripting | requests, pathlib   | File and network automation          |

In this course, we focus on **scientific and data-oriented libraries**.

---

## 7.3 Package Management in Python <a name="73-package-management-in-python"></a>

### What Is `pip`?

`pip` is Python’s **package installer**. It communicates with the **Python Package Index (PyPI)**, which hosts hundreds of thousands of Python packages.

Key characteristics:

* Installs libraries and their dependencies
* Resolves versions and compatibility
* Works per Python installation

---

### Checking Your Python and pip Versions

Before installing anything, always verify your environment.

```bash
python --version
pip --version
```

You should see a Python 3.x version. For this course, Python **3.10** is recommended.

---

## 7.4 Installing Python Libraries (General) <a name="74-installing-python-libraries-general"></a>

### Installing a Single Library

```bash
pip install numpy
```

This installs the **latest compatible version** of NumPy.

---

### Installing Multiple Libraries at Once

```bash
pip install numpy pandas matplotlib
```

This is useful for quick setup but **not ideal for reproducibility**.

---

### Installing a Specific Version

```bash
pip install pandas==1.5.3
```

This guarantees the exact version is installed, which is critical for consistency across machines.

---

## 7.5 Virtual Environments (Strongly Recommended) <a name="75-virtual-environments-strongly-recommended"></a>

A **virtual environment** isolates project dependencies from the system Python installation.

### Creating a Virtual Environment

```bash
python -m venv venv
```

### Activating the Environment

**Windows:**

```bash
venv\Scripts\activate
```

**macOS / Linux:**

```bash
source venv/bin/activate
```

Once activated, `pip install` affects only this environment.

---

## 7.6 Required Libraries for This Course <a name="76-required-libraries-for-this-course"></a>

To ensure consistency across all students and grading environments, this course uses **fixed library versions**.

### Required Versions

```text
Python:     3.10
NumPy:      1.26.4
pandas:     1.5.3
matplotlib: 3.7.5
```

These versions are:

* Stable
* Widely used in academia
* Compatible with each other

---

### Installing the Course Stack

```bash
pip install numpy==1.26.4 pandas==1.5.3 matplotlib==3.7.5
```

After installation, verify:

```python
import numpy
import pandas
import matplotlib

print(numpy.__version__)
print(pandas.__version__)
print(matplotlib.__version__)
```

---

## 7.7 Reproducibility with `requirements.txt` <a name="77-reproducibility-with-requirementstxt"></a>

In professional environments, dependencies are documented using a `requirements.txt` file.

### Example `requirements.txt`

```text
numpy==1.26.4
pandas==1.5.3
matplotlib==3.7.5
```

Install all dependencies at once:

```bash
pip install -r requirements.txt
```

This guarantees everyone uses **the same versions**.

---

> ## 7.8 pandas (Spoiler): `append()` vs `concat()` <a name="78-pandas-spoiler-append-vs-concat"></a>
> 
> ### Historical Background
> 
> In earlier versions of pandas, dataframes could be extended using:
> 
> ```python
> df = df.append(new_row)
> ```
> 
> However:
> 
> * `DataFrame.append()` was **deprecated in pandas 1.4.x**
> * It was **removed entirely in pandas 2.0**
> 
> This means:
> 
> * Code using `append()` will **break** in modern pandas
> * New code should **never rely on `append()`**
> 
> ---
> 
> ### Why `append()` Was Removed
> 
> * Inefficient for repeated operations
> * Encouraged poor performance patterns
> * `concat()` provides a clearer and more flexible API
> 
> ---
> 
> ### Correct Modern Approach: `pd.concat()`
> 
> ```python
> import pandas as pd
> 
> row = pd.DataFrame([{ 'A': 10, 'B': 20 }])
> df = pd.DataFrame([{ 'A': 1, 'B': 2 }])
> 
> result = pd.concat([df, row], ignore_index=True)
> print(result)
> ```
> 
> Key advantages:
> 
> * Works across all supported pandas versions
> * Explicit and performant
> * Supports concatenating multiple dataframes at once
> 
> ---
> 
> ### When You May Still See `append()`
> 
> You may encounter `append()`:
> 
> * In older codebases
> * In outdated tutorials
> * In legacy academic material
> 
> In this course:
> 
> * You **may read** such code
> * You **must not write** new code using `append()`
> 
> ---
> 
>> ### Usage Examples:
>> 
>>> #### **`append()`:**
>>> 
>>> ##### **Basic Usage (DataFrame to DataFrame)**
>>> 
>>> ```python
>>> import pandas as pd
>>> 
>>> # Initial DataFrames
>>> df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
>>> df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})
>>> 
>>> # Appending df2 to df1
>>> df_combined = df1.append(df2, ignore_index=True)
>>> print(df_combined)
>>> ```
>>> 
>>> ##### **Appending Dictionary to DataFrame**
>>> 
>>> ```python
>>> # Append a dictionary
>>> new_row = {'A': 9, 'B': 10}
>>> df_combined = df1.append(new_row, ignore_index=True)
>>> print(df_combined)
>>> ```
>>> 
>>> ##### **Appending Series to DataFrame**
>>> 
>>> ```python
>>> # Append a Series
>>> series_row = pd.Series({'A': 11, 'B': 12})
>>> df_combined = df1.append(series_row, ignore_index=True)
>>> print(df_combined)
>>> ```
>>> 
>>> ##### **Appending NumPy Array** _(requires intermediate conversion)_
>>> 
>>> ```python
>>> import numpy as np
>>> 
>>> # NumPy array to DataFrame
>>> np_array = np.array([[13, 14]])
>>> df_np = pd.DataFrame(np_array, columns=['A', 'B'])
>>> 
>>> # Append converted NumPy array
>>> df_combined = df1.append(df_np, ignore_index=True)
>>> print(df_combined)
>>> ```
>>> 
>>> ### **Additional Examples**
>>> 
>>> ```python
>>> import numpy as np
>>> 
>>> data = {'a': [33, 43, 23, 59],
>>> 'b': [35, 54, 12, 54],
>>> 'c': [33, 32, 12, 32]}
>>> 
>>> df = pd.DataFrame(data)
>>> 
>>> df2 = df.append({'a': 3}, ignore_index = True)
>>> 
>>> s = pd.Series([1, 2, 3], index = df.columns)
>>> df3 = df.append(s, ignore_index = True)
>>> ```
>> 
>> ---
>> 
>>> #### **`concat()`:**
>>> 
>>> ##### **Basic Usage (DataFrame to DataFrame)**
>>> 
>>> ```python
>>> import pandas as pd
>>> 
>>> # Initial DataFrames
>>> df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
>>> df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})
>>> 
>>> # Concatenate df1 and df2
>>> df_combined = pd.concat([df1, df2], ignore_index=True)
>>> print(df_combined)
>>> ```
>>> 
>>> ##### **Concatenating Dictionary (convert to DataFrame first)**
>>> 
>>> ```python
>>> # Dictionary to DataFrame first
>>> new_row = pd.DataFrame([{'A': 9, 'B': 10}])
>>> 
>>> # Concatenate
>>> result = pd.concat([df1, new_row], ignore_index=True)
>>> print(result)
>>> ```
>>> 
>>> ##### **Concatenating Series** _(Series as DataFrame first)_
>>> 
>>> ```python
>>> # Series to DataFrame first
>>> series_row = pd.Series({'A': 11, 'B': 12}).to_frame().T
>>> 
>>> # Concatenate
>>> result = pd.concat([df1, series_row], ignore_index=True)
>>> print(result)
>>> ```
>>> 
>>> ##### **Concatenating NumPy Array** _(direct conversion)_
>>> 
>>> ```python
>>> import numpy as np
>>> 
>>> # NumPy array
>>> np_array = np.array([[13, 14]])
>>> 
>>> # Convert to DataFrame first
>>> np_df = pd.DataFrame(np_array, columns=['A', 'B'])
>>> 
>>> # Concatenate
>>> result = pd.concat([df1, np_df], ignore_index=True)
>>> print(result)
>>> ```
>>> 
>>> ##### **Concatenating Using Axis**
>>> 
>>> ```python
>>> import pandas as pd
>>> 
>>> # --- Sample DataFrames ---
>>> df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]}, index=['row1', 'row2'])
>>> df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]}, index=['row3', 'row4'])
>>> df3 = pd.DataFrame({'C': [9, 10]}, index=['row1', 'row2'])
>>> 
>>> # -----------------------------------
>>> # 1. Concatenation along rows (axis=0) — default
>>> # -----------------------------------
>>> # Stacks DataFrames vertically (like append)
>>> df_row_concat = pd.concat([df1, df2], axis=0)
>>> print("Concatenated along rows (axis=0):")
>>> print(df_row_concat)
>>> 
>>> # -----------------------------------
>>> # 2. Concatenation along columns (axis=1)
>>> # -----------------------------------
>>> # Stacks DataFrames side-by-side
>>> df_col_concat = pd.concat([df1, df3], axis=1)
>>> print("\nConcatenated along columns (axis=1):")
>>> print(df_col_concat)
>>> 
>>> # -----------------------------------
>>> # 3. With ignore_index=True (only for axis=0)
>>> # -----------------------------------
>>> # Resets the index in row-wise concat
>>> df_row_reset_index = pd.concat([df1, df2], axis=0, ignore_index=True)
>>> print("\nRow-wise concat with ignore_index=True:")
>>> print(df_row_reset_index)
>>> 
>>> # -----------------------------------
>>> # 4. Handling mismatched columns (axis=0)
>>> # -----------------------------------
>>> # If columns don't match, pandas will fill with NaN
>>> df_mismatch = pd.DataFrame({'C': [100, 200]}, index=['row5', 'row6'])
>>> df_mismatch_concat = pd.concat([df1, df_mismatch], axis=0)
>>> print("\nRow-wise concat with mismatched columns:")
>>> print(df_mismatch_concat)
>>> 
>>> # -----------------------------------
>>> # 5. Concatenating Series along columns
>>> # -----------------------------------
>>> # Create two Series with the same index
>>> s1 = pd.Series([10, 20], index=['row1', 'row2'])
>>> s2 = pd.Series([30, 40], index=['row1', 'row2'])
>>> 
>>> # Concatenate as columns (side-by-side)
>>> series_concat = pd.concat([s1, s2], axis=1)
>>> series_concat.columns = ['S1', 'S2']  # Set column names
>>> print("\nSeries concatenated along columns:")
>>> print(series_concat)
>>> 
>>> # -----------------------------------
>>> # 6. Concatenating with keys (to create MultiIndex)
>>> # -----------------------------------
>>> df_multi = pd.concat([df1, df2], axis=0, keys=['First', 'Second'])
>>> print("\nRow-wise concat with hierarchical keys:")
>>> print(df_multi)
>>> ```
>>> 
>>> ###### 🧠 Explanation of `axis`:
>>> 
>>> - `axis=0` (default): concatenate **rows** → stack vertically.
>>> - `axis=1`: concatenate **columns** → align rows by index and merge side-by-side.

---

## 7.9 Summary <a name="79-summary"></a>

By completing this chapter, you should now:

* Understand how Python libraries are distributed and installed
* Be able to install and verify required libraries locally
* Appreciate the importance of version pinning
* Understand why `append()` is deprecated and what to use instead

You are now ready to proceed to **NumPy**, which forms the foundation for pandas and most numerical data processing in Python.