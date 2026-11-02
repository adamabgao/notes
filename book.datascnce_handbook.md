
---
# Format Guidline

**signposting**
```html
<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;"> YOUR TEXT HERE </div>
```

**caution**
```html
<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : YOUR TEXT HERE </div>
```

**tables**
Substitute markdown tables for itemized bullet point definitions. 

**sub-example**
```
↳ ex. YOUR TEXT HERE
```

---
# Contents
[[#I Jupyter]]

---
# I: Jupyter 

---
## 1: IPython and Jupyter

**introspection command ?**
We can use `?` to return the docstring for a given object i.e. `len?` (returns similar result as `help()` in python). Using `??` will return the sourcecode. 

**auto-completion functions**
Jupyter labs does not use the mypy static checker. There is no auto-autocomplete but `<TAB>` toggles relevant attributes for objects. `ipy` has a built-in toggle search that searches for partial name matches using `**?` i.e. `random.*rand*?` returns all the functions in `random` standard library module containing `rand` in their name.

---
## 2: Enhanced Interactive Features
**%run**
Runs any `py` or `ipy` file by pathname, either relative or absolute (absolute pathname must be stated in `str`)

**%magic**
Returns all the `ipy` magic commands (`%lsmagic` for complete magic commands list). 

 **inputs and outputs**
All inputs and outputs in jupyter labs are serialized as consecutive cell executions. In other words, there is no line-by-line index across cells. Each consecutive cell input, regardless of its output (including error) is serialized (each cell has a singular index of one). The cell index is listed left to the cell box. If you delete a cell, the index will appear out of order (restarting the kernel will re-order the execution according to a fresh input/output cache). 
 
**statement: In**
Returns all previous (exhaustive) cell inputs as a list.  `In[x]` will return an input indexed to its execution i.e. `In[3]` returns the third cell input. Note, `In` returns cell inputs from the cell's perspective; the information is raw cellular input. 

**statement: Out**
Returns mapping (dictionary) of `{In : Out}` history. `Out` is only populated with inputs that return values. Statements, assignments, imports, (`print`) are not cached in `Out`. Like `In`, `Out[x]` return the `In:Out` pair according to its index.  Note, `Out` returns cell inputs-output pairs from the cell's perspective; the information is raw cellular output.

**statement: \_**
The last three outputs are isolated in a cache accessible via the underscore`_`, `__`, `___` respectively i.e. `print(_)` . Any `Out[X]` can also be recalled as `_X` i.e. `Out[3] == _3`

**statement: \;**
You can suppress output using semicolon, `;`, at the end of an expression i.e. `math.sin(2) + math.sin(0);`. This will also hide the output from the `Out` cache and index. Note,  `Out` eliminates all output from the users view even though the output itself is computed and assigned i.e. `z = 20 +40;` , `print(z) == 60`. 
 
**%hist**
Returns an  exhaustive command history $\rightarrow$ the command history differs from `In` in that its read-only, whereas `In` is an indexable list (however both show input history). Note, `%hist` can also be called as `history`. 

**%rerun**
Re-executes part of the command history. 
 
**%save**
Saves some set of the command history

**basic shell scripting**
Shows some useful unix shell commands. 

| command | description                                                |
| ------- | ---------------------------------------------------------- |
| `echo`  | prints text; `echo cat` produces `cat`                     |
| `pwd`   | prints the path of the current working directory           |
| `ls`    | lists directory contents                                   |
| `cd`    | changes the current working directory                      |
| `mkdir` | creates a new directory                                    |
| `mv`    | moves a file or directory; `mv file.txt path/to/directory` |
| `mv`    | renames a file or directory; `mv oldname.txt newname.txt`  |

**running shell scripts in from ipy**
Use `!` to run shell commands directly from a notebook i.e.`!pwd`. Shell outputs can be saved and vice-versa i.e. `directory = !pwd` or `contents = !ls`.

**shell-related magic commands**
Some shell commands do not work because shell commands run from an `ipy` notebook are processed in a temporary sub-shell that does not maintain state from command to command i.e. `!cd` does not work. To solve this you can use shell magic commands or auto-magic commands. These are just a subset of shell commands that use `%` instead of `!`: `%cat`, `%cp`, `%env`, `%ls`, `%man`, `%mkdir`, `%more`,`%mv`, `%pwd`, `%rm`, `%rmdir` (all of which can even be stated without the `%` qualifier i.e. `cat`, `env`).

---
## 3: Debugging and Profiling
**%xmode**
Magic command has three settings to control the amount of output returned ; `Context`, `Plain`, `Verbose` (`Context` is the default setting). 

**%debug**
opens an interactive ipy console running the python debugger, `pdb`, using the last python exception object (error) and debugger, which allows you to inspect variables at different frames in the script execution. Below are some useful `pdb` commands. 

| command    | description                                                                                      |
| ---------- | ------------------------------------------------------------------------------------------------ |
| `d` (down) | moves to a newer execution frame, closer to the current execution point                          |
| `u` (up)   | moves to an older execution frame, closer to the caller                                          |
| `p`        | evaluates and prints an expression in the selected frame; if `a = 5`, then `p a + 5` prints `10` |
| `help`     | lists available `pdb` commands                                                                   |
| `quit`     | exits the debugger and aborts the program being debugged                                         |

**profiling: time profiling code execution**

| command                | description                                                                                                      |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `%time` / `%%time`     | Reports wall time and CPU time for a single execution of code                                                    |
| `%timeit` / `%%timeit` | Reports mean execution time and standard deviation across repeated timing runs                                   |
| `%prun` / `%%prun`     | Reports a table of function call counts, time spent within each function, and cumulative time including subcalls |

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Multiline ipy magic denoted <code>%%</code> must appear at the top of a cell (as it modifies the cell itself), and is scoped to an entire cell. You cannot stack multiline magic functions.</div>

---
# II: Numpy

---
## 4: Data Types in Python
**py datatypes**
The core types in python like `int`, `float`, and `str` are `C` implemented bundles containing various metadata that define their behaviour in python. None of the built-in py types have a `__dict__` (or `__slots__`).

All python objects (including python-defined objects) have a have a `PyObject_HEAD` object distilled in `C`, which holds two items: a memory address stored as an internal reference count and a type pointer, `PyTypeObject`, which tells python what its data type. Depending on the type, varying additional metadata are stored i.e. `int` holds `PyObject_HEAD`,  `ob_size`, and `ob_digit`. 

**object.list**
`list` is a heterogenous 'object container', containing pointers to a block of pointers, which in turn, point to individual objects. While this allows for mixed-data storage and flexibility, it is inefficient for complex computations (see, $O(n)$, $O(1)$ complexity). 

**numpy.array**
A numpy `nd.array` is a homogeneous, fixed-type, mutable array of elements. The default `dtype` depends on your system (usually `float64` for modern systems) but can be set explicitly with `dtype=`  (check using `np.array.dtype`). 

For non-object `dtypes`, elements are stored as `C` values in a contiguous memory buffer (not Python). The array object holds a pointer to this data buffer. The data itself represents actual `dtype` values. Because of this, `numpy.ndarray` is more efficient for operational computing. 

---
## 5: Numpy Basics

**numpy.ndarray**
Base class; do ring discourages calling `ndarray` (actual class); recommends using factory functions instead i.e. `np.array`, `np.zeros`, `np.ones`, etc.

Some array attributes are included below. The following are `ndarray` attributes fixed in C language. 

| attribute  | description                              |
| ---------- | ---------------------------------------- |
| `ndim`     | number of dimensions                     |
| `shape`    | tuple of axis lengths                    |
| `size`     | total number of elements (∏ shape)       |
| `strides`  | bytes to step in each dimension          |
| `data`     | buffer / pointer-like view of raw memory |
| `itemsize` | bytes per element                        |
| `nbytes`   | total bytes consumed by elements         |
| `dtype`    | data type descriptor (`np.dtype`)        |
| `flags`    | memory/layout flags object               |

**indexing numpy arrays**
Numpy indexing handles just like list-based python indexing. It is a zero-based index system (values start at zero)
 
**multidimensional indexing**
All n-dimensional arrays can be indexed using zero-based tuple indexing i.e. if `ndim=3` (dimensions = 3) for `x`, then `x[a,b,c]`, where `a`, `b`, `c` are commensurate `dtype` values. 

the `size` of an array is given by:
$$
\text{size} = \prod_{i=1}^{n} d_i
$$
where, 
$$
 \text{size} = d_1 \times d_2 \times \cdots \times d_n
 $$

for an `np.ndarray` of size `i`, the length of any axis is equal to `n-1` in zero-based indexing
$$
i \in \{0, 1, \dots, n - 1\}
$$
$$
i_{\max} = n - 1
$$
⚠️ : A `[3, 4, 5]`, shaped `np` array,  is indexable up to `[2, 3, 4]` and has `size` 3 x 4 x 5 = `60`.


<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">The rest of this chapter includes important functions and methods </div>

**numpy.array**
Primarily designed as conversion / coercion (casting) constructor (you are meant to pass objects to it as opposed to generating new objects $\rightarrow$ prioritizes semantic conversion). 

If you pass an existing `np.ndarray` to `np.array()` with `copy=False` and compatible `dtype` and `order`, it can return the original array itself rather than a separate view. You can use `np.shares_memory(a, b)` to check whether two arrays share memory, although this does not distinguish an array from a separate view, and views need not start at the same memory address. To convert an array-like object to an `np.ndarray` while avoiding a copy when possible, use `np.asarray()`; to explicitly create a view of an existing array, use `a.view()`.


```python 
#signature
numpy.array(object, dtype=None, *, copy=True, order='K', subok=False, ndmin=0, ndmax=0, like=None)
```

- `copy=False` : returns a view object

**numpy.asarray**
Input data, in any form that can be converted to an array. This includes lists, lists of tuples, tuples, tuples of tuples, tuples of lists and `ndarrays`. 

Use `np.array()` to create an `np.ndarray` with a copy by default, and use `np.asarray()` to convert array-like input to an `np.ndarray`, copying only when necessary. Using `np.asarray()` says, “I accept array-like input and do not require a new data buffer,” while using `np.array()` with its default settings says, “I want a fresh NumPy array with its own data buffer.”
```python
#signature
numpy.asarray(a, dtype=None, order=None, *, device=None, copy=None, like=None)
```

**numpy.reshape, ndarray.reshape**
Used to reshape any array to dimensions given i.e. `reshape([x, y, ...]` (out-of-place). Any dimension set to `-1` means: infer this dimension automatically based on the total number of elements and the other dimensions you specified
```python
#signature
numpy.reshape(a, /, shape, order='C', *, copy=None)
```

```python
#signature
ndarray.reshape(_shape_, _/_, _*_, _order='C'_, _copy=None) a.reshape(*shape_, _order='C'_, _copy=None_)
```

**np.unique**
Returns all the unique values in an array
```python
#example
np.unique(ar, return_index=False, return_inverse=False, return_counts=False, axis=None, *, equal_nan=True, sorted=True)
```

- `return_counts` : returns the counts of the unique values

**numpy.zeros**  
Return a new array setting values to zero.
```python
#signature
numpy.zeros(shape, dtype=None, order='C', *, device=None, like=None)
```

**numpy.ones**  
Return a new array of given shape and type, filled with ones.
```python
#signature
numpy.ones(shape, dtype=None, order='C', *, device=None, like=None)
```

**numpy.empty**  
Return a new array of given shape and type, without initializing entries.
```python
#signature
numpy.empty(shape, dtype=None, order='C', *, device=None, like=None)
```

**numpy.full**  
Return a new array of given shape and type, filled with fill_value.
```python
#signature
numpy.full(shape, fill_value, dtype=None, order='C', *, device=None, like=None)
```

**numpy.linspace**  
Returns evenly spaced numbers over the specified `num=x` interval. The `retstep` parameter returns the computed per-interval step between values.
```python
#signature
numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None, axis=0, *, device=None)
```

**numpy.concatenate**  
Joins a sequence of arrays along an existing axis. General-purpose join function. The `axis` parameter specifies the existing dimension to join along, using zero-based indexing. For `ndim=2`, `axis=0` joins vertically/down rows, while `axis=1` joins horizontally/across columns. Out-of-place unless `out` is explicitly provided.
```python
#signature
numpy.concatenate(arrays, /, axis=0, out=None, *, dtype=None, casting='same_kind')
```

```python
#example
x = [1, 2, 3]
y = [4, 5, 6]
np.concatenate([x, y])
```

```python
>>> array([1, 2, 3, 4, 5, 6])
```

**numpy.stack**  
Joins a sequence of arrays along a new axis. Unlike `concatenate`, `stack` increases the number of dimensions by one. Intuition: `concatenate` glues arrays together; `stack` packs arrays into a new dimension.
```python
#signature
numpy.stack(arrays, axis=0, out=None, *, dtype=None, casting='same_kind')
```

**numpy.vstack**  
Stacks arrays vertically, row-wise. Equivalent to concatenation along `axis=0` after arrays are at least 2D. For multiple same-length 1D arrays, each input becomes a row. Basic row stack shape: `(number_of_arrays, n)`.
```python
#signature
numpy.vstack(tup, *, dtype=None, casting='same_kind')
```

**numpy.hstack**  
Stacks arrays horizontally, column-wise. Equivalent to concatenation along `axis=1`, except for 1D arrays, where it concatenates along `axis=0`. Rebuilds arrays divided by `hsplit`. Out-of-place.
```python
#signature
numpy.hstack(tup, *, dtype=None, casting='same_kind')
```

**numpy.c_, numpy.column_stack**  
Places inputs beside each other as columns. Intuition: column builder. For same-length 1D arrays, each input becomes a column. Basic column stack shape: `(n, number_of_arrays)`. For 2D arrays, behaves like horizontal concatenation along `axis=1`. The `np.c_` object is index-trick shorthand $\rightarrow$ `np.column_stack` is the regular function form.
```python
#signature
numpy.c_[a, b]
np.column_stack([a, b])
```

**numpy.r_**  
Concatenates along `axis=0`. Intuition: row builder. For 1D arrays, joins end-to-end. For 2D arrays, stacks arrays vertically/down rows. The `np.r_` object is index-trick shorthand and also supports slice syntax such as `np.r_[1:5]`.
```python
#signature
numpy.r_ = <numpy.lib._index_tricks_impl.RClass object>
```

**numpy.split**  
Splits an array into multiple sub-arrays as views into `ary`. The `axis` parameter represents dimensions using zero-based indexing. For an array of `ndim=2` (two dimensions), `axis=0` splits along rows, while `axis=1` splits along columns. Check memory when sharing matters. Concatenation creates a new array unless an `out` array is explicitly provided.
```python
#signature
numpy.split(ary, indices_or_sections, axis=0)
```

**numpy.hsplit**  
Splits an array into multiple sub-arrays horizontally (column-wise). Out-of-place.
```python
#signature
numpy.hsplit(ary, indices_or_sections)
```

**numpy.vsplit**  
Splits an array into multiple sub-arrays vertically (row-wise). Out-of-place.
```python
#signature
numpy.vsplit(ary, indices_or_sections)
```

**numpy.ravel**  
Returns `a` as a 1D flattened array. Tries to return a view, but may copy if needed. Changes to the result may affect the original if memory is shared.
```python
#signature
numpy.ravel(a, order='C')
```

**numpy.ndarray.ravel**  
Method version of `np.ravel()`, called as `arr.ravel()`. Returns a 1D flattened array. Tries to avoid copying, with the same general behavior as `np.ravel(arr)`.
```python
#signature
numpy.ndarray.ravel(order='C')
```

**numpy.ndarray.flatten**  
Returns a 1D flattened copy, independent from the original array. Safer when you do not want changes to affect the original.
```python
#signature
numpy.ndarray.flatten(order='C')
```

**ndarray.flat**  
Attribute that returns a 1D iterator, not a new `ndarray`. Common use: `for ax in axes.flat:`. Best for looping/indexing through an array as if it were 1D.
```python
#signature
ndarray.flat
```

---
## 6: Computation and Ufuncs
**ufuncs**
While basic python is versatile, computational performance is hindered by typing checks and function dispatches. Numpy improves computation performance with universal functions (ufuncs).
 
Ufuncs are primitive numeric operations; memory-optimized pre-compiled `C` kernels that operate on each element of a whole array at once. The expression, `a + b`, where `a` and `b` are numpy objects dispatch to the ufunc, `np.add`.  Ufuncs are unary or binary on their input. 

While traditional loops operate on each element individually, ufuncs use a pre-compiled C struct to apply operations to an array at once. 

**ufunc wrappers & overloads**
Many functions and methods in the numpy library implement ufuncs implicitly. Ufunc wrappers are signed as `_ufunc 'matmul'_` in their signature in the numpy docs.  

Many python operators are numpy-overloaded ufunc wrappers allowing for fast computation between numpy objects (all supported operator types include, arithmetic, comparison, and bitwise). 

**ufunc methods**
Ufuncs, themselves, hold (optional) methods on them, `ufunc.method` because their fundamental operation is a modifiable vector operation (vectorized operations perform the same action on multiple data elements). In other words, the application method itself, can be modified. 

**numpy.ufunc.reduce**  
Applies a ufunc along one axis, reducing the number of dimensions unless `keepdims=True`. Called on a specific ufunc, such as `np.add.reduce()` or `np.subtract.reduce()`.
```python
#signature
numpy.ufunc.reduce(array, axis=0, dtype=None, out=None, keepdims=False, initial=<no value>, where=True)
```

**numpy.ufunc.accumulate**  
Accumulates the results of applying the operator to successive elements along an axis.
```python
#signature
numpy.ufunc.accumulate(array, axis=0, dtype=None, out=None)
```

A one-dimensional example is given below. For a two-dimensional example $\rightarrow$ accumulate along axis zero rows, down columns.
```python
#example
i = np.arange(6)
np.add.accumulate(i)
```

```python
>>> array([0, 1, 3, 6, 10, 15])
```


<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : You can directly write the result of a ufunc into an array by specifying the array under <code>out=x</code> where <code>x</code> can be an existing or a new array i.e. <code>y[::2]</code>.  </div>

---
## 7: Aggregations

**numpy arrays are algebraic**
The numpy array is a 'fixed-dimensional' data structure and such, the length of each dimension may vary (i.e.`(2, 3, 6, 2)`). Each axis is fixed and of equal length as to facilitate linear constancy (an array without fixed dimensions could be `(2, 3, (4, 5), (4, 3)`, where each dimension itself, holds multiple dimensions resembling a tree-like structure i.e. `JSON` files). 

**n-dimensional indexing**
Indexing n dimensions is conceptually difficult, however there are some tricks. For arrays in $\mathbb{R}^4$, an array index can be understood as: 
$$\boxed{\text{1.block(s)}}\rightarrow\boxed{\text{2.matrix(es)}}\rightarrow\boxed{\text{3.row(s)}}\rightarrow\boxed{\text{4.columns(s)}}$$
Where 3 and 4 form the actual array index of `dtype` values `[row, col]`. An array of n dimensions can be understood conceptually as two information types $\rightarrow$ 1. index content 2. data content.

For arrays beyond $\mathbb{R}^3$, $\mathbb{R}^4$, all indexes beyond the last two forming the actual array of values, are index content, `[..., row, col]`. 

**aggregation & dimensions**
The basic 'aggregation functions' are actually python-level convenience wrappers around `py.ufunc.reduce()`. Hence, these functions reduce array dimensionality by one, through aggregations.

The `axis` param determines the axis along which a dimension is collapsed. Ex. for a two-dimensional array with `shape=(2, 2)`,  `axis=0` collapses dimensions row-wise, returning `([a, b])`. 

Consider, the value you set in `axis` is the dimension you eliminate. Therefore, `axis=0`, `axis=1`, eliminate rows (`0`) and columns (`1)` in a two-dimensional array, respectively. The rule is for `axis=n`, `n` is deleted such that for shape `shape=(2,3,4,5)`, `axis=0` returns a new array with shape `(3,4,5)`.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : The basic aggregation function are listed in Table 3. </div>

---
## 8: Broadcasting
**broadcasting**
Is the numpy protocol for resolving dimensionality differences between arrays in computation.

**broadcasting rules**
1. If the two arrays differ in their number of dimensions, the shape of the one with fewer dimensions is padded with ones on its leading (left) side. 
   
   ↳ex. the dimension is padded $\Rightarrow$ $\text{shape} = (3,) \rightarrow (1, 3)$.


1. If the shape of the two arrays does not match in any dimension, the array with shape equal to 1 in that dimension is stretched to match the other shape.
   
   ↳ex. a dimension is stretched only if it contains an axis length $1$ in any dimension $\Rightarrow$ $\text{shape}=(1, 3)\rightarrow(4,3)$.

1. If in any dimension the sizes disagree and neither is equal to $1$, an error is raised.
   ↳ ex. $\text{shape}=(3,2)$ and $\text{shape} = (4,5)$ are incompatible because there are no ones, padded or otherwise, to upscale. 

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : We cannot broadcast arrays of different shapes if any of the axis lengths are not equal, or not equal to <code>1</code>. </div>

**broadcasting example**
In practice, the rules are designed to work together. For the arrays `a.shape=(3,)` and `b.shape(4, 3)`, the first rule pads `a` with `1` on its leading left side, such that `(3,)` becomes `(1, 3)`. The second rule scales the `1` to match `b`; `(1,3)`$\rightarrow$`(4,3)`.
 
## 9: Comparisons and Masking
**comparison**
Are relational measurements between quantities. The comparison operators are ufuncs, which produce boolean values. Booleans take bit (and integer) values `0`, `1` for `False`, and `True` respectively (`numpy.sum` to count `True` values).

↳ ex : `x=np.arange(10)`
- `x > 6` $\rightarrow$ `(array([False, False, False, False, False, False,  True,  True,  True, True]),`
- `x <= 3` $\rightarrow$ `array([ True,  True,  True,  True, False, False, False, False, False,False]))`
- `(2*j) == (1.5*j)` $\rightarrow$ ` array([ True, False, False, False, False, False, False, False, False,False]))`

↳ advanced ex. : `x2=np.arange(10).reshape((2, 5))`
- `np.sum(x2>5, axis=1)`,  $\rightarrow$ `array([0, 4])`; the sum is computed on `x2>5`, `axis=1` (summed `True` values; `6`, `7`, `8`, `9` $\star$, `True` ~ `1`)
- `np.mean(x2<=4, axis = 0)`  $\rightarrow$  `array([0.5, 0.5, 0.5, 0.5, 0.5])`; the mean is computed on `x2<=4`, `axis=0` (averaged `True/False` values; `0`, `1`, `2`, `3`, `4`)

↳ other ex. : `a=rng2.integers(1, 21, [3, 7])`, `b=rng2.integers(1, 21, [7,])`
- `a+b` $\rightarrow$ `b` is broadcast to `[1, 7]` $\rightarrow$ `[3, 7]`

↳ other ex. : `a=rng2.integers(1, 21, [3, 7])`, `b=rng2.integers(1, 21, [6,])`
- `a+b` $\rightarrow$ `ValueError:operands could not be broadcast together` 

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Comparison operators produce booleans. </div>

**bitwise (boolean) operators**
The boolean operators `and`, `not`, and `or` are python logical operators and do not work with numpy arrays; `ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all()`. 

The bitwise operators, bitwise AND (`&`), bitwise OR (`|`), bitwise NOT (`~`), bitwise XOR (`^`), are element-wise boolean operators. Note, these function element-wise, per value. 

The bitwise operators work on anything representable in bits ( `str`, `int`, etc.). Consider the binary representation of an `int`; `0`s and `1`s are `False` and `True` values, that when zipped with another integer can perform a bitwise operation i.e. `101 & 011` (`5 & 3`), zipped are `1 & 0`, `0 & 1`, `1 & 1`, resulting in `001` or `1`. See [[#Scalar logic]] to understand how `True` and `False` values resolve with different operators. 

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : The python boolean operators do not work element-wise on arrays. </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : See Scalar logic in the Appendix. </div>

**bitwise operations on arrays**
A bitwise operation between arrays is computed on the array values themselves. The statement `(a > 5) & (b > 5)`, where `a` and `b` are arrays of different sizes, evaluates each comparison first and then the `&` operator between the two arrays, producing an array of booleans. 

The statement `a & b` computes the bitwise operator of each array's elements such that the array `([2, 4, 6])`, and  `([3, 5, 7])` yield `([2 & 3], [4 &5],[6 & 7])`, where each pair is evaluated. 

**numpy.any**  
Returns whether any of the items in the array are `True`.
```python
#signature
numpy.any(a, axis=None, out=None, keepdims=<no value>, *, where=<no value>)
```

```python
#example
x = np.arange(5)
(x > 10).any()
```
```python
>>> False
```

**numpy.all**  
Returns whether all of the items in the array are `True`.
```python
#signature
numpy.all(a, axis=None, out=None, keepdims=<no value>, *, where=<no value>)
```

```python
#example
x = np.arange(10)
(x < 10).all()
```
```python
>>> True
```

**masks (boolean. indexing)**
Masks are comparisons used as indexes to select data subsets on the result of the comparison i.e. `a[a>5]`. Boolean indexing selects elements where the mask is `True`. `False` values are omitted. Some other examples, `summer = (days > 172) & (days < 262)`, `rainy = rainfall_mm[rainfall_mm > 0]`. 

---
## 10: Advanced Indexing
**square bracket \[]**
The `[]` symbol is the index marker. Each level of nesting of `[]` $\rightarrow$ one axis.

**simple Indexes**
Simple indexes are triggered by the below. When possible, basic indexes return`Views`. 

|index type|description|
|---|---|
|`int`|Selects one position along an axis, removing that axis and reducing dimensionality by 1|
|slice (`:`, `a:b`)|Selects a range along an axis, preserving that axis|
|ellipsis (`...`)|Expands to as many full slices (`:`) as needed to cover the remaining axes|
|`None` / `np.newaxis`|Inserts a new axis of length 1, increasing dimensionality by 1|
 
**advanced indexing**
Advanced indexes are triggered by the below. The shape of the data array is the broadcast shape of the index arrays for advanced indexing. 

Because an int array is the trigger for advanced indexing and thus `[0,2,3]` is a list values, whereas `[[0],[2],[3]]` is a list of list of values with shape `(3,1)`. The general rule for indexes is each level of nesting $\rightarrow$ one axis.

| index type                   | description                                                                                                   |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------- |
| list of integers             | Selects positions using advanced indexing; examples include `[a, b, c]`, `[[a], [b], [c]]`, and `[[a, b, c]]` |
| `np.ndarray` of integers     | Selects positions using an integer array; for example, `np.array([0, 1, 2])`                                  |
| boolean list or `np.ndarray` | Selects positions where the mask is `True`; for example, `np.array([True, True, False])`                      |
<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : For advanced indexes, we distinguish between the data array (the array being indexed) and the index array (used to index the data array). </div>

**mixed indexing**
Mixed indexes are triggered by the below. In mixed indexing, the advanced indexes are broadcast first and then the basic indexes modify the advanced index. The data array’s shape is only consulted afterward, to supply the sizes of any remaining simple-indexed axes. 

| index type                    | description                                                                |
| ----------------------------- | -------------------------------------------------------------------------- |
| advanced index + simple index | Combines advanced and basic indexing; for example, `x[[1, 4, 6], None, :]` |

---
## 11: Sorting Arrays

**numpy.sort**  
Returns a sorted copy of an array, requiring memory for the output array. The `axis` parameter selects the sorting axis; no axes are consumed. Use `x.sort()` for the in-place method.
```python
#signature
numpy.sort(a, axis=-1, kind=None, order=None, *, stable=None)
```

**numpy.argsort**  
Returns the indices that would sort an array along the specified axis.
```python
#signature
numpy.argsort(a, axis=-1, kind=None, order=None, *, stable=None)
```

**numpy.partition**  
Returns a partitioned copy with the element that would occupy index `kth` in a fully sorted array placed at that index. For a 1D array, this element equals `np.sort(x)[kth]`. Smaller or equal values appear before it, and larger or equal values appear after it. Neither side is necessarily sorted. Generally more efficient than `np.sort()` because it performs a partition rather than a full sort.
```python
#signature
numpy.partition(a, kth, axis=-1, kind='introselect', order=None)
```

**numpy.argpartition**  
Returns the indices that would partition an array, rather than the partitioned values.
```python
#signature
numpy.argpartition(a, kth, axis=-1, kind='introselect', order=None)
```

---
## 12: Structured Arrays
**structured array**
A structured array is an array that uses a custom data type specified by the `dtype` param. The structured array is preprogrammed with an immutable `size` using one of three casting functions, `np.zeros`, `np.ones`, `np.empty`, each of which take as their first positional argument, `size` (`np.empty` is a performance-critical function, which initializes random values stored in the memory cache).

The `dtype` param takes a `np.dtype` object or an object that can be converted to a `dtype` object, which are the following. 

| type specification                                          | example                                      |
| ----------------------------------------------------------- | -------------------------------------------- |
| Array-scalar types                                          | `np.complex128`                              |
| Built-in Python types                                       | `int`, `str`                                 |
| One-character type codes, optionally prefixed by byte order | `np.dtype('>H')` — big-endian unsigned short |
| String with comma-separated fields                          | `np.dtype("i4, (2,3)f8, f4")`                |
| Type string                                                 | `np.dtype('float64')`                        |

For example, a structured array, `np.zeros((3, ), dtype = [('names', 'u4'), ('age', 'i4')])`, takes form `array([(0, 0), (0, 0), (0, 0)], dtype=[('names', '<u4'), ('age', '<i4')])`. Each tuple, `('name', 'f8')`, is a field descriptor. Once initialized, an array is set according to its `shape` ( `names = ['adam', 'dee', 'fletcher', 'zey']`, or `names = ['adam', 'dee']` cause error as `shape=(3,)`). 

**type strings**
Any string name of a numpy `dtype`, which can be organized into different structures (these are strings which `np.dtype` can read without explicit calls to a constructor):
1. `(flexible_dtype, itemsize)`; i.e. `('U', 10))` (not used, see type strings [numpy.org](https://numpy.org/doc/stable/reference/arrays.dtypes.html#specifying-and-constructing-data-types). 
2. `(fixed_dtype, shape)`; deprecated [numpy.org](https://numpy.org/doc/stable/reference/arrays.dtypes.html#specifying-and-constructing-data-types). 
3. `[(field_name, field_dtype, field_shape), ...]`; "obj should be a list of fields where each field is described by a tuple of length 2 or 3." [numpy.org](https://numpy.org/doc/stable/reference/arrays.dtypes.html#specifying-and-constructing-data-types). This format is parsed by numpy to the metadata structure `{'names', 'formats', 'offsets', 'titles', 'itemsize'}`, which can also be passed explicitly as the `dtype` param (`names` and `formats` are required positionals); i.e. `[('R','u1'), ('G','u1'), ('B','u1'), ('A','u1')]`. 

**numpy.recarray**  
Creates a record array, an `ndarray` subclass that allows field access using attributes (dot notation). Like a structured array, but with attribute access to fields.
```python
#signature
numpy.recarray(shape, dtype=None, buf=None, offset=0, strides=None, formats=None, names=None, titles=None, byteorder=None, aligned=False, order='C')
```

---
# III: Pandas

---
## 13: Pandas Objects
**pandas.Series**  
A one-dimensional labeled array that can hold heterogeneous values.

The `index` must have the same length as the data when provided as an array-like sequence. Index labels must be hashable.

Supports array-style slicing, although indexing behaves differently.

Operations between Series (`+`, `-`, `/`, `*`, `**`) align values by their index labels; the Series need not have the same length. The resulting index is the sorted union of the two indexes.
```python
#signature
pd.Series(data=None, index=None, dtype: 'Dtype | None' = None, name=None, copy: 'bool | None' = None, fastpath: 'bool' = False) -> 'None'
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Lists (<code>list</code>), dictionaries (<code>dict</code>), sets (<code>set</code>), bytearrays (<code>bytearray</code>) are unhashable types because their contents can change over time. </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Hashability of a tuple (<code>tubple</code>) depends on its contents. </div>

**pandas.DataFrame**  
A two-dimensional labeled data structure with columns that can hold different data types $\rightarrow$ the primary pandas object.The `data` parameter accepts dictionary-like containers and other tabular inputs. A common input is a dictionary mapping column names to values.

The `index` supplies row labels, which must be hashable and match the number of rows.The `columns` parameter supplies column labels. For dictionary input, it selects and orders columns by their keys.

Supports attribute access to columns, such as `x.countries`, when the name is a valid Python identifier and does not conflict with an existing attribute. The `DataFrame.dtypes` property returns a Series containing the data type of each column.
```python
#signature
pd.DataFrame(data=None, index: 'Axes | None' = None, columns: 'Axes | None' = None, dtype: 'Dtype | None' = None, copy: 'bool | None' = None) -> 'None'
````

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Attribute dot notation is discouraged as columns may contain forbidden keywords. </div>

At a low level, the DataFrame is a collection of aligned one-dimensional arrays. Row-wise access is generally discouraged as it requires iterating values across multiple columns, which is memory and compute intensive. Frequent row-wise access suggests restructuring the array is preferable. 
```python
#example
pd.DataFrame({'country': g7, 'population': pops})

pd.DataFrame([
    {'country': 'Canada', 'population': 38000000},
    {'country': 'France', 'population': 68000000},
])

pd.DataFrame(
    [('Canada', 38000000), ('France', 68000000)],
    columns=['country', 'population'],
)

pd.DataFrame(
    [[38000000, 68000000, 84000000]],
    columns=['Canada', 'France', 'Germany'],
)
```

---
## 14: Data Indexing and Selection
**subscriptions**
Subscriptions are the square bracket operators `obj[]` used to index pandas objects, which trigger the objects underlying `__getitem__` special method. 'Indexing' is is the common informal term when the key is positional.

**subscription indexing**
Subscription indexing is either explicit or implicit. Explicit indexing uses indexers `.iloc[]` and `.loc[]` for positional and label-indexing, respectively. All subscription formats refer to `df.loc['row_index', 'column_index']`, `df.loc[:, 'col_name']`. Omitting column values yields row integers by default i.e. `df.loc[index]` = `df.loc[index, :]`. Explicit Subscription Indexing `[,]` is Half-Open, while Implicit Indexing `[,]` isFully-Open. For the implicit index,`s[a]`, if `a` is a label or a column key, then `s[a]` is label-based, otherwise `s[a]` is positional. 

**pandas.Index**  
An immutable, array-like sequence used for indexing and alignment. It stores axis labels for pandas objects and is optimized for label lookup. Although an `Index` resembles a NumPy array, it is a distinct object.

A DataFrame’s `.columns` and `.index` attributes contain `Index` objects. Individual labels cannot be changed in place, but the entire index can be replaced by assignment, such as `df.columns = ('cats', 'dogs', 'wolves', 'hamsters')`.

To change selected labels, use `df.rename(columns={'cats': 'kittens'})` or `df.rename(index={'old': 'new'})`. These return a new DataFrame by default; assign the result or use `inplace=True`.

The `data` parameter accepts array-like input. The `dtype` parameter specifies the output data type; if omitted, it is inferred from the data. The `name` parameter sets the name of the index itself, rather than its individual labels.
```python
#signature
pd.Index(data=None, dtype=None, copy=None, name=None, tupleize_cols=True)
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Indexing in pandas is label based; meaning that instead of just operating on two aligned vectors, index rows are only operated on where those index labels have the same values. In other words, pandas finds the union of index labels and computes their values, filling in <code>nan</code> where values are missing. </div>

**reusing a pandas.index**
A pandas Index is an immutable sequence that can be reused by another DataFrame when its rows correspond to the same labels in the same order. Pandas checks only that the lengths match, not that the labels meaningfully correspond to the data. Ensuring that relationship is the user’s responsibility. Assigning an index replaces row labels by position; it does not align or reorder the data.

Rule of thumb: Assign an index when you know the positional relationship is correct. Use alignment-based operations when you need to match data by existing labels.

**native indexes**
The native index attributes for `DataFrame` and `Series` objects respectively are `.index` and `.columns`. 

However,`Series` contains only `.index` as `ndim=1` $\rightarrow$ both `.index` and `.columns` return `Index` objects which are immutable (they can be reassigned but must match length). 

**pandas.DataFrame.rename**  
Renames selected row or column labels using a mapping or function. Use `columns` to rename columns or `index` to rename rows. Alternatively, use `mapper` with `axis='columns'` or `axis='index'`. Set `inplace=True` to modify the existing DataFrame.
```python
#signature
DataFrame.rename(mapper=None, *, index=None, columns=None, axis=None, copy=<no_default>, inplace=False, level=None, errors='ignore')
```

```python
#example
final.rename({'area (sq. mi)': 'area'}, axis='columns', inplace=True)
```

Other index class objects include:

| Index Class                  | Description                                                         |
| ---------------------------- | ------------------------------------------------------------------- |
| `Index`                      | Generic immutable index object used for axis labeling               |
| `RangeIndex`                 | Memory-efficient default integer index (start, stop, step)          |
| `Int64Index`, `Float64Index` | Numeric index types optimized for integer or float labels           |
| `DatetimeIndex`              | Time-aware index supporting slicing, alignment, and resampling      |
| `CategoricalIndex`           | Category-based index optimized for memory usage and faster grouping |
| `MultiIndex`                 | Hierarchical (multi-level) index for structured or grouped data     |

---
## 15: Pandas Operations
**general computation**
General datascience pipelines; computations are performed on a column or an entire array. Row-wise computation is discouraged less performant.

**index alignment**
Computation between Series or DataFrame objects with different shapes yields `np.nan` values . To avoid this, use the explicit function call `DataFrame.add(x, fill_value=…)`, where the `fill_value` parameter sets the value to use for missing data. The `fill_value` is applied only at indices where a `np.nan` would otherwise occur, and the computation is performed between this value and the corresponding non-missing value in the other operand.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Note, <code>np.nan</code> is float-point null representation which upcasts an entire array to <code>float64</code> python values (<code>dtype=object</code>). </div>

**broadcasting vs. alignment**
When does pandas choose to broadcast and when to return null values? If the indexing aligns, (`index` and `columns` are equal) the result will be a broadcasted object if broadcasting rules apply, else a `ValueError`. If the indexing does not align, pandas will return null values.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  The type of the null value depends on the <code>dtype </code>. </div>

---
## 16: Missing Data
There are three different null values each with different context. The default structure of a 

**np.nan**
"Not a numbers" is a special floating point sentinel value (`type=float`), which means "missing or undefined numeric data"- the default numpy null value. Any arithmetic involving `np.nan` usually results in `np.nan`. It does not equal itself i.e `np.nan != np.nan`. The following nan-safe functions/methods apply: 

| function           | description                                                                                        |
| ------------------ | -------------------------------------------------------------------------------------------------- |
| `np.isnan`         | Tests element-wise whether values are NaN                                                          |
| `np.nansum`        | Computes the sum, treating NaNs as zero                                                            |
| `np.nanmin`        | Returns the minimum, ignoring NaNs                                                                 |
| `np.nanmax`        | Returns the maximum, ignoring NaNs                                                                 |
| `np.nanmean`       | Computes the mean, ignoring NaNs                                                                   |
| `np.nanmedian`     | Computes the median, ignoring NaNs                                                                 |
| `np.nanstd`        | Computes the standard deviation, ignoring NaNs                                                     |
| `np.nanvar`        | Computes the variance, ignoring NaNs                                                               |
| `np.nanprod`       | Computes the product, treating NaNs as one                                                         |
| `np.nanargmin`     | Returns the index of the minimum, ignoring NaNs                                                    |
| `np.nanargmax`     | Returns the index of the maximum, ignoring NaNs                                                    |
| `np.nanpercentile` | Computes a percentile (0–100), ignoring NaNs                                                       |
| `np.nanquantile`   | Computes a quantile (0–1), ignoring NaNs                                                           |
| `np.nancumsum`     | Computes cumulative sums, treating NaNs as zero                                                    |
| `np.nancumprod`    | Computes cumulative products, treating NaNs as one                                                 |
| `np.nan_to_num`    | Replaces NaNs with zero and infinities with finite limits by default; supports custom replacements |
| `np.isfinite`      | Tests element-wise whether values are finite (neither NaN nor infinity)                            |
| `np.isinf`         | Tests element-wise whether values are positive or negative infinity                                |

**numpy array & np.nan**
The numpy `np.ndarray` has no nullable `dtypes`; representing missing numeric data uses `np.nan`, which forces integer arrays to upcast to a float `dtype` (often `float64`). In a mixed setting i.e. `np.array([1, None, np.nan])`, the array will default to `dtype=object`, which upcasts the array values to python built-in types, which comprises performance. 

**pandas series & np.nan**
If an operation produces a new column of missing values, they default to `np.nan`. If you don’t explicitly choose a pandas nullable `dtype` (or don’t already have one), pandas arithmetic/alignment typically produces `NaN`, not `<NA>`.  

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  The <code>DataFrames</code> is a heterogenous columns-wise mapping of <code>Series</code>, which may cast <code>np.nan</code> and <code>pd.NA</code> values in one <code>DataFrame</code> if a computation results in an empty column (the default for an empty column of missing values is <code>np.nan</code>; <code>float64</code>). </div>

**pd.NA**
The pandas nullable, `pd.NA`, represents most of the pandas `dtype` string aliases. Because `np.ndarray` upcasts to `float` through `np.nan`, the pandas nullable was created to support non-float values. Unless explicitly invoked with a string alias, (i.e.`dtype='Int32'` ; denoted with capital letter) `pd.NA` is rarely produced unless through a function or method that upcasts an to a pandas nullable (i.e. `pd.array`). 

**numpy.isnan**  
Returns a boolean array indicating which elements are NaN, or a boolean scalar for scalar input.
```python
#signature
np.isnan(x, /, out=None, *, where=True, casting='same_kind', order='K', dtype=None, subok=True)
```

**pandas.isnull**  
Identifies missing values, including `np.nan`, `None`, `pd.NA`, and `NaT`. Returns a boolean scalar or an array-like result matching the input.
```python
#signature
pd.isnull(obj)
```

**pandas.notnull**  
Returns the opposite of `pd.isnull`, identifying values that are not missing.
```python
#signature
pd.notnull(obj)
```

**pandas.Series.dropna**  
Drops missing values from a Series. The `axis` parameter has no effect; it exists for consistency with the DataFrame API.
```python
#signature
pd.Series.dropna(*, axis=0, inplace=False, how=None, ignore_index=False)
```

**pandas.DataFrame.dropna**  
Drops rows or columns containing missing values. The `axis` parameter determines whether rows (`axis=0`) or columns (`axis=1`) are dropped. The `thresh` parameter specifies the minimum number of non-null values required to retain each row or column.
```python
#signature
pd.DataFrame.dropna(*, axis=0, how=<no_default>, thresh=<no_default>, subset=None, inplace=False, ignore_index=False)
```

**pandas.Series.fillna**  
Fills missing values. The `value` parameter specifies the replacement value.
```python
#signature
pd.Series.fillna(value=None, *, method=None, axis=None, inplace=False, limit=None, downcast=<no_default>)
```

**masking with .isnull()**
The `.isnull()` function returns a boolean object with the same shape as the `DataFrame`. When this mask is applied directly, pandas performs elementwise masking: values where the mask is `False` are replaced with `NaN`, and only positions where the mask is `True` are retained. Because missing values are typically sparse, this often results in an output that appears entirely `NaN`.

---
## 17: Hierarchical Indexing
Hierarchical indexing is a form of indexing and data labelling using hierarchical table levels. A `Series`, or `DataFrame` have fixed structural dimensionality (1D, 2D respectively). While hierarchical indexing does not change the physical dimensionality of the array, it allows the label dimensionality to scale indefinitely. 

**pandas.MultiIndex**
A multi-level, or hierarchical, index object for pandas objects. Attributes include `names` (names of the index levels), `levels` (unique labels for each level), `codes` (integer indexers mapping entries to level labels), `nlevels` (number of levels), and `levshape` (tuple representing the length of each level). The `set_levels` method sets new levels on a MultiIndex. See [pandas.MultiIndex](https://pandas.pydata.org/docs/reference/api/pandas.MultiIndex.html) for more.

The `pd.MultiIndex` object allows you to create a hierarchical index from a few functions:
1. `pd.MultiIndex.from_arrays(arrays[, sortorder, names])`
Convert arrays to MultiIndex.
2. `pd.MultiIndex.from_tuples(tuples[, sortorder, names])`
Convert list of tuples to MultiIndex.
3. `pd.MultiIndex.from_product(iterables[, sortorder, names])`
 Make a MultiIndex from the cartesian product of multiple iterables.
4. `from_frame(df[, sortorder, names])`
 Make a MultiIndex from a DataFrame. The `pd.DataFrame.values()` will constitute the new MultiIndex
```python
#signature
pandas.MultiIndex(levels=None, codes=None, sortorder=None, names=None, copy=False, name=None, verify_integrity=True)
```


**indexing a MultiIndex**
Indexing MultiIndex array is signified with tuples for rows and columns (multiIndex arrays require the explicit indexers `loc`, `iloc`):

- `.loc[(row_indexer),(column_indexer)]`

For each indexer with n label dimensions (levels) you have:
 
- `.loc[(row_lv1,row_lv2,...,row_lvN),(col_lv1,col_lv2,..., col_levelM)]` 

**slicing a MultiIndex array**
Use `pd.IndexSlice` (conventionally `idx = pd.IndexSlice`), as an indexer for slicing MultiIndex arrays i.e. `.loc[(idx[2013:2014], 'visit2'), idx['Bob':'Dick']]`. 

**chained indexing**  
An indexing pattern where one selection produces an intermediate object, and a second indexing operation is applied to that result.
```python
#example
merge[merge['state/region'] == 'PR'].loc[:, 'state'] = 'Puerto Rico'
```

The boolean selection creates an intermediate DataFrame, and the assignment targets that object rather than reliably updating the original `merge`.

To update the original DataFrame, combine the row and column selections in a single `.loc[mask, column]` assignment:
```python
#example
merge.loc[merge['state/region'] == 'PR', 'state'] = 'Puerto Rico'
```

This assigns directly to the selected rows and column of `merge`, modifying it in place.

---
## 18: Concat and Append
**pandas.concat**  
Concatenates pandas objects along a specified axis.

The `join` parameter accepts `'inner'` or `'outer'`, determining whether labels on the other axis are intersected or combined.

Setting `ignore_index=True` replaces labels along the concatenation axis with consecutive integers starting at zero. Labels on the other axis still participate in alignment.
```python
#signature
pandas.concat(objs, *, axis=0, join='outer', ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False, sort=<no_default>, copy=<no_default>)
```

---
## 19: Merge and Join
**what is a merge?**
A merge performs a relational join between two DataFrames based on matching keys. The join type determines which keys are retained, and columns from both DataFrames are combined in the result.

1.  inner join: retains only keys present in both DataFrames, discarding unmatched keys $\rightarrow$ for example, `pd.merge(..., how='inner')`.

2. outer join: retains all keys from both DataFrames, including unmatched keys; for example, `pd.merge(..., how='outer')`. Where a key has no match, the missing values are filled with null values such as `NaN`.

**pandas.merge**  
Merges two DataFrames using columns or indexes as join keys.
```python
#signature
pandas.merge(left, right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, sort=False, suffixes=('_x', '_y'), copy=<no_default>, indicator=False, validate=None)
```

- `how` : Specifies the join type, such as `'inner'` (retains matching keys) or `'outer'` (retains all keys).
- `on` : Specifies shared column names to use as merge keys. When merging on columns without specifying keys, pandas uses all shared column names, forming a composite key if there are multiple.
- `left_on` / `right_on` : Specifies the key columns in each DataFrame, allowing their names to differ. Use both for column-to-column joins, or combine one with the other DataFrame’s index flag for a column-to-index join.
- `left_index` / `right_index` : Uses the corresponding DataFrame’s index as the join key when set to `True`. For single-level indexes, the index names do not need to match. The selected join type still applies.

---
## 20: Aggregate and Group

**GroupBy & groupby**
The GroupBy object is a flexible abstraction, containing the unique keys from the key column and the split DataFrames themselves. 

Both objects are accessible, for example $\rightarrow$ `for key, group in df.groupby('keys'): print(key, group)`. Note, `groupby` returns a `GroupBy` object, and subsequent operations are typically chained onto this object (for theory on groupby, see [[#Split-Apply-Combine Strategy]] i the appendix). 

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  The <code>GroupBy</code> object is formally listed under <code>pandas.api.typing.DataFrameGroupBy</code>, and <code>pandas.api.typing.SeriesGroupBy</code>. </div>

**pandas.DataFrame.groupby**  
Returns a `GroupBy` object that represents groups of rows. It is lazily evaluated and iterable, but is not itself an iterator or a Python generator.

Grouping is usually followed by an aggregation function (see Table 8 Pandas Aggregation Methods). Iterating over the object yields pairs of group labels and their corresponding DataFrames.
```python
#signature
DataFrame.groupby(by=None, level=None, *, as_index=True, sort=True, group_keys=True, observed=True, dropna=True)
```

- `by` : Specifies how rows are grouped. Accepts a column label, a list of column labels, or an external array-like object containing one group label per row. A Series is aligned to the DataFrame’s index before grouping. A dictionary maps existing index labels to group labels; for example, `{'A': 'vowel', 'B': 'consonant', 'C': 'consonant'}` groups rows indexed by `B` and `C` together. A function is applied to each index label to determine its group. Multiple grouping specifications can be combined in a list.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Keys missing in multi-index groupbys, (x, ?) will be dropped and values will be filled with <code>nan</code>. </div>

**FAAT**
Filter, apply, aggregate, transform, (FAAT) methods efficiently implement operations on GroupBy objects. 

**DataFrameGroupBy.filter**  
Performs group-wise subsetting. The function receives each full group and must return a single boolean deciding whether the entire group is kept or dropped, such as `len(g) == 2`.
```python
#signature
pandas.core.groupby.DataFrameGroupBy.filter(func, dropna=True, *args, **kwargs)
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Receives DataFrame. </div>

**DataFrameGroupBy.aggregate**  
Performs column-wise group aggregation, producing reduced results per group.

Functions can be specified as a list, such as `['min', 'mean', 'std']`, or as a column-to-function mapping, such as `{'col_name_a': 'mean', 'col_name_b': 'std'}`.
```python
#signature
pandas.core.groupby.DataFrameGroupBy.aggregate(func=None, *args, engine=None, engine_kwargs=None, **kwargs)
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Receives Series. </div>

**DataFrameGroupBy.apply**  
Applies a function to each group and combines the results into a single object. The function receives the full group and can return a DataFrame, Series, or scalar, allowing more flexible output shapes than `aggregate` or `transform`.
```python
#signature
pandas.core.groupby.DataFrameGroupBy.apply(func, *args, **kwargs)
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Receives DataFrame. </div>

**DataFrameGroupBy.transform**  
Performs group-wise transformation while preserving the original index and the shape of the selected data. The function must return a result with the same shape as its input or a result that can be broadcast to that shape, allowing group-level scalar values to be repeated across rows.
```python
#signature
pandas.core.groupby.DataFrameGroupBy.transform(func, *args, engine=None, engine_kwargs=None, **kwargs)
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Receives Series. </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Only <code>transform()</code> will silently drop columns due to dtype incompatibility. The other <code>GroupBy</code> methods will usually raise errors instead of dropping. This happens silently- transform is designed to broadcast numeric group statistics. Non-compatible dtypes simply don’t participate. </div>

---
## 21: Pivot Tables 

**pandas.pivot_table**  
Creates a spreadsheet-style pivot table as a DataFrame. Combines grouping, aggregation, and reshaping to arrange summarized values by row and column labels.
```python
#signature
pandas.pivot_table(data, values=None, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False, dropna=True, margins_name='All', observed=True, sort=True, **kwargs)
```

```python
#example
titanic.pivot_table('survived', index=['sex'], columns='class', aggfunc='mean')

pd.pivot_table(
    titanic,
    values='survived',
    index=['sex', demo],
    columns='class',
    aggfunc='mean',
)

titanic.pivot_table('survived', ['sex', demo], [fare, 'class'])
```

---
## 22: Vectorized String Operations
**pandas.Series.str**  
Provides access to vectorized string operations through a `StringMethods` accessor object. The `.str` accessor validates the Series data and exposes string methods. It is not a lazy generator; calling its methods performs the corresponding operations.
```python
#accessor
pd.Series.str
```

The following python methods work through the string accessor:

1. built-ins  : `len`, `lower`, `translate`, `islower`, `ljust`, `upper`, `startswith`, `isupper`, `rjust`, `find`, `endswith`, `isnumeric`, `center`, `rfind`, `isalnum`, `isdecimal`, `zfill`, `index`, `isalpha`, `split`, `strip`, `rindex`, `isdigit`, `rsplit`, `rstrip`, `capitalize`, `isspace`, `partition`, `lstrip`, `swapcase`

2. regex : `match`, `extract`, `findall`, `replace`, `contains`, `count`, `split`

3. other methods : `get`, `slice`, `slice_replace`, `cat`, `repeat`, `normalize`, `pad`, `wrap`, `join`, `get_dummies`

**pandas.Series.str.get_dummies**  
Returns a DataFrame of dummy/indicator variables. Each distinct token becomes a column, with `1` indicating its presence in a Series element and `0` indicating its absence.
```python
#usage
pd.Series.str.get_dummies(sep=delimiter)
```

- `sep` : Delimiter used to split each string into tokens; defaults to `'|'`.

---
## 23: Time Series

**time objects**
Time objects are categorized by measurement type; dates, periods, and time shifts (deltas). Each time object has a related 'index' object, which is a container for storying multiples (the associated index object is not an `Index` object).

In pandas, a time series is a structured data object built on specialized time-aware classes, providing native support for time indexing, alignment, conversion, and date/time arithmetic.

Times Series make use of `np.datetime64`, which costs time resolution by max time span equal to
$$2^{64}\approx18446744073709551616M\approx\text{18 quintillion}$$
Values are not perfectly divisible relative to total timespan; each time unit has a max time span capacity. The time span relative to time resolution. See [[#64-Bit-TimeSpan Tradeoff]].

**1. Timestamp and DatetimeIndex**
A `Timestamp` object represents a single instance of time at a date or time. If you omit either a date or a time, `Timestamp` will still resolve to a date + time value i.e. `pd.Timestamp('2020-01')`  $\rightarrow$  `Timestamp('2020-01-01 00:00:00')`.

A `DatetimeIndex` is an aperiodic container for `Timestamp` objects; a container of time points. If the given values for a `DatetimeIndex([values], freq=a)` do not conform to `freq=a`, you will get `ValueError`, as `DatetimeIndex` interprets values as `Timestamp` objects. 

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Receives Series. </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ :  Because <code>Timestamp</code> specifies a date + time, <code>pd.Timestamp('00:00:00')</code> defaults to the daily time at the present date. All <code>Timestamp</code> objects are fixed relationally to the unix epoch. </div>


**2. Period  andPeriodIndex**
A period object represents a single span of time at a time reference + frequency. If you omit a frequency, it will infer through context i.e. `pd.Period('00:00:00')` $\rightarrow$ `Period('1-01-01 00:00:00', 'S')`.

A `PeriodIndex` is a periodic container for `Period` objects; a container of time references by a frequency. If the given values for a `PeriodIndex([values], freq=a)` do not conform to `freq=a`, `PeriodIndex` will attempt to coerce them to the given `freq`, frequency.

**3.Timedelta and TimedeltaIndex**
Timedelta object represents a single difference in time as time + units. If you omit a units, integers will default to nanoseconds. Use `help(pd.Timedelta)` to determine 

TimedeltaIndex is a container for Timedelta objects. Values represent vectors and can be heterogenous. 

**time series operations and indexing**
A set of `Timestamp` objects form an affine space. Subsequently, `Timestamp` objects are defined only on certain operations. Although, a `Timestamp` object does hold an origin, the unix epoch, it does not support the addition of points as the value of a date is ambiguous, although the difference between two points is measurable. Supported operations include the following.
-  `Timestamp - Timestamp` $\rightarrow$ `Timedelta #vector`
-  `Timestamp ± Timedelta` $\rightarrow$ `Timestamp #point` 

All time-based indexing uses the `.loc` indexer. This can be used partially or fully i.e. 
- `df.loc['2012']` $\rightarrow$ retrieves all `2012` values
- `df.loc['2012-01']` $\rightarrow$ retrieves all `2012-01` values

Additionally, timestamp objects supports date slicing. 
- `df.loc['2012' : '2015']`

**frequencies and offsets**
Frequency codes specify the frequency of time objects, particularly the `freq` param. 

**unix epoch**
An epoch is a fixed reference point from which time is measured. The Unix epoch is January 1, 1970, at 00:00:00 UTC. Integer inputs to `pd.Timestamp` default to nanoseconds relative to this epoch; positive values represent times after it, and negative values represent times before it. For example, `pd.Timestamp(45)` returns `Timestamp('1970-01-01 00:00:00.000000045')`.

**affine space**
An affine space is a space with no distinguished origin. Its points represent locations, while vectors represent displacements between those locations. Choosing an origin allows points to be identified with vectors relative to that origin. Adding two points, scaling a point, or taking a dot product of points is not intrinsically defined. However, subtracting two points produces a vector, \(Q-P \in V\), and adding a vector to a point produces another point, \(P+v \in A\).

Timestamps follow this pattern: subtracting two timestamps produces a duration, while adding a duration to a timestamp produces another timestamp.

**pandas.DatetimeIndex**  
An immutable index of datetime values, supporting date components, rounding, formatting, and timezone operations.
```python
#signature
pandas.DatetimeIndex(data=None, freq=<no_default>, tz=<no_default>, ambiguous='raise', dayfirst=False, yearfirst=False, dtype=None, copy=None, name=None)
```
methods for this object:
- `normalize()` : Converts times to midnight while preserving dates and timezone.
- `strftime(format)` : Formats datetimes as strings using the specified pattern.
- `round(freq)` : Rounds datetimes to the nearest specified frequency.
- `floor(freq)` : Rounds datetimes down to the specified frequency.
- `ceil(freq)` : Rounds datetimes up to the specified frequency.
- `to_period(freq)` : Converts the DatetimeIndex to a PeriodIndex at the given frequency.
- `to_series()` : Converts the index into a Series.
- `tz_localize(tz)` : Assigns a timezone to timezone-naive datetimes without changing their local clock times.
- `tz_convert(tz)` : Converts timezone-aware datetimes to another timezone while preserving the instants they represent.

attributes for this object:
- `year` : Year of each datetime.
- `month` : Month number, from January=1 to December=12.
- `day` : Day of the month.
- `dayofweek` : Day of the week, from Monday=0 to Sunday=6.
- `dayofyear` : Ordinal day of the year, starting at 1.
- `quarter` : Quarter of the year, from 1 to 4.


**pandas.to_datetime**  
Converts date and time input to pandas datetime objects: a Timestamp for scalar input, a DatetimeIndex for array-like input, or a datetime Series for Series input. Accepts dates with or without a time component. When only a date is provided, the time defaults to midnight (`00:00:00`).
```python
#signature
pd.to_datetime(arg, errors='raise', dayfirst=False, yearfirst=False, utc=False, format=None, exact=<no_default>, unit=None, origin='unix', cache=True)
```

**pandas.DataFrame.to_period / pandas.Series.to_period**  
Converts a DatetimeIndex to a PeriodIndex, leaving the data values unchanged. For a DataFrame, the `axis` parameter selects which axis to convert. To convert datetime values within a Series, use `series.dt.to_period(freq)`.
```python
#signature
pd.DataFrame.to_period(freq=None, axis=0, copy=None)
pd.Series.to_period(freq=None, copy=None)
```

- `freq` : Frequency of the resulting periods; inferred from the datetime index if omitted.

**pandas.date_range**  
Returns a DatetimeIndex of equally spaced timestamps within the specified range. If the start is later than the end and the frequency moves forward, the result is empty.
```python
#signature
pandas.date_range(start=None, end=None, periods=None, freq=None, tz=None, normalize=False, name=None, inclusive='both', *, unit=None, **kwargs)
```

**pandas.period_range**  
Returns a PeriodIndex containing periods at a specified frequency within the given range.
```python
#signature
pandas.period_range(start=None, end=None, periods=None, freq=None, name=None)
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : <code>period_range</code> vs. <code>date_range</code>- consider <code>period_range</code> returns period objects, whereas <code>date_range</code> returns datetime objects. Period objects are not vectors, but are labeled buckets of time intervals whereas datetime objects are exact points in time. A period must be accompanied by a frequency, whereas a datetime may be accompanied by a frequency. Moreover, arithmetic respects period logic i.e months stays months. </div>

**pandas.DataFrame.resample**  
Groups time-series data into frequency-based bins and returns a Resampler object, on which aggregation methods such as `mean()` can be called. Empty bins may produce missing values, depending on the aggregation and data type. Numeric means typically produce `NaN`, while missing datetime values use `NaT`.
```python
#example
df.resample('ME').mean()
```

**pandas.DataFrame.asfreq**  
Converts a time series to a specified frequency by reindexing, without aggregation. Also available on Series.

Values are retained at matching timestamps; newly introduced timestamps receive missing values unless a filling option is specified.
```python
#signature
pd.DataFrame.asfreq(freq, method=None, how=None, normalize=False, fill_value=None)
```

- `freq` : Target frequency.
- `method` : Uses `'ffill'` to carry the previous value forward or `'bfill'` to use the next value. These are filling methods, not linear interpolation.
- `how` : For a PeriodIndex, selects the start or end of each period using `'start'` or `'end'`.
- `normalize` : Sets the resulting timestamps to midnight when `True`.
- `fill_value` : Fills missing values introduced by reindexing; does not replace missing values already present in the data.

**datetime.strftime**  
Formats a datetime object as a string using a specified pattern. The correct spelling is `strftime`, not `strfrmtime`. Pandas also provides this operation through `Timestamp.strftime()`, `DatetimeIndex.strftime()`, and `Series.dt.strftime()`.
```python
#example
dt.strftime('%Y/%d/%m')
```

**pandas.DataFrame.rolling**  
Returns a Rolling object representing moving windows over the data. Calling an aggregation method computes a result for each window; the object is not itself a lazy iterator.
```python
#signature
DataFrame.rolling(window, min_periods=None, center=False, win_type=None, on=None, closed=None, step=None, method='single')
```

- `window` : Window size, such as an integer number of observations or a time interval.
- `min_periods` : Minimum number of observations required within a window to produce a result.
- `center` : Places result labels at the center of each window when `True`.
```python
#example
gdp.rolling(5, center=True).mean()
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Many pandas methods are absent from <code>pandas.DataFrame.__dict__</code> because they are inherited from <code>pandas.core.generic.NDFrame</code> or other base classes. Use <code>pandas.DataFrame.mro()</code> to inspect the method resolution order and search each class’s <code>__dict__</code> to locate a method’s definition. The <code>NDFrame</code> base class provides shared functionality for <code>DataFrame</code> and <code>Series</code>, although some methods are defined or overridden directly on those subclasses. </div>


---
## 24: Eval and Query
**compound operations**
Compound operations perform multiple operations generating intermediate `DataFrame` objects, consuming memory. Consider the operation `df[df>5] & df[df<4]` is computationally equal to `df1=df[df>5]`, `df2=df[df<4]`, `df1 & df2`. Multiple intermediate `DataFrame` objects are created to compute `df1 & df2`. 

The way around that is to use `pd.eval`, `df.eval`, and `df.query`, which take a `str` formatted version of compound operation performing the entire computation at once i.e. `pd.eval('df[df>5] & df[df<4]')`. Note, because the computations are `str`, scope adjusts accordingly. 

**pandas.eval**  
Evaluates a string expression using objects from the local or global namespace. Can improve efficiency for large array-based computations by reducing intermediate allocations. Supports arithmetic, comparisons, boolean/bitwise operations, attribute access such as `obj.attr`, and indexing such as `obj[index]`. Supported expressions depend on the parser and engine.
```python
#signature
pandas.eval(expr, parser='pandas', engine=None, local_dict=None, global_dict=None, resolvers=(), level=0, target=None, inplace=False)
```

**pandas.DataFrame.eval**  
Evaluates a string expression involving DataFrame columns. Column names can be referenced directly, as in `df.eval('A + B')`. External variables can be referenced with `@`; for example, if `adder = 5`, then `df.eval('A + @adder')` adds 5 to column `A`. Supports assignments such as `df.eval('D = A + B')`, which returns a new DataFrame containing column `D` by default.
```python
#signature
DataFrame.eval(expr, *, inplace=False, **kwargs)
```

- `inplace` : When `True`, assignment expressions modify the original DataFrame.

**pandas.DataFrame.query**  
Filters rows using a string expression, retaining rows where the condition is true. Supports `@` to reference external variables, such as `df.query('A > @threshold')`. Does not support assignment expressions.
```python
#signature
DataFrame.query(expr, *, inplace=False, **kwargs)
```

- `inplace` : When `True`, modifies the original DataFrame instead of returning a filtered copy.
- `**kwargs` : Additional evaluation options, such as `parser`, `engine`, `local_dict`, and `global_dict`.

---
## More...
| corrected name                    | purpose                                                                                                                         |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `pd.to_datetime()`                | Converts input to pandas datetime objects                                                                                       |
| `df.to_csv()` / `series.to_csv()` | Exports a DataFrame or Series to CSV; not `pd.to_csv()`                                                                         |
| `pd.read_csv()`                   | Reads CSV data into a DataFrame                                                                                                 |
| `pd.read_*()`                     | Refers to pandas input functions such as `pd.read_excel()`, `pd.read_json()`, and `pd.read_parquet()`; the `*` is a placeholder |
| `df.astype()` / `series.astype()` | Converts data to a specified dtype; not `pd.astype()`                                                                           |

---
# IV: Matplotlib

---
## 25: Interface Objects
**matplotlib interfaces**
Matplotlib has two main plotting interfaces: the pyplot interface, inspired by MATLAB, and the object-oriented (OOP) interface. The latter provides more explicit control and is particularly useful for managing multiple plots.

**pyplot interface**
A stateful interface used for quick plotting. Functions such as `plt.plot()` operate on the current figure and axes, which pyplot tracks automatically. Subsequent commands affect whichever figure or axes is currently active.

**OOP Interface**
An interface that uses explicit references to Figure and Axes objects, commonly created together with `fig, ax = plt.subplots()`. The Figure represents the overall canvas and contains one or more Axes. Each Axes represents a plotting area containing plotted data, ticks, gridlines, labels, and a title. Methods such as `ax.plot()` act directly on a particular Axes.

**matplotlib.pyplot.figure**  
Creates a new Figure or activates an existing Figure.
```python
#signature
matplotlib.pyplot.figure(num=None, figsize=None, dpi=None, *, facecolor=None, edgecolor=None, frameon=True, FigureClass=<class 'matplotlib.figure.Figure'>, clear=False, **kwargs) -> Figure
```

- `figsize` : Figure size as `(width, height)` in inches.

**matplotlib.figure.Figure**  
The Figure class represents the overall plotting canvas. Its constructor accepts general figure settings.
```python
#signature
matplotlib.figure.Figure(figsize=None, dpi=None, *, facecolor=None, edgecolor=None, linewidth=0.0, frameon=None, subplotpars=None, tight_layout=None, constrained_layout=None, layout=None, **kwargs)
```

**Figure.add_axes**  
Adds an Axes to the Figure. Accepts a rectangle specifying its position and size, or an existing Axes belonging to the same Figure.
```python
#signature
Figure.add_axes(*args, **kwargs)

#call forms
fig.add_axes(rect, projection=None, polar=False, **kwargs)
fig.add_axes(ax)
```

- `rect` : Tuple `(left, bottom, width, height)` specifying the Axes position and size as fractions of the Figure’s width and height.

**matplotlib.axes.Axes**  
Represents an individual plot or subplot, containing plotted data, ticks, gridlines, labels, and a title.
```python
#signature
matplotlib.axes.Axes(fig, *args, facecolor=None, frameon=True, sharex=None, sharey=None, label='', xscale=None, yscale=None, box_aspect=None, forward_navigation_events='auto', **kwargs)
```

**matplotlib.pyplot.axes**  
A convenience wrapper that adds an Axes to the current Figure and makes it the current Axes. Passing an existing Axes makes that Axes current.
```python
#signature
matplotlib.pyplot.axes(arg=None, **kwargs) -> Axes

#call forms
plt.axes(rect, projection=None, polar=False, **kwargs)
plt.axes(ax)
```

- `rect` : Tuple `(left, bottom, width, height)` specifying the Axes position and size as fractions of the Figure’s width and height.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Each new <code>Axes</code> added to a <code>Figure</code> is drawn in front of previously created <code>Axes</code> when they have the same <code>zorder</code>. The <code>rect</code> parameter defines the <code>Axes</code> position and size as fractions of the <code>Figure</code> dimensions in the form <code>[left, bottom, width, height]</code>. For example, <code>rect=[0, 0.25, 1, 1]</code> places the bottom of the <code>Axes</code> 25% up from the bottom of the Figure. Because its height is 100% of the Figure’s height, its top extends 25% beyond the Figure. </div>

**matplotlib interfaces: pyplot vs. oop**
The `Figure` object represents the top level container in which an `Axes` is stored (x and y axes, ticks, labels, title, grid, plotted lines, etc). The interaction between `Figure` and `Axes` seems implicit, but when you call a figure object, `fig=plt.figure`, it registers that `Figure` in a global “current figure” slot inside pyplot (`Figure` acts as a context manager, but does not implement the protocol, i.e. `__enter__`, `__exit__`). This is why calling `Figure` usually proceeds calling `Axes`. If you call  `Axes` without creating a `Figure` object, pyplot will create one internally. When you call pyplot, it implicitly does `fig = get_current_figure()` $\rightarrow$ `ax = fig.add_axes(default_position)` $\rightarrow$ `set_current_axes(ax)` $\rightarrow$ `return ax` (pseudocode).  The rules are shown as follows.

1. The pyplot interface tracks the current Figure and Axes internally. Functions operate on these current objects: `plt.gcf()` retrieves the current Figure, and `plt.gca()` retrieves the current Axes, creating them if necessary.
2. The object-oriented (OOP) interface uses explicit references to Figure and Axes objects and calls their methods directly. Assigning `plt.figure()` to a variable does not bypass pyplot’s internal tracking; the distinction is how subsequent operations target the objects.

**OOP-style plotting**
Additional Axes can be added by calling `fig.add_axes(...)` again. Each returned Axes can be managed independently.
```python
fig = plt.figure()                  # Create a Figure; pyplot also tracks it
ax1 = fig.add_axes([0, 0, 1, 1])    # Add an Axes directly to this Figure
ax1.plot(x, y)                      # Plot directly on this Axes
plt.show()
```

**pyplot-style plotting**
Pyplot determines which objects receive the plotting commands through its current Figure and Axes state.
```python
plt.plot(x, y)    # Plot on the current Axes; create a Figure and Axes if needed
plt.show()
```

**hybrid plotting**
This combines pyplot’s management of the current Figure with direct method calls on the returned Axes.
```python
fig = plt.figure()    # Create a Figure and make it current
ax = plt.axes()       # Add an Axes to pyplot’s current Figure
ax.plot(x, y)        # Plot through an explicit Axes reference
plt.show()
```

**matplotlib.pyplot.gca**  
Returns the current Axes. If the current Figure has no Axes, creates one using `Figure.add_subplot()`.
```python
#signature
matplotlib.pyplot.gca()
```

**matplotlib.pyplot.gcf**  
Returns the current Figure. If no Figure exists in pyplot’s Figure stack, creates one using `figure()`.
```python
#signature
matplotlib.pyplot.gcf()
```

| plt Function   | ax Equivalent     | What It Does (Brief)                                                                                                                      |
| -------------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `plt.xlabel()` | `ax.set_xlabel()` | Sets the label for the x-axis.                                                                                                            |
| `plt.ylabel()` | `ax.set_ylabel()` | Sets the label for the y-axis.                                                                                                            |
| `plt.xlim()`   | `ax.set_xlim()`   | Sets the limits (min, max) of the x-axis.                                                                                                 |
| `plt.ylim()`   | `ax.set_ylim()`   | Sets the limits (min, max) of the y-axis.                                                                                                 |
| `plt.title()`  | `ax.set_title()`  | Sets the title of the axes.                                                                                                               |
| `plt.legend()` | `ax.legend()`     | Adds a legend to the plot. Requires `label=` parameter inside plotting calls (e.g., `ax.plot(..., label="Series A")`) to display entries. |

---
## 26: Linear Plots
**matplotlib.axes.Axes.plot**  
Plots data as lines and/or markers. Returns a list of `Line2D` objects, one for each plotted line.

```python
#signature
matplotlib.axes.Axes.plot(*args, scalex=True, scaley=True, data=None, **kwargs)

#call forms
plot([x], y, [fmt], *, data=None, **kwargs)
plot([x], y, [fmt], [x2], y2, [fmt2], ..., **kwargs)
```

- `x` : Optional horizontal coordinates; defaults to `0` through `len(y) - 1`.
- `y` : Vertical coordinates.
- `fmt` : Optional format string specifying marker, line style, and color.
- `scalex` / `scaley` : Whether to automatically adjust the corresponding axis limits.
- `data` : Object containing named data that can be referenced through string arguments.
- `**kwargs` : Additional `Line2D` properties.

Square brackets in the call forms indicate optional arguments, not literal lists.

`Axes.set(*, adjustable=<UNSET>, agg_filter=<UNSET>, alpha=<UNSET>, anchor=<UNSET>, animated=<UNSET>, aspect=<UNSET>, autoscale_on=<UNSET>, autoscalex_on=<UNSET>, autoscaley_on=<UNSET>, axes_locator=<UNSET>, axisbelow=<UNSET>, box_aspect=<UNSET>, clip_box=<UNSET>, clip_on=<UNSET>, clip_path=<UNSET>, facecolor=<UNSET>, forward_navigation_events=<UNSET>, frame_on=<UNSET>, gid=<UNSET>, in_layout=<UNSET>, label=<UNSET>, mouseover=<UNSET>, navigate=<UNSET>, path_effects=<UNSET>, picker=<UNSET>, position=<UNSET>, prop_cycle=<UNSET>, rasterization_zorder=<UNSET>, rasterized=<UNSET>, sketch_params=<UNSET>, snap=<UNSET>, subplotspec=<UNSET>, title=<UNSET>, transform=<UNSET>, url=<UNSET>, visible=<UNSET>, xbound=<UNSET>, xlabel=<UNSET>, xlim=<UNSET>, xmargin=<UNSET>, xscale=<UNSET>, xticklabels=<UNSET>, xticks=<UNSET>, ybound=<UNSET>, ylabel=<UNSET>, ylim=<UNSET>, ymargin=<UNSET>, yscale=<UNSET>, yticklabels=<UNSET>, yticks=<UNSET>, zorder=<UNSET>)`
- convenience wrapper `Axes.set` functions
- these can all be specified individually, or through `ax.set` in one pass

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : There is no equivalent convenience wrapper for the pyplot interface. Each param must be specified individually. </div>

**matplotlib.pyplot.plot**  
The pyplot interface’s general-purpose line plotting function. Plots y versus x as lines and/or markers on the current Axes. Returns a list of `Line2D` objects. See `Line2D` for additional styling properties. Square brackets in the call forms indicate optional arguments, not literal lists.
```python
#signature
matplotlib.pyplot.plot(*args, scalex=True, scaley=True, data=None, **kwargs)

#call forms
plot([x], y, [fmt], *, data=None, **kwargs)
plot([x], y, [fmt], [x2], y2, [fmt2], ..., **kwargs)
```

**matplotlib.pyplot.axis**  
Convenience function for getting or setting axis limits, visibility, and aspect ratio on the current Axes. This sets equal scaling so that one unit on the x-axis has the same displayed length as one unit on the y-axis.

```python
#signature
matplotlib.pyplot.axis(arg=None, /, *, emit=True, **kwargs)
```

```python
#example
plt.axis('equal')
```

**matplotlib.lines.Line2D**
The general `Line2D` class representing a two-dimensional line. Plotting functions that return `Line2D` objects accept many of the above parameters as keyword arguments.
```python
#example
class matplotlib.lines.Line2D(
    xdata, ydata, *,
    linewidth=None, linestyle=None, color=None,
    gapcolor=None, marker=None, markersize=None,
    markeredgewidth=None, markeredgecolor=None,
    markerfacecolor=None, markerfacecoloralt='none',
    fillstyle=None, antialiased=None,
    dash_capstyle=None, solid_capstyle=None,
    dash_joinstyle=None, solid_joinstyle=None,
    pickradius=5, drawstyle=None, markevery=None,
    **kwargs
)
```
- `linestyle`: See Table 11, Matplotlib Line Style Codes.
- `color`: See Table 12, Matplotlib Colour Codes.
- `marker`: See Table 13, Matplotlib Scatter Markers.
- Defines line properties used by `plot` functions, rather than their complete input patterns.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : If you pass one value to <code>plot(a)</code>, it assumes <code>a=y</code>, and computes <code>x</code> as <code>np.arange(len(x))</code> or  (same results). </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : You must pass the <code>str</code> text of  <code>'None'</code> to <code>linestyle</code> and not the <code>None</code> object to eliminate lines. </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Codes can be mixed and matched i.e. <code>ax.plot(x, y, '-xk')</code>; solid line (<code>-</code>), x marker  (<code>x</code>), black colour (<code>k</code>). </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : The param <code>args</code> is a flexible positional input structure derived from <code>Line2D</code> often specified as <code>plot([x], y, fmt,...)</code> where <code>fmt</code> can instantly parse codes in format <code>'[line][marker][color]'</code> or vice-versa. </div>

**.plot**
- 1D : If you pass a singular object to plot,  `.plot(x)`, this is treated as `y`, as the call signature, `plot([x], y, [fmt], *, data=None, **kwargs)`, shows `x` is optional param is `x`. To create a plot, `.plot()` will auto-format your `x` values to `np.arange(len(x))`. 
 
- 2D : If you pass an `ndim=2` object to plot, `.plot(x)`, then it will interpret `y` as column-wise entries such that the array, `np.arange(8).reshape(2, 4)`, produces four lines `[0, 4]`, `[1, 5]`, `[2, 6]`, `[3,7]`, where `x` is `np.arange(len(x))` (consider `len(x)`) on `np.ndarray` returns the number of rows). 

- ?D : `Line2D` objects are 2D only. 

**matplotlib.pyplot.hlines**  
Plots horizontal lines at `y`, extending from `xmin` to `xmax`.
```python
#signature
matplotlib.pyplot.hlines(y, xmin, xmax, colors=None, linestyles='solid', *, label='', data=None, **kwargs)
```

- `linestyles` : Line style, commonly `'solid'` or `'dashed'`.

**matplotlib.pyplot.vlines**  
Plots vertical lines at `x`, extending from `ymin` to `ymax`.
```python
#signature
matplotlib.pyplot.vlines(x, ymin, ymax, colors=None, linestyles='solid', *, label='', data=None, **kwargs)
```

- `linestyles` : Line style, commonly `'solid'` or `'dashed'`.

**readers note**
The remaining chapters in this part wont explicitly label all interface versions as they are most interchangeable. 

---
## 27: Scatter Plots
**matplotlib.pyplot.plot**  
General-purpose line plotting function that can also display scatter-style plots. Using the format string `'o'` plots circular markers without connecting lines.
```python
#signature
matplotlib.pyplot.plot(*args, scalex=True, scaley=True, data=None, **kwargs)
```

```python
#example
plt.plot(x, y, 'o')
```

**matplotlib.pyplot.scatter**  
Creates a scatter plot and returns a `PathCollection`. Unlike `plt.plot()`, which uses common styling for each plotted dataset, `plt.scatter()` supports varying marker sizes, face colors, and edge colors across individual points. Numeric color values can be mapped through a colormap.
```python
#signature
matplotlib.pyplot.scatter(x, y, s=None, c=None, *, marker=None, cmap=None, norm=None, vmin=None, vmax=None, alpha=None, linewidths=None, edgecolors=None, colorizer=None, plotnonfinite=False, data=None, **kwargs) -> PathCollection
```

**PathCollection.legend_elements**  
Creates legend handles and labels from a scatter plot’s mapped colors or marker sizes. Each handle is a `Line2D` object, paired with a label representing the corresponding value.
```python
#example
scatter = plt.scatter([1, 2, 3], [4, 5, 6], c=[7, 2, 3])
plt.legend(*scatter.legend_elements(num=None))
```

**matplotlib.pyplot.errorbar**  
Plots data with horizontal and/or vertical error bars.
```python
#signature
matplotlib.pyplot.errorbar(x, y, yerr=None, xerr=None, fmt='', *, ecolor=None, elinewidth=None, capsize=None, barsabove=False, lolims=False, uplims=False, xlolims=False, xuplims=False, errorevery=1, capthick=None, data=None, **kwargs)
```

- `x` / `y` : Data positions.
- `xerr` / `yerr` : Horizontal and vertical error sizes. A scalar specifies the same symmetric ± error for every data point.

---
## 28: Density And Contour Plots
**contour plot**
An easy way to visualize three-dimensional data is by using a contour plot. A contour plot takes three two-dimensional arrays, X, Y, and Z, and constructs a two-dimensional representation of a three-dimensional surface.
$$  
 f : \mathbb{R}^2 \rightarrow \mathbb{R}  
$$
The function $f(x,y)$ defines a scalar field (a surface in $\mathbb{R}^3)$ via
 $$
 f(x, y)
$$
To evaluate this function over a grid, coordinate points must be expanded into coordinate matrices. This is commonly done with `np.meshgrid`, which takes two one-dimensional arrays x and y (`ndim=1`) and returns their structured Cartesian product arranged as matrices. If, `x.shape = (Nx,)`, `y.shape = (Ny,)`, then: `X, Y = np.meshgrid(x, y)`, produces `X.shape = (Ny, Nx)`, and `Y.shape = (Ny, Nx)`. The apparent reversal of `Ny` and Nx `occurs` because NumPy arrays are indexed as (rows, columns), where rows correspond to the y-direction (vertical axis) and columns correspond to the x-direction (horizontal axis).
![[np_meshgrid.png]]
Common pipeline looks like `x`, `y` are `np.linspace()`, and then passed to `X, Y = np.meshgrid(x,y)` $\rightarrow$ `np.contour(X, Y, Z)` where `Z` is some product of `X` and `Y`.  `Z` must have shape `(Ny, Nx)`, where `Nx=x.shape` and `Ny=y.shape`. 

**matplotlib.pyplot.contour**  
Draws contour lines connecting locations where a two-dimensional surface has a constant height or value.

Each coordinate pair `(X[i, j], Y[i, j])` has a corresponding value `Z[i, j]`. For a contour level of 3, the plot draws lines through locations where the surface equals 3, interpolating between grid points when necessary.
```python
#signature
matplotlib.pyplot.contour(*args, data=None, **kwargs) -> QuadContourSet

#call form
contour([X, Y,] Z, /, [levels], **kwargs)
```

- `X` / `Y` : Coordinate arrays. These can be 2D arrays matching the shape of `Z`, or 1D arrays specifying column and row coordinates, respectively. If omitted, integer grid coordinates are used.
- `Z` : A 2D array containing the surface value at each grid point.
- `levels` : An integer requesting automatically selected contour levels, or an increasing sequence specifying the exact levels.

**matplotlib.pyplot.contourf**  
Draws filled contour regions between consecutive levels rather than contour lines.
```python
#signature
matplotlib.pyplot.contourf(*args, data=None, **kwargs) -> QuadContourSet

#call form
contourf([X, Y,] Z, /, [levels], **kwargs)
```

**matplotlib.contour.QuadContourSet**  
Represents the contour set returned by `contour()` or `contourf()`. Its constructor exposes additional contour configuration options.
```python
#signature
matplotlib.contour.QuadContourSet(ax, *args, levels=None, filled=False, linewidths=None, linestyles=None, hatches=(None,), alpha=None, origin=None, extent=None, cmap=None, colors=None, norm=None, vmin=None, vmax=None, colorizer=None, extend='neither', antialiased=None, nchunk=0, locator=None, transform=None, negative_linestyles=None, clip_path=None, **kwargs)
```

**matplotlib.pyplot.hist**  
Groups data into bins and plots their counts or normalized densities.
```python
#signature
matplotlib.pyplot.hist(x, bins=None, *, range=None, density=False, weights=None, cumulative=False, bottom=None, histtype='bar', align='mid', orientation='vertical', rwidth=None, log=False, color=None, label=None, stacked=False, data=None, **kwargs)
```

- `bins` : Number of bins, explicit bin edges, or a supported bin-selection strategy. Using approximately $\sqrt{n}$ bins, where $n = \operatorname{len}(x)$, is a simple heuristic rather than a universally optimal choice.

**matplotlib.pyplot.imshow**  
Displays array data as a raster image. See Raster Graphics for more. For a 2D scalar array, values are normalized and mapped to colours through a colormap. With standard linear normalization:

$$
u = \frac{x - v_{\min}}{v_{\max} - v_{\min}}
$$

Values between `vmin` and `vmax` map to floating-point values between 0 and 1. Passing these normalized values to a colormap, such as `plt.cm.viridis(u)`, produces RGBA colors. Integer inputs to a colormap instead select entries by index.
```python
#signature
matplotlib.pyplot.imshow(X, cmap=None, norm=None, *, aspect=None, interpolation=None, alpha=None, vmin=None, vmax=None, colorizer=None, origin=None, extent=None, interpolation_stage=None, filternorm=True, filterrad=4.0, resample=None, url=None, data=None, **kwargs)
```

- `X` : Array-like data or a PIL image. Supported array shapes are `(M, N)` for scalar values, `(M, N, 3)` for RGB, and `(M, N, 4)` for RGBA.
- `cmap` : Colormap used for scalar data; ignored for RGB or RGBA input.
- `norm` : Normalization applied before mapping scalar values to colors.
- `vmin` / `vmax` : Bounds of the normalization range when using the default linear normalization.

For scalar data, `X.shape` determines the number of rows and columns in the image grid.

---
## 29: Legends
**matplotlib.pyplot.legend**  
Adds a legend to the current Axes. Use `ax.legend()` to add a legend to a specific Axes.
```python
#signature
matplotlib.pyplot.legend(*args, **kwargs)

#call forms
legend()
legend(handles, labels)
legend(handles=handles)
legend(labels)
```

- `handles` : Sequence of Artist objects, such as lines or patches, to include in the legend. Combine with `labels` for explicit control over the entries.
- `labels` : Text labels for the legend entries. When omitted, labels are taken from the Artists.
- `loc` : Location of the legend, such as `'upper right'`, `'lower left'`, or `'best'`.
- `ncols` : Number of columns in the legend.
- `scatterpoints` : Number of marker points displayed per scatter-plot legend entry.
- `fancybox` : Whether the legend frame has rounded corners.
- `shadow` : Whether to draw a shadow behind the legend.

**.labels**
The `legend` will only take inputs where the `label` param is defined. The way `legend` works 
1) is it scans the axes, 
2) it collects artists whose label does not start with `"_"`, 
3) For each artist, it looks up a legend handler based on the artist type
4) That handler decides how to draw the legend marker.

For instance, on `PathCollection`, the `label` controls whether the artist is included and what text appears, but the visual legend marker is derived from the artist’s own properties. If no values are passed to legend, then only the values passed to `legend` itself will be specified in the legend graphic. Again, only the values in `label` are passed to the `legend` as labels. 

**passing empty lists:** 
If the information you want in the legend cannot be automatically inferred from the plotted data, you can create dummy plotting objects with empty coordinates and labels. For example, `plt.scatter([], [], s=variable, label='name')` creates a `PathCollection` object with no visible points, but `legend()` can still use it as a legend handle. This method is useful for custom legends, especially marker-size legends, because you can loop through different marker sizes and assign each one a label. For example, `for size in [20, 80, 200]: plt.scatter([], [], s=size, label=f'size={size}')`, followed by `plt.legend()`, will create a legend showing different marker sizes even though no extra points are drawn on the plot.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : You can use an easier alternative method under <code>scatter.legend_elements</code>. </div>
## CHP30: Colormaps
**matplotlib.colormaps / matplotlib.pyplot.colormaps**  
Colormap registry. Use `list(matplotlib.colormaps)` or `plt.colormaps()` to list names.

**matplotlib.cm / matplotlib.pyplot.cm**  
Provides colormap objects, such as `cmap = plt.cm.plasma`. Floating-point inputs from 0 to 1 map to RGBA colors; integer inputs select color indices.

**matplotlib.colors.LinearSegmentedColormap**  
Creates a colormap using piecewise-linear interpolation.
```python
#signature
matplotlib.colors.LinearSegmentedColormap(name, segmentdata, N=256, gamma=1.0)
```

**matplotlib.colors.ListedColormap**  
Creates a colormap from a list of colors.
```python
#signature
matplotlib.colors.ListedColormap(colors, name='from_list', N=None)
```

**matplotlib.pyplot.colorbar**  
Adds a colorbar for a mappable object.
```python
#signature
matplotlib.pyplot.colorbar(mappable=None, cax=None, ax=None, **kwargs)
```

- `cax` : Axes in which to draw the colourbar.

**matplotlib.pyplot.clim**  
Sets the current image’s colour-mapping limits, also updating its associated colourbar.
```python
#signature
matplotlib.pyplot.clim(vmin=None, vmax=None)
```

**matplotlib.pyplot.get_cmap**  
Returns a colormap, optionally resampled to `lut` evenly spaced colours.
```python
#signature
matplotlib.pyplot.get_cmap(name=None, lut=None)
```

**the three colormap types**
1.Sequential Colormaps: Represent ordered data using a progression in lightness or colour. They may use a single hue or multiple hues. Perceptually uniform sequential colormaps make equal data changes appear as roughly equal visual changes and generally remain interpretable in grayscale. Other sequential colormaps may vary less consistently in lightness.

2.Diverging Colormaps: Use two distinct colour progressions extending from a neutral midpoint to show deviations around a meaningful reference value, such as zero or a mean.

3.Qualitative Colormaps: Use distinct colours to represent categories with no inherent order. The colours distinguish groups rather than indicate numerical magnitude.

---
## 31: Subplots
**matplotlib.figure.Figure.add_subplot**  
Adds a subplot to a Figure using the object-oriented interface.
```python
#signature
matplotlib.figure.Figure.add_subplot(*args, **kwargs)

#call forms
fig.add_subplot(nrows, ncols, index, **kwargs)
fig.add_subplot(pos, **kwargs)
fig.add_subplot(ax)
```

- `nrows` / `ncols` : Number of rows and columns in the subplot grid.
- `index` : Subplot position, starting at 1 in the upper-left and increasing across rows. For example, `(1, 2, 1)` selects the left subplot.
- `pos` : Three-digit shorthand, such as `121` for `(1, 2, 1)`.
- `ax` : An existing subplot Axes belonging to the same Figure.

For explicit positioning with `[left, bottom, width, height]`, use `fig.add_axes(...)`.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : This function is often confused with <code>plt.add_axes</code> which has a different standard call signature except when using <code>add_subplot(pos, ...)</code>  which looks very much the same. The two functions add an <code>Axes</code> to a <code>Figure</code> object, but when called as <code>add_subplot(nrows, ncols, index,...)</code>, adds a plot to a subplot arrangement.  </div>

**matplotlib.pyplot.subplots**  
Pyplot convenience function that creates a Figure and a set of subplot Axes, commonly assigned as `fig, ax = plt.subplots(...)`.
```python
#signature
matplotlib.pyplot.subplots(nrows=1, ncols=1, *, sharex=False, sharey=False, squeeze=True, width_ratios=None, height_ratios=None, subplot_kw=None, gridspec_kw=None, **fig_kw)
```

- `nrows` / `ncols` : Number of rows and columns in the subplot grid.
- `squeeze` : When `True`, returns a single Axes for a 1×1 grid, a 1D array for a single row or column, and a 2D array otherwise. Set `False` to always access subplots as `ax[i, j]`.
- `subplot_kw` : Dictionary passed to `Figure.add_subplot()` for each Axes; common keys include `projection`, `xticks`, `yticks`, and `facecolor`.
- `gridspec_kw` : Dictionary controlling the grid layout; common keys include `hspace`, `wspace`, `width_ratios`, and `height_ratios`.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Method instantly creates a grid.  </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : To automate plotting across numerous subplots, use <code>enumerate(axes.flat)</code> where <code>fig, axes = plt.subplots(...)</code> represents the grid. Note, this method allows you to  loop an index with a single integer, <code>[i]</code> across a two-dimensional subplot as opposed to looping two independent integers across each row x column, <code>[i, j]</code> i.e. <code>for i, axes in enumerate(axex.flat)</code> where <code>axes != ax</code>.  </div>

**matplotlib.figure.Figure.subplots_adjust**  
Adjusts subplot margins and spacing.
```python
#signature
matplotlib.figure.Figure.subplots_adjust(left=None, bottom=None, right=None, top=None, wspace=None, hspace=None)
```

- `left` / `bottom` / `right` / `top` : Boundaries of the subplot area as fractions of the Figure dimensions.
- `wspace` / `hspace` : Horizontal and vertical spacing as fractions of the average Axes width and height, respectively.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Method, only works if our grid was constructed as a subplot grid.  </div>

**matplotlib.gridspec.GridSpec**  
Defines a grid layout for placing subplots within a Figure, providing more control than `plt.subplots()`. Indexing or slicing the grid selects the cells a subplot occupies.
```python
#signature
matplotlib.gridspec.GridSpec(nrows, ncols, figure=None, left=None, bottom=None, right=None, top=None, wspace=None, hspace=None, width_ratios=None, height_ratios=None)
```

```python
#example
grid = plt.GridSpec(2, 3)
ax = plt.subplot(grid[0, 0])
```

---
## 32: Text and Annotation
**matplotlib.pyplot.text**  
Adds text at a specified position on the current Axes. Returns a `Text` object.
```python
#signature
matplotlib.pyplot.text(x, y, s, fontdict=None, **kwargs) -> Text
```

- `x` / `y` : Position in data coordinates by default. The origin depends on the axis limits; increasing `y` normally moves upward unless the y-axis is inverted.
- `s` : Text string.
- `fontdict` : Dictionary of text properties, such as `{"fontsize": 14, "fontweight": "bold", "fontstyle": "italic", "color": "blue"}`.
- `transform` : Coordinate system: `ax.transData` for data coordinates, `ax.transAxes` for Axes fractions, or `fig.transFigure` for Figure fractions. In the latter two, `(0, 0)` is bottom-left and `(1, 1)` is top-right.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Params, <code>x</code>, <code>y</code>, must be <code>dtype</code> consistent i.e. if the graph uses <code>DatetimeIndex</code>, then the `x` must take format `pd.Timestamp`.  </div>

**matplotlib.text.Text**  
Class returned by `plt.text()` and `ax.text()`.
```python
#signature
matplotlib.text.Text(x=0, y=0, text='', *, color=None, verticalalignment='baseline', horizontalalignment='left', multialignment=None, fontproperties=None, rotation=None, linespacing=None, rotation_mode=None, usetex=None, wrap=False, transform_rotates_text=False, parse_math=None, antialiased=None, **kwargs)
```

- `horizontalalignment` : Horizontal alignment; accepts the alias `ha`.

**Axes.annotate**  
Adds text with an optional arrow pointing to a specified location. Returns an `Annotation` object.
```python
#signature
Axes.annotate(text, xy, xytext=None, xycoords='data', textcoords=None, arrowprops=None, annotation_clip=None, **kwargs) -> Annotation
```

- `text` : Annotation text.
- `xy` : Point being annotated—the arrow’s destination.
- `xytext` : Text position; defaults to `xy`. The arrow points from here toward `xy`.
- `xycoords` : Coordinate system for `xy`, such as `'data'`, `'axes fraction'`, or `'figure fraction'`.
- `textcoords` : Coordinate system for `xytext`; defaults to `xycoords`. Supports `'offset points'` for positioning text relative to `xy`.
- `arrowprops` : Dictionary configuring the arrow, such as `{"arrowstyle": "->"}`. Supports `FancyArrowPatch` properties when `arrowstyle` is specified.

**matplotlib.text.Annotation**  
Text subclass representing an annotation and its optional arrow. Inherits from `Text` and `_AnnotationBase`.
```python
#signature
matplotlib.text.Annotation(text, xy, xytext=None, xycoords='data', textcoords=None, arrowprops=None, annotation_clip=None, **kwargs)
```

**matplotlib.patches.FancyArrowPatch**  
Configurable arrow patch used by annotations.
```python
#signature
matplotlib.patches.FancyArrowPatch(posA=None, posB=None, *, path=None, arrowstyle='simple', connectionstyle='arc3', patchA=None, patchB=None, shrinkA=2, shrinkB=2, mutation_scale=1, mutation_aspect=1, **kwargs)
```

- `arrowstyle` : Arrow shape, such as `'->'`, `'<->'`, or `'-|>'`.
- `connectionstyle` : Connecting path style; for example, `'arc3,rad=0.2'` creates a curve. With `arc3`, `rad=0` gives a straight line, and its sign determines the direction of curvature.
---
## 33: Ticks, Axis, Spines
**formatter and locator**
Each `Axes` object has `Axes.xaxis` and `Axes.yaxis` attributes (also `Axes`), which provide access to the major and minor tick system for each axis. These axis objects manage both locators and formatters. 

Locators determine where ticks are placed. Formatters determine how the tick labels are displayed. 

Ticks themselves can be divided into major ticks (the primary, emphasized ticks) and minor ticks (the smaller, intermediate ticks between majors).

Examples include the following. 
- `ax.xaxis.set_major_locator(plt.MaxNLocator(20))` (set the max number of major locators on the x-axis equal to twenty).
 
- `ax.yaxis.set_major_locator(plt.FixedLocator([0, 0.135, 0.99]))` (the shown locators are 0, 0.135, and 0.99).

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Using locator and formatter objects to control axes is the manual method. You can use the high-level-helper functions  <code>set_xticks</code> or <code>set_yticks</code> for convenience. </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : The locator objects only reference values within the range of the data plotted on the <code>Axes</code> i.e. setting <code>FixedLocator([0, 0.135, 0.99]</code> will only display if the values exist in the data plotted, however their values may be displayed differently using a formatter object. </div>

**matplotlib.spines.Spine**  
Represents a border of the plotting area. Standard Cartesian Axes have four spines: `'left'`, `'right'`, `'top'`, and `'bottom'`, accessible through the dict-like `ax.spines` container.
```python
#signature
matplotlib.spines.Spine(axes, spine_type, path, **kwargs)
```

Use `ax.spines.keys()` to inspect names and `ax.spines.values()` to iterate over the Spine objects.

- `set_visible(bool)` : Sets visibility.
- `set_color(color)` : Sets border color.
- `set_linewidth(width)` : Sets width in points.
- `set_linestyle(style)` : Sets line style.
- `set_alpha(alpha)` : Sets opacity.
- `set_position(position)` : Sets spine position.
- `set_bounds(low, high)` : Restricts the spine’s extent in data coordinates.
```python
#example
ax.spines['bottom'].set_linewidth(2)

for spine in ax.spines.values():
    spine.set_color('gray')
```

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Sometimes you might need to <code>set_visible(True)</code> and <code>set_color(...)</code> and <code>set_linewidth(...)</code>. </div>

**Axes.set_frame_on**  
Toggles drawing of the Axes background patch and spines. Enabling it does not override their individual visibility settings.
```python
#signature
Axes.set_frame_on(b)
```

- `b` : `True` to enable the frame; `False` to hide it.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : There is no pyplot equivalent way to edit spines so you must use <code>ax = plt.gca()</code> to get the <code>Axes</code> and edit each individual spine. </div>

**Axes.set_xticks**  
Sets x-axis tick locations and optional labels without manually creating locator or formatter objects.
```python
#signature
Axes.set_xticks(ticks, labels=None, *, minor=False, **kwargs)
```

**Axes.set_yticks**  
Sets y-axis tick locations and optional labels without manually creating locator or formatter objects.
```python
#signature
Axes.set_yticks(ticks, labels=None, *, minor=False, **kwargs)
```

- `ticks` : Tick positions.
- `labels` : Optional labels corresponding to the positions.
- `minor` : Sets minor ticks when `True`; major ticks otherwise.
- `**kwargs` : Text properties for explicitly supplied labels.

**matplotlib.ticker**  
Module containing locator classes for tick positions and formatter classes for tick labels.

locators: 
- `NullLocator` : No ticks
- `FixedLocator` : Tick locations are fixed
- `IndexLocator` : Ticks placed at regular intervals based on index values
- `LinearLocator` : Evenly spaced ticks from min to max
- `LogLocator` : Logarithmically spaced ticks from min to max
- `MultipleLocator` : Ticks and range are a multiple of a base value
- `MaxNLocator` : Finds up to a maximum number of ticks at nice locations
- `AutoLocator` : (Default) `MaxNLocator` with simple defaults
- `AutoMinorLocator` : Locator for minor ticks

formatters:
- `NullFormatter` : No labels on the ticks
- `IndexFormatter` : Sets tick labels from a list of strings
- `FixedFormatter` : Sets tick labels manually
- `FuncFormatter` : Uses a user-defined function to generate tick labels
- `FormatStrFormatter` : Uses a format string for each tick value
 - `ScalarFormatter` : Default formatter for scalar values
- `LogFormatter` : Default formatter for log axes

**Axes.set_xlabel**  
Sets the x-axis label. Pyplot equivalent: `plt.xlabel()`.

```python
#signature
Axes.set_xlabel(xlabel, fontdict=None, labelpad=None, *, loc=None, **kwargs)
```

**Axes.set_ylabel**  
Sets the y-axis label. Pyplot equivalent: `plt.ylabel()`.
```python
#signature
Axes.set_ylabel(ylabel, fontdict=None, labelpad=None, *, loc=None, **kwargs)
```

**Axes.set_title**  
Sets the Axes title. Pyplot equivalent: `plt.title()`.
```python
#signature
Axes.set_title(label, fontdict=None, loc=None, pad=None, *, y=None, **kwargs)
```

- `fontdict` : Text properties for all three methods, such as `{"fontsize": 14, "fontweight": "bold", "fontstyle": "italic", "color": "blue"}`.

---
## 34: Stylesheets, rcParams
**rc params**
There are three ways to customize `mpl`. Only two are relevant. 
1. Customizing `rcParams` at runtime (run commands)
2. Setting a stylesheet

Matplotlib is designed to mix and match different stylesheets and `rcParams`, however running `rcParams` take precedence over stylesheets. 

**matplotlib.pyplot.style.use**  
Applies a stylesheet. Use `plt.style.available` to list available styles.
```python
#example
plt.style.use('ggplot')
```

**Axes.grid**  
Controls gridline visibility and styling. Pyplot equivalent: `plt.grid()`.
```python
#signature
Axes.grid(visible=None, which='major', axis='both', **kwargs)
```

**matplotlib.rcParams**  
Dict-like store of plotting configuration defaults. Changes generally affect subsequently created objects, rather than restyling existing plots.
```python
#example
mpl.rcParams['lines.linewidth'] = 2
mpl.rcParams.update({'lines.linewidth': 2, 'lines.linestyle': '--'})
mpl.rcParams.keys()
```

**matplotlib.pyplot.rc**  
Sets configuration defaults by group; for example, `'lines'` corresponds to `lines.*` keys in `rcParams`.
```python
#signature
plt.rc(group, **kwargs)

#example
plt.rc('lines', linewidth=2, linestyle='--')
```

**matplotlib.pyplot.rcdefaults**  
Restores Matplotlib’s built-in configuration defaults, except settings excluded from style changes.
```python
#signature
plt.rcdefaults()
```

**cycler and rcParams**
Matplotlib uses `cycler.cycler` to iterate certain `Artist` (or subclass) params  (i.e. `Line2D`) to vary output when plotted. Running `plt.plot()` multiple times on the same `Figure`, yields lines with different colours if `color` is left unspecified. This cycling occurs internally through a `cycler` object as a pseudo modulo operator. 

Consider, `color` is a param that when modified through `rcParam` has no effect on the `Figure` (even with `plt.rc`) because its controlled by the `cycler`. To change `color` you must use `cycler` i.e. either through `matploblic.rcParam['axes.prop_cycle'] = cycler(color='str')` or `plt.rc('axes', prop_cycle=cycler(color='str'))`, where `prop_cycle` takes a `cycler` object. 

**cycler.cycler**  
Creates a Cycler of named property values, commonly used for plotting style cycles.
```python
#call forms
cycler(label=iterable, ...)
cycler(label, iterable)
```

- `label` : Property key. In the two-argument form, it can be any hashable object; in keyword form, it must be a valid Python identifier.
- `iterable` : Property values to cycle through. Multiple keyword iterables must have equal lengths.
```python
#example
from cycler import cycler

styles = cycler(color=['red', 'blue'], linestyle=['-', '--'])
```

---
## 35: 3D Plotting
See matplotlib API for more. This section is skipped for relevance. 

---
## 36: seaborn
**seaborn.kdeplot**  
Plots a kernel density estimate—a smooth estimate of a distribution—in one or two dimensions.
```python
#signature
seaborn.kdeplot(data=None, *, x=None, y=None, hue=None, weights=None, palette=None, hue_order=None, hue_norm=None, color=None, fill=None, multiple='layer', common_norm=True, common_grid=False, cumulative=False, bw_method='scott', bw_adjust=1, warn_singular=True, log_scale=None, levels=10, thresh=0.05, gridsize=200, cut=3, clip=None, legend=True, cbar=False, cbar_ax=None, cbar_kws=None, ax=None, **kwargs)
```

- `data` : DataFrame, NumPy array, mapping, or sequence.
- `x` / `y` : Variables to plot; supplying both produces a two-dimensional density plot.

**seaborn.jointplot**  
Shows the relationship between two variables alongside their marginal distributions.
```python
#signature
seaborn.jointplot(data=None, *, x=None, y=None, hue=None, kind='scatter', height=6, ratio=5, space=0.2, dropna=False, xlim=None, ylim=None, color=None, palette=None, hue_order=None, hue_norm=None, marginal_ticks=False, joint_kws=None, marginal_kws=None, **kwargs)
```

- `kind` : Plot type: `'scatter'`, `'kde'`, `'hist'`, `'hex'`, `'reg'`, or `'resid'`.

**seaborn.pairplot**  
Shows pairwise relationships across multiple variables. Off-diagonal cells show relationships between the row and column variables; diagonal cells usually show each variable’s distribution.
```python
#signature
seaborn.pairplot(data, *, hue=None, hue_order=None, palette=None, vars=None, x_vars=None, y_vars=None, kind='scatter', diag_kind='auto', markers=None, height=2.5, aspect=1, corner=False, dropna=False, plot_kws=None, diag_kws=None, grid_kws=None, size=None)
```

- `hue` : Column used to distinguish groups by color.

**pairplot**
A pairplot measures the the value from one column relative to the value of another column, forming a join scatterplot. For the columns `height_cm = [185, 180, 170]` , `weight_kg = [69, 75, 54]`, pairplot values take `[(185, 69), (180, 75), (170, 54)]`. 

**seaborn.FacetGrid**  
Creates a subplot grid split by categorical variables. Apply the same plotting function to each subset to compare groups.
```python
#signature
seaborn.FacetGrid(data, *, row=None, col=None, hue=None, col_wrap=None, sharex=True, sharey=True, height=3, aspect=1, palette=None, row_order=None, col_order=None, hue_order=None, hue_kws=None, dropna=False, legend_out=True, despine=True, margin_titles=False, xlim=None, ylim=None, subplot_kws=None, gridspec_kws=None)
```

**FacetGrid.map**  
Applies a plotting function to each subset. Positional arguments name data columns; keyword arguments configure the plotting function.
```python
#signature
FacetGrid.map(func, *args, **kwargs)

#example
grid = sns.FacetGrid(data, col='category')
grid.map(plt.hist, 'tip_pct', bins=np.linspace(0, 40, 15))
```

**seaborn.scatterplot**  
Draws a scatter plot on a single Axes, optionally mapping variables to marker color, size, and style.
```python
#signature
seaborn.scatterplot(data=None, *, x=None, y=None, hue=None, size=None, style=None, palette=None, hue_order=None, hue_norm=None, sizes=None, size_order=None, size_norm=None, markers=True, style_order=None, legend='auto', ax=None, **kwargs)
```

**seaborn.relplot**  
Figure-level wrapper for scatter or line plots, optionally divided into panels by row and column. Returns a `FacetGrid`.
```python
#signature
seaborn.relplot(data=None, *, x=None, y=None, hue=None, size=None, style=None, units=None, weights=None, row=None, col=None, col_wrap=None, row_order=None, col_order=None, palette=None, hue_order=None, hue_norm=None, sizes=None, size_order=None, size_norm=None, markers=None, dashes=None, style_order=None, legend='auto', kind='scatter', height=5, aspect=1, facet_kws=None, **kwargs)
```

- `kind` : `'scatter'` or `'line'`.

**seaborn.lmplot**  
Combines `regplot()` and `FacetGrid` to plot data and regression fits across subsets.
```python
#signature
seaborn.lmplot(data, *, x=None, y=None, hue=None, col=None, row=None, palette=None, col_wrap=None, height=5, aspect=1, markers='o', sharex=None, sharey=None, hue_order=None, col_order=None, row_order=None, legend=True, legend_out=None, x_estimator=None, x_bins=None, x_ci='ci', scatter=True, fit_reg=True, ci=95, n_boot=1000, units=None, seed=None, order=1, logistic=False, lowess=False, robust=False, logx=False, x_partial=None, y_partial=None, truncate=True, x_jitter=None, y_jitter=None, scatter_kws=None, line_kws=None, facet_kws=None)
```

**seaborn.axes_style**  
Returns a style dictionary that can also act as a context manager. Use `sns.set_style()` to apply a style globally.
```python
#signature
seaborn.axes_style(style=None, rc=None)

#example
with sns.axes_style('whitegrid'):
    sns.scatterplot(data=data, x='x', y='y')
```

- `style` : `'darkgrid'`, `'whitegrid'`, `'white'`, `'dark'`, or `'ticks'`.

**seaborn.catplot**  
Draws categorical plots across a `FacetGrid`.
```python
#signature
seaborn.catplot(data=None, *, x=None, y=None, hue=None, row=None, col=None, kind='strip', estimator='mean', errorbar=('ci', 95), n_boot=1000, seed=None, units=None, weights=None, order=None, hue_order=None, row_order=None, col_order=None, col_wrap=None, height=5, aspect=1, log_scale=None, native_scale=False, formatter=None, orient=None, color=None, palette=None, hue_norm=None, legend='auto', legend_out=True, sharex=True, sharey=True, margin_titles=False, facet_kws=None, ci=<deprecated>, **kwargs)
```

- `kind` : `'strip'`, `'swarm'`, `'box'`, `'violin'`, `'boxen'`, `'point'`, `'bar'`, or `'count'`.

**seaborn.heatmap**  
Displays rectangular data as a color-encoded matrix. Useful for visualizing the output of `sklearn.metrics.confusion_matrix()`.
```python
#signature
seaborn.heatmap(data, *, vmin=None, vmax=None, cmap=None, center=None, robust=False, annot=None, fmt='.2g', annot_kws=None, linewidths=0, linecolor='white', cbar=True, cbar_kws=None, cbar_ax=None, square=False, xticklabels='auto', yticklabels='auto', mask=None, ax=None, **kwargs)
```

---
## More
**mpl.patches**
Patches are `Artist` with a face colour and an edge colour. They are added via the OOP interface i.e. `ax.add_patch` (`matplotlib.axes.Axes.add_patch(p)` $\rightarrow$ `p` can be summoned from the pyplot interface or the OOP interfaces $\rightarrow$ `plt.circle` or `mpl.patches.Circle()`, which both return the `Circle` patch. 

**different types of patch classes**
`Annulus`, `Arc`, `Arrow`, `ArrowStyle`, `BoxStyle`, `Circle`, `CirclePolygon`, `ConnectionPatch`, `ConnectionStyle`, `Ellipse`, `FancyArrow`, `FancyArrowPatch`, `FancyBboxPatch`, `Patch`, `PathPatch`, `StepPatch`, `Polygon`, `Rectangle`, `RegularPolygon`, `Shadow`, `Wedge`

**matplotlib.patches.Patch**  
Base class for 2D shapes with an edge and face color. Inherits from `Artist`.
```python
#signature
matplotlib.patches.Patch(*, edgecolor=None, facecolor=None, color=None, linewidth=None, linestyle=None, antialiased=None, hatch=None, fill=True, capstyle=None, joinstyle=None, hatchcolor=None, edgegapcolor=None, **kwargs)
```

**matplotlib.axes.Axes.add_patch**  
Adds a Patch to the Axes and returns it.
```python
#signature
matplotlib.axes.Axes.add_patch(p)
```

- `p` : Patch object to add.
---
# V: Machine Learning

---
## 37: Machine Learning Introduction
**supervised vs. unsupervised**
There are two primary types of machine learning:
1. Supervised: uses categorized data to make decisions (i.e. regression, classification).
2. Unsupervised: uses uncategorized data to make decisions (i.e. clustering, dimensionality reduction). 

**Target Array & Features Matrix**
A dataset is distinguished by its target array and its features matrix. 
1. Target Array: The dependent variable.
2. Features Matrix: The independent variables (columns/features forming a matrix) 

---
## 38: Scikit-Learn 
**sklearn API**
The `sklearn` API hosts a uniform interface for machine-learning inference where estimators are imported from modules as classes, instantiated with parameters as instances, and fitted to data. There process is as follows:
1. Choose a class of model by importing the appropriate estimator class from `sklearn`. 
2. Choose the model hyper-parameters by instantiating the class with desired values.
3. Arrange data into a features matrix and target vector.
4. Fit the model to your data by calling the `fit` method of the model instance.
5. Apply the model to new data. 
 The interface applies to all `sklearn` estimators. 

**Introductory Scikit-Learn Examples**
**Regression: Random Data**
Consider, `X`, must take `ndim=2`, as values in shape `shape=(100,)` are ambiguous with respect to column or row values (you an fit unambiguous 1D data as `X = x[:, np.newaxis]`). 
```python
from sklearn.linear_model import LinearRegression #supervised model
model = LinearRegression() #create an instance
model.fit(X, y) #fit the model instance to data
reg1 = model.predict(X_test)
```

**Classification: Iris Data**
In the example below, `X_iris` and `Y_iris` represent the feature matrix and target array respectively. More in following chapters. 
```python
from sklearn.naive_bayes import GaussianNB #supervised model
from sklearn.model_selection import train_test_split
Xtrain, Xtest, Ytrain, Ytest = train_test_split(X_iris, Y_iris,...)
model = GaussianNB()
model.fit(Xtrain, Ytrain)
y_model = model.predict(Xtest)

from sklearn.metrics import accuracy_score
accuracy_score(Ytest, y_model)
```

**Dimensionality Reduction (PCA): Iris Data**
Principal component analysis (`PCA`) is a transformation method, which changes a datasets' dimensions and therefore contains no `.predict`. Unsupervised models usually take feature array, (i.e. `X_iris`) on input. 
```python
from sklearn.decomposition import PCA #unsupervised model
model = PCA(n_components=2)
model.fit(X_iris)
X_2D = model.transfor
```

**Clustering: Iris Data**
Here, `y_gmm` produces data where each cluster is categorized numerically categorized by integers starting at zero. 
```python
from sklearn.mixture import GaussianMixture
model = GaussianMixture(n_components=3)
model.fit(X_iris, Y_iris)
y_gmm = model.predict(X_iris)
```

**Application: Digits Data**
See the attached notebook. Note, a confusion matrix is used in this example. A confusion matrix is a performance evaluation table for machine learning classification models, comparing predicted labels against actual target values) and plotted with `sns.heatmap()`. 

![[chp38_sklearn.ipynb]]

---
## 39: Hyper-Parameters and Model Validation
**train-test splits**
A model is 'trained' on statistical data (samples), which are split into two sets: training and testing sets. The training set usually has more samples than the testing set. The testing set is also called the 'holdout set'. 

The train-test split is formed to let a statistical model make prediction on unforeseen data. Consequently, the accuracy of predictions on a training set is higher than on a test set as a model tunes its parameters to the training set. 

**cross validation**
Cross validation is a validation method, which iterates the training and testing of a model on various data subsets called folds. These folds are then aggregated to produce a representative measure of the model on all folds. Cross validation can be 

**bias-variance tradeoff**
A model that underfits data is 'high bias'. It holds wrong assumptions at the of expensive of accuracy. A model that overfits data is 'high variance'. It exhibits high variance when measured against other datasets as its too sensitive to training set noise. 

The optimal model balances variance with bias to unearth the structure being modelled. 

**validation curves**
Shows a model’s training and validation score as a function of a chosen hyperparameter or complexity setting. The training score is most always higher than the validation score (for obvious reasons). If the training score is lower, there is likely an error. 

At low complexity the model underfits, at high complexity it overfits, and the validation score usually reaches a local maximum at an intermediate level of complexity in between. 

**sklearn.model_selection.validation_curve**  
Computes training and cross-validation scores across values of a single hyperparameter. Returns score arrays for plotting a validation curve.
```python
#signature
sklearn.model_selection.validation_curve(estimator, X, y, *, param_name, param_range, groups=None, cv=None, scoring=None, n_jobs=None, pre_dispatch='all', verbose=0, error_score=nan, params=None)
```

- `param_name` : Hyperparameter name, such as `'max_depth'`; use `'step_name__param_name'` for a parameter within a Pipeline.
- `param_range` : Sequence of values to test for the specified hyperparameter.
- `cv` : Number of cross-validation folds, a cross-validation splitter, or an iterable of train/test splits.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Note, <code>param_name</code> requires the double-underscore after the estimator param and the estimator param string is lowercase. </div>

**learning curves**
Shows a model's training and validation score as a function of increasing sample size. Generally, a model with a larger sample will accommodate greater model complexity, whereas a model with a smaller sample will not. 

**sklearn.model_selection.learning_curve**  
Computes training and cross-validation scores for different training-set sizes. Returns training sizes and score arrays for plotting a learning curve.
```python
#signature
sklearn.model_selection.learning_curve(estimator, X, y, *, groups=None, train_sizes=array([0.1, 0.33, 0.55, 0.78, 1.]), cv=None, scoring=None, exploit_incremental_learning=False, n_jobs=None, pre_dispatch='all', verbose=0, shuffle=False, random_state=None, error_score=nan, return_times=False, params=None)
```

- `train_sizes` : Sequence of integers specifying sample counts, or floats in `(0, 1]` specifying fractions of the maximum training-fold size. With standard 7-fold cross-validation, that maximum is approximately $\frac{6}{7}n$, where $n$ is the total sample size.

**sklearn.model_selection.GridSearchCV**  
Exhaustively searches specified hyperparameter combinations using cross-validation. Creates a search object with `fit()`, `score()`, and, when supported by the estimator and refitting is enabled, `predict()`.
```python
#signature
sklearn.model_selection.GridSearchCV(estimator, param_grid, *, scoring=None, n_jobs=None, refit=True, cv=None, verbose=0, pre_dispatch='2*n_jobs', error_score=nan, return_train_score=False)
```

- `param_grid` : Dictionary mapping parameter names to candidate values, or a list of such dictionaries.
- `refit` : When `True`, refits the best estimator on all data supplied to `fit()`.

*attributes*
- `best_estimator_` : Best estimator, already refitted; unavailable when `refit=False`.
- `best_score_` : Best mean cross-validation score. Scikit-learn scorers use higher-is-better values, including negated losses.
- `best_params_` : Dictionary of the best parameter settings.
- `cv_results_` : Dictionary of result arrays containing parameter combinations, scores, rankings, and timings.

*methods*
- `fit(X, y)` : Evaluates all parameter combinations and refits the best estimator when enabled.
- `predict(X)` : Predicts using the refitted best estimator. Alternatively, assign `model = search.best_estimator_` and call `model.predict(X)`; another `fit()` is unnecessary.

---
## CHP40: Feature Engineering
**transforming data**
Sometimes transforming data is better than using a different model. This method is used to train models on augmented data samples and then used to make predictions on the original data. 
 
Scikit-learn has a class of preprocessing transformers which transform data based on certain characteristics. 

**dummy variables**
Dummy variables are used to quantify categorical variables by parsing unique values from categorical features to singular columns represented by zeros and ones `False` and `True` respectively. 

Because dummy variables greatly extend the number of features in a matrix, by encoding unique categorical variables as columns, numpy uses sparse matrices, which are derived from SciPy, a scientific computing library for data optimization.

**scipy.sparse.csr_matrix**  
Stores a sparse matrix in compressed sparse row (CSR) format.
```python
#signature
scipy.sparse.csr_matrix(arg1, shape=None, dtype=None, copy=False, *, maxprint=None)
```

- `shape` : Matrix dimensions `(n_rows, n_columns)`.
- `size` : Number of stored entries, including explicitly stored zeros—not the total number of matrix positions.
- `data` : Array of stored values.
- `toarray()` : Returns a dense NumPy `ndarray`.

**sklearn.feature_extraction.DictVectorizer**  
Converts an iterable of feature dictionaries into an encoded matrix, returning a sparse CSR matrix by default. Numeric values are preserved; string-valued categorical features become one-hot indicator columns. Suited to mapping-style data.
```python
#signature
sklearn.feature_extraction.DictVectorizer(*, dtype=<class 'numpy.float64'>, separator='=', sparse=True, sort=True)
```

- `fit()` : Learns the feature-to-column mapping, including categorical expansions and column order.
- `transform()` : Encodes dictionaries using the learned mapping.
- `fit_transform()` : Learns the mapping and returns the encoded matrix.
- `get_feature_names_out()` : Returns feature names in output-column order.
- `get_params()` : Returns estimator configuration parameters.
- `inverse_transform()` : Converts an encoded matrix back to feature dictionaries.

**sklearn.preprocessing.OneHotEncoder**  
Encodes categorical columns as binary indicator columns. Requires 2D input and returns a sparse CSR matrix by default. Commonly used within a `ColumnTransformer`, which can be included in a `Pipeline`.

OneHotEncoder is commonly included in a ColumnTransformer within a Pipeline, allowing encoded and other feature columns to be combined without manual concatenation.

```python
#signature
sklearn.preprocessing.OneHotEncoder(*, categories='auto', drop=None, sparse_output=True, dtype=<class 'numpy.float64'>, handle_unknown='error', min_frequency=None, max_categories=None, feature_name_combiner='concat')
```

- `fit()` : Learns the categories for each input feature.
- `transform()` : Encodes data using the learned categories.
- `fit_transform()` : Learns the categories and returns the encoded matrix.
- `get_feature_names_out()` : Returns feature names in output-column order.
- `get_params()` : Returns estimator configuration parameters.

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Note, <code>OneHotEncoder</code> is not usually used on its own, but is part of a longer pipeline so that  manual concatenation is unnecessary. </div>

<div style="border: 1px solid #ffe680; color: white; padding: 5px; font-size: 12px;">⚠️ : Note, input must be two dimension; where categorical feature columns are concerned, use <code>dataset[['feature_name']]</code> to add trailing 1 to <code>shape</code>. </div>


**sklearn.preprocessing.LabelEncoder**  
Encodes target labels (`y`) as integers from `0` to `n_classes - 1`. Intended for target labels, not categorical feature columns; use `OrdinalEncoder` for those.
```python
#signature
sklearn.preprocessing.LabelEncoder()
```

- `fit()` : Learns the target classes.
- `transform()` : Converts labels to their learned integer codes.
- `fit_transform()` : Learns the classes and returns their integer codes.
```python
#example
encoder = LabelEncoder()
encoder.fit_transform(['adam', 'ben', 'abgao'])
# array([1, 2, 0])
```

**missing data**
Missing data is often resolved with some form imputation. Missing values can be imputed with simple methods like replacing them with the column or complex methods like matrix completion. The sophisticated methods tend to be application specific and are not covered in this section. 

**sklearn.impute.SimpleImputer**  
Fills missing values column-wise using the specified strategy.
```python
#signature
sklearn.impute.SimpleImputer(*, missing_values=nan, strategy='mean', fill_value=None, copy=True, add_indicator=False, keep_empty_features=False)
```
- `strategy` : `'mean'`, `'median'`, `'most_frequent'`, `'constant'`, or a callable passed directly. The callable receives each column’s non-missing values and returns a scalar replacement.
- `fill_value` : Replacement value when `strategy='constant'`.

**feature pipelines**
A feature pipeline is a programming and machine learning workflow that lets you apply a chained sequence of preprocessing and statistical transformations to a dataset automatically and consistently.

This is probably the most important feature of data manipulation in python.  

**sklearn.pipeline.make_pipeline**  
Constructs a Pipeline from estimators, automatically naming each step. Call `fit()` to train the pipeline and `predict()` when the final estimator supports prediction.
```python
#signature
sklearn.pipeline.make_pipeline(*steps, memory=None, transform_input=None, verbose=False)
```

- `*steps` : Estimators in execution order. Intermediate steps must support `fit()` and `transform()`.
```python
#example
model = make_pipeline(
    SimpleImputer(strategy='median'),
    PolynomialFeatures(degree=3),
    LinearRegression(),
)
```
## CHP41: Naive Bayes Classification
**Naive Bayes** 
$$\hat{y} = \arg\max_y P(y) \prod_{i=1}^{n} P(x_i \mid y)
$$
$where,$
$\hat{y} = \text{the probability of target y equalling a certain output, y=a}$
$x_i = \text{feature column i}$

A prediction is the class $y$ that maximizes the product of the conditional probabilities of each feature value given $y$, multiplied by the class probability of $y$. W

**Gaussian Naive Bayes**
$$
\hat{y} = \arg\max_{c \in \mathcal{C}} ; P(c)\prod_{i=1}^{n}  
\left[  
\frac{1}{\sqrt{2\pi \sigma_{c,i}^{2}}}  
\exp\left(-\frac{(x_i-\mu_{c,i})^{2}}{2\sigma_{c,i}^{2}}\right)  
\right]  
$$  
where,
$\frac{1}{\sqrt{2\pi \sigma_{c,i}^{2}}} = \text{normalizing constant of the gaussian density}$

$\exp\left(-\frac{(x_i-\mu_{c,i})^{2}}{2\sigma_{c,i}^{2}}\right) = \text{Gaussian (exponential) shape term}$

`class sklearn.naive_bayes.GaussianNB(*, priors=None, var_smoothing=1e-09)`
- general classification model serves as a lightweight bench line model to reference model performance against
- returns an instance of itself, `self`
- see the API for attributes specifically, [GaussianNB with sklearn](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html).

The left term (normalizing constant of the gaussian density) represents the normalization of the bell curve height to the data whereas the right term represents the the shape of the bell curve itself i.e. gaussian probability density function = height adjustment × bell-shape term. 
## CHP42: Linear Regression
**General Linear Regression**
$$ Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + \beta_3X_3 + \ldots \beta_nX_n + \epsilon$$
$$f(X) = \beta_0 + \sum_{i=1}^n \beta_i X_i + \epsilon$$
`class sklearn.linear_model.LinearRegression(*, fit_intercept=True, copy_X=True, tol=1e-06, n_jobs=None, positive=False)`
- [*attribute*]  `coef_` : learned $\beta$ values 
- [*attribute*] `intercept_` : learned $\beta_0$ value 

The linear regression function operates as follows: The $\beta_i$ parameters are learned through the design matrix. The design matrix is the feature matrix actually used on the model after all transformations. If $X$, is the design matrix, then $X \rightarrow \Phi(X)$, where $\Phi$ is the transformation i.e. gaussian basis function, polynomial features, etc. 

To compute the $\beta$ parameters we minimize the residual, which is equal to 
$$ \text{residual}_i = y_i - \hat{y_i}$$
where, $y_i$ is equal to
$$ \hat{y_i} = \beta_0 + \beta_1x_1 + \ldots+\beta_{ip}x_{ip}$$
resulting in the following minimization
$$\min_{\beta} \sum_{i=1}^n (y_i - \hat{y_i})$$
which is equivalent to the following in open form
$$\min_{β_0​,β_1\ldots\beta_p}=∑_{i=1}^n​(y_i​−β_0​−β_1​x_{i1}​−β_2​x_{i2}​\ldotsβ_{ip}​x_{ip}​)^2$$
Alternatively we can state the above as  
$$\hat{\beta} = (X^TX)^{-1}X^Ty$$
where $X$ is the design matrix, $y$ is the observed target vector, and $\hat{\beta}$ is the estimated coefficient vector. The result, $\hat{\beta}$, shows the closed form-matrix solution to the minimization above. 

In other words, the coefficient vector in ordinary least squares is obtained by minimizing the residual sum of squares (RSS), and solving that minimization gives the normal equation. 

If `X.shape=(n, m)`, then `beta.shape=(m,)`, the design matrix has $m$ columns, where each column corresponds to one coefficient, and the $\text{coeffient vector}$, has $m$ entries so that
$$\hat{\beta} = \begin{bmatrix} \beta_0 \\ \beta_1 \\ \beta_2 \\ \vdots \\ \beta_{m-1} \end{bmatrix}$$
If the intercept is not in X (like `sklearn` with `fit_intercept=True`), then `X.shape=(n,m)`, `coef_.shape == (m,)`, `intercept_` is separate. 

**Regularization**
The regulations below may be used due to correlation between the feature columns. Two or more feature columns may be correlated, carry similar information, or contain duplicate measures, due to transformations including polynomial features, basis function alterations, dummy variable redundancy, or real world issues. 

If the design matrix carries correlated values, it may yield unstable beta coefficients measured as large polar magnitudes between beta coefficients i.e. $\beta_1=99, \beta_2=-100$. This instability is measured as increased model variance, $\text{Var}(\hat{y_i})$. Large polar beta coefficients can be a sign of increased model variance. IF the

The regularization methods below penalize large beta coefficients by adding a coefficient penalty term to the minimization objective. Alternatively, regularization penalizes model complexity by adding a penalty term based on the magnitude of the beta coefficients to the loss function.

**Ridge Regression (L2 Regularization)**
$$_\text{basis form notation}$$
$$\min_{\beta_0, \beta_1} \sum_{i=1}^n(y_i - \beta_0 - \sum_{j=1}^p\beta_jx_{ij})^2_+\lambda\sum_{j=1}^p\beta^2_j$$

$$\min_\beta \sum_{i=1}^n(y_i-\hat{y_i})^2+\lambda\sum_{j=1}^p\beta^2_j$$

$$_\text{vector norm notation}$$
$$\min_\beta\parallel y-X\beta\parallel^2_2+\lambda\parallel\beta\parallel^2_2$$
`class sklearn.linear_model.Ridge(alpha=1.0, *, fit_intercept=True, copy_X=True, max_iter=None, tol=0.0001, solver='auto', positive=False, random_state=None)`
- Linear least squares with L2 regularization. Minimizes the objective function $||y - Xw||^2_2 + \alpha||w||^2_2$
- [*attribute*]  `coef_` : learned $\beta$ values 
- [*attribute*] `intercept_` : learned $\beta_0$ value 

Ridge regression (L2 regularization) adjusts larger $\hat{\beta}$ values by penalizing their sum of squares. The $\alpha$ parameter (noted as $\lambda$ above), represents the hyperparameter controlling the strength of the penalty applied wherein $\alpha=0$ recovers the original regression function, $(0)\sum_{j=1}^p\beta^2_j=0$, and $\alpha=\infty$ suppresses all model coefficients. Remember the larger the regularization value, the greater the suppressant as measured through, $\min_\beta$. Ridge regression is computationally efficient. 

**Lasso Regression (L1 Regression)**
$$_\text{basis form notation}$$
$$\min_{\beta_0,\beta_1\ldots\beta_p}\sum_{i=1}^n(y_i-\beta_0-\beta_1x_{i1}-\beta_2x_{i2})^2+\lambda(|\beta_1|+|\beta_2|+\ldots\beta_p|)$$
$$\min\sum_{i=1}^n(y_i-\hat{y_i})^2+\lambda\sum_{j=1}^p|\beta_j|$$
$$_\text{vector norm notation}$$
$$\min_\beta\parallel y-X\beta\parallel^2_2+\lambda\parallel\beta1\parallel_1$$
`class sklearn.linear_model.Lasso(alpha=1.0, *, fit_intercept=True, precompute=False, copy_X=True, max_iter=1000, tol=0.0001, warm_start=False, positive=False, random_state=None, selection='cyclic')`
- Linear Model trained with L1 prior as regularizer (aka the Lasso).
- The optimization objective for Lasso is $(\frac{1}{2 n_{samples}}) ||y - Xw||^2_2 + \alpha ||w||_1$, which is different from the equations above as `sklearn` scales the squared-error term by $\frac{1}{2n}$, which is a cleaner optimization form less dependant on the sample size

The lasso regression (L1 regularization) is similar to the L2 regularization except that it uses the L1 norm to penalize beta coefficients by summing their absolute value. Its construction tends to favour sparse models where possible. Lasso regression is computationally efficient. 
## CHP43: Support Vector Machines
**Linear SVM**
$$ _\text{decision boundary for sample i}$$

$$w^T x_i - b = 0$$
$$w_1x_{i1} + w_2x_{i2} + \ldots + w_px_{ip} -b = 0 $$
`class sklearn.svm.SVC(*, C=1.0, kernel='rbf', degree=3, gamma='scale', coef0=0.0, shrinking=True, probability=False, tol=0.001, cache_size=200, class_weight=None, verbose=False, max_iter=-1, decision_function_shape='ovr', break_ties=False, random_state=None)`
- `C` : margin parameter (this is a soft-margin SVM model)
- `kernel`: kernel type $\rightarrow$ {`‘linear’`, `‘poly’`, `‘rbf’`, `‘sigmoid’`, `‘precomputed’`} or `callable`, default=`’rbf’`$\rightarrow$ `rbf` is 'radial basis function' $\rightarrow$ transformation to higher feature space
- `degree` : int, default=3, if `'poly'`
- [*attribute*] `support_vectors_` : `ndarray` of shape (`n_SV`, `n_features`) support vectors (an empty array if kernel is precomputed)
- [*attribute*] `support_vectors_` : `ndarray` of shape (`n_SV`, `n_features`) support vectors (an empty array if kernel is precomputed)


Support vector machine (SVM) classifies data by maximizing two margins, either hyperplanes or lines, depending on the amount of features $n$, to divide the data into separate groups. Because an infinite set of hyperplanes can divide data, we optimize the function 
$$w^Tx_i-b=0$$
,where $w^T$ is the transpose matrix, $x_i$ are the data inputs and $b$ is the intercept subject to the 'margin' constraints,
$$w^Tx_i-b \ge 1, \quad \text{if } y_i = 1, \text{ or}$$ $$w^Tx_i-b \le -1, \quad \text{if } y_i = -1.$$
$$ = y_i(w^Tx_i-b) \ge1  \text{, for all, }i = 1,\ldots,n$$
Because, $\frac{2}{\parallel w \parallel }$, represents the margin width, we need to minimize $\parallel w \parallel$ because if $n= \text{margin width}$, then $\parallel w \parallel \uparrow n$. Subject to our constraints, the optimization is 

**hard-margin svm**
$$ \min_{w, b} \frac{1}{2} \parallel w \parallel^2 $$
$$\text{ s.t. } y_i(w^Tx_i+b) \ge 1\text{ for all } i=1,\ldots,n$$
The above represents the 'hard margin' SVM which does not allow any datapoints to fall within the margins, $y_i(w^Tx_i-b) \ge1  \text{, for all, }i = 1,\ldots,n$. The soft margin SVM controls a $C$ parameter, which determines the number of datapoints allowed within the margins on both sides.
  
![[SVM_margin.webp|419|359]]

**soft-margin svm**
$$ \min_{w, b, \xi} \frac{1}{2} \parallel w \parallel^2 + C \sum_{i=1}^n\xi_i$$$$\text{ s.t. } y_i(w^Tx_i+b) \ge 1-\xi_i\text{ for all } i=1,\ldots,n$$$$\text{and } \xi_i \ge \text{for all} i = 1,\ldots,n$$
where $\xi$ represents the slack variable defined as   
 -  $\xi_i = 0 \Rightarrow \text{correctly classified and outside/on the margin}$, 
 - $0 < \xi_i < 1 \Rightarrow \text{correctly classified but inside the margin}$, 
 - $\xi_i = 1 \Rightarrow \text{exactly on the decision boundary}$, 
 - $\xi_i > 1 \Rightarrow \text{misclassified}$, 
 
and $C$ controls the strength of penalty such that a high, $C \sum_{i=1}^n\xi_i$, no values are allowed inside the margin, and low $C \sum_{i=1}^n\xi_i$, many values are allowed inside the margin. 
 
**linear regression vs. svm**
Like linear regression, SVM also learns a weighted linear function, $In^Tx+b$. But instead of using that function to predict a continuous value, SVM uses it to classify points based on which side o decision boundary they fall on. The decision boundary is where $In^Tx+b = 0$, and the margin boundaries are usually represented by $In^Tx+b = 1$ and $In^Tx+b =-1$. SVM attempts to choose the weights which give the widest possible margin while soft-margin SVM allows points to fall into the margin or even be classified with a lower $C$ parameter. The default model in sklearn, `sklearn.svc.SVM` is a soft-margin model with a variable $C$, `C`, parameter best optimized through cross-fold validation. 

**kernel trick**
SVM is often paired with a with a transformation, particularly a kernel transformation which maps data to higher feature space so that data is more easily divisible. This can be don using the `kernel` parameter for `SVC`. 

## CHP44: Decision Tree Learning
For a classification dataset
$$\textit{D}= \{(x_i, y_i)\}_{i=1}^n \qquad x_i\in \mathbb{R}^p, \qquad y_i \in \{1, \ldots, K\}$$
a decision tree recursively partitions feature spaces into terminal regions

$$R_1, R_2, \ldots, R_3$$
and predicts
$$\hat{f}(x) = \sum_{m=1}^Mc_mI(x\in R_m)$$
where $I(\cdot)$ represents the piecewise indicator, $I=1$ for $x \in R_m$ for $m=i$ and $c_m$, is 
$$c_m = \arg \max_{k \in \{1, \ldots, K \}} \hat{p}_{mk}$$
and
$$\hat{p}_{mk} = \frac{1}{N_m}\sum_{x_i \in R_m} I(y_i =k)$$
where $R_m$ is a terminal leaf region, $N_m$ is the number of training samples in region $R_m$, $K$ is the number of classes and $\hat{p}_{mk}$ is the empirical proportion of class $k$ in leaf $m$. At each internal node $m$, the tree searches over feature-threshold split candidates of the form
$$R_L(j, t) = \{x_i \in R_m : x_{ij} \le t\}$$
$$R_R(j, t) = \{x_i \in R_m : x_{ij} > t\}$$
and choose the split
$$(j*, t*) \arg \min_{j, t} [\frac{N_L}{N_m}I(R_L) + \frac{N_R}{N_m}I(R_R)]$$
where $N_L$ and $N_R$ are the number of samples sent to the left and right child nodes and $I(\cdot)$ is the impurity function. For Gini impurity,
$$I(R_m) = G(R_m) = 1 - \sum_{k=1}^K \hat{p}^2_{mk}$$
where $\hat{p}_{mk}$ is the sample as above. Thus, the full criterion with Gini is
$$(j*, t*) \arg \min_{j, t} [\frac{N_L}{N_m}(1 - \sum_{k=1}^K \hat{p}^2_{Lk}) + \frac{N_R}{N_m}(1 - \sum_{k=1}^K \hat{p}^2_{Rk})]$$

`class sklearn.tree.DecisionTreeClassifier(*, criterion='gini', splitter='best', max_depth=None, min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_features=None, random_state=None, max_leaf_nodes=None, min_impurity_decrease=0.0, class_weight=None, ccp_alpha=0.0, monotonic_cst=None)`
- `criterion` : the type of purity metric used, `{“gini”, “entropy”, “log_loss”}, default=”gini”`
- `splitter`: the type of splitting algorithm, `{“best”, “random”}, default=”best”`; `"best"` is exhaustive greedy whereas `random` is just greedy. 

**overview**
Decision tree learning is a non parametric (does not assume a fixed functional form or distribution of the underlying distribution) supervised machine learning model, which splits data into successive branches (partitions) based on an optimization criterion. Decision tree learning can be configured varyingly. 

Because there are an infinite number of potential splits, and many possible solutions, the perfect solution is a computationally expensive global tree optimization, which is (usually) substituted for a greedy local solution (greedy algorithm).  

A decision tree classifier searches feature columns $j$ and candidate thresholds $t$, choosing the split $(j,t)$ that best reduces impurity. Each split partitions the current node into two regions, $R_L$ and $R_R$. As depth increases, the number of possible terminal regions can grow up to $2^d$, where $d$ is the tree depth i.e. grows exponentially with base two. Consider, no column $j$, needs to be split if it does not meet the decision criterion. In the limit, with unlimited tree depth, a decision tree splits a feature space until individual training samples are isolated into terminal leaves. In this sense, the decision tree can memorize the individual class labels which will overfit the data.

**splitting algorithm**
1. Overview: At each node, the splitting algorithm recommends candidate splits from which  the best option according to the decision criterion (i.e. $(j*, t*) \arg \min_{j, t} [\frac{N_L}{N_m}I(R_L) + \frac{N_R}{N_m}I(R_R)]$) is selected. The algorithm used to search candidates at each node of equal tree depth, is an exhaustive greedy algorithm. 
2. Exhaustive Greedy Algorithm: The greedy algorithm is local optimization solution, based on the travelling salesman problem, which chooses a random location to start and optimize the problem from there. For a decision tree this means searching all recommended candidates at node depth, $d$. The 'exhaustive' part means searching all available candidates and 'greedy' means you solve at equal node depth, $d$, as opposed to finding a global solution. 
3. Splitting Candidate Recommendation: Candidates are recommended based on midway points between distinct adjacent values. Note, the midway point is mathematically equal to average, $\frac{1}{n}\sum_{i=1}^n x_i$, but in cython computed as $\text{midpoint} = \frac{x_{prev}}{2} + \frac{x_{current}}{2}$, to avoid computational overflow. 
   
   Computational overflow results inefficient computation order i.e. if $x_{prev} = 1.7 \times 10^{308}$, $x_{current} = 1.7 \times 10^{308}$, $\Rightarrow$ $\frac{x_{prev}+{x_{current}}}{2}$, results in $\Rightarrow$ $\frac{\infty}{2}$ because floating point cannot hold values above $1.8 \times 10^{308}$, however if computed using the midpoint above, results in $0.85 \times 10^{308}+0.85 \times 10^{308}=1.7 \times 10^{308}$, which is computationally feasible. 
   
   For the list, $[1, 3, 5]$, there are two candidates, ($[2, 4]$), which means for any column with rows, $n$, the amount of candidates, $c$, are equal to $c_{\text{max candidates at a node}} = (n_{\text{node samples}} - 1) \times n_{\text{features considered}}$. In general, for one feature column at a node, the maximum number of candidate split thresholds is $n_{\text{node samples}}-1$. 
   
4. Feature shuffling: At each node split, sklearn randomly permutes the feature order, even when `splitter='best'`. If `max_features = a`, the model randomly considers `a` features at each split, then searches for the best valid split among those features. If `max_features = n_features`, the random order usually does not change the result unless multiple splits have identical impurity reduction, in which case the selected split can vary unless `random_state` is fixed. If `splitter='best'`, each selected feature column is sorted at the current node, valid split candidates are computed between consecutive distinct values, and the split with the greatest impurity reduction is chosen. If `splitter='random'`, sklearn chooses random threshold candidates for the selected features instead of exhaustively evaluating all midpoint candidates, then chooses the best split among those random candidates. Note, for `features= 'best'` the feature columns themselves are randomly permuted/shuffled at each node split. Then, for each selected feature, sklearn sorts the values within that feature column at that node and evaluates threshold candidates between sorted distinct values.

**ensemble methods**
An ensemble method averages a series of overfit decision tree classifiers to return a more stable and accurate result. One type of ensemble is called a bagging classifier, which can be used on any machine learning method. An ensemble of decision tree classifiers is called a random forest. 

**for continuous targets...**

**sklearn.tree.DecisionTreeRegressor**  
Decision tree model for predicting continuous values.

```python
#signature
sklearn.tree.DecisionTreeRegressor(*, criterion='squared_error', splitter='best', max_depth=None, min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_features=None, random_state=None, max_leaf_nodes=None, min_impurity_decrease=0.0, ccp_alpha=0.0, monotonic_cst=None)
```

**for ensembles...**

**sklearn.ensemble.RandomForestClassifier**  
Combines decision trees trained on bootstrap samples by default. Each split considers a random subset of features; predictions combine the trees’ class probabilities.
```python
#signature
sklearn.ensemble.RandomForestClassifier(n_estimators=100, *, criterion='gini', max_depth=None, min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_features='sqrt', max_leaf_nodes=None, min_impurity_decrease=0.0, bootstrap=True, oob_score=False, n_jobs=None, random_state=None, verbose=0, warm_start=False, class_weight=None, ccp_alpha=0.0, max_samples=None, monotonic_cst=None)
```

**sklearn.ensemble.RandomForestRegressor**  
Averages predictions from regression trees trained on bootstrap samples by default. Feature order is randomized at each split, but the default `max_features=1.0` considers all features.
```python
#signature
sklearn.ensemble.RandomForestRegressor(n_estimators=100, *, criterion='squared_error', max_depth=None, min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_features=1.0, max_leaf_nodes=None, min_impurity_decrease=0.0, bootstrap=True, oob_score=False, n_jobs=None, random_state=None, verbose=0, warm_start=False, ccp_alpha=0.0, max_samples=None, monotonic_cst=None)
```

**sklearn.ensemble.BaggingClassifier**  
General bagging wrapper for compatible classifiers. Fits copies on sampled data and combines their predictions.
```python
#signature
sklearn.ensemble.BaggingClassifier(estimator=None, n_estimators=10, *, max_samples=1.0, max_features=1.0, bootstrap=True, bootstrap_features=False, oob_score=False, warm_start=False, n_jobs=None, random_state=None, verbose=0)
```

**sklearn.ensemble.BaggingRegressor**  
General bagging wrapper for compatible regressors. Fits copies on sampled data and averages their predictions.
```python
#signature
sklearn.ensemble.BaggingRegressor(estimator=None, n_estimators=10, *, max_samples=1.0, max_features=1.0, bootstrap=True, bootstrap_features=False, oob_score=False, warm_start=False, n_jobs=None, random_state=None, verbose=0)
```
## CHP45: Principle Component Analysis
**Covariance/Eigenvalue Method: PCA without Singular Value Decomposition (SVD)**
Let:
$$X \in \mathbb{R}^{n \times p[}$$

where, $n = \text{number of observations}$, $p = \text{number of features}$, and $x_i \in \mathbb{R}^p$. Center the data with $\mu = \frac{1}{n}\sum_{i=i}^n x_i$, $\mu \in \mathbb{R}^p$, $$X_c = X - 1\mu^T$$where, $1 \in \mathbb{R}^n$, $X_c \in \mathbb{R}^{n \times p}$. Each feature column is shifted so that the mean is zero ($1\mu^T$ simply denotes the feature matrix with same shape as $X$, where we subtract the feature means from every row- it is equivalent to subtracting $\mu_p$ from each row value in feature $p$). Compute the covariance matrix, $$\Sigma = \frac{1}{n-1}X_c^TX_C$$where $\Sigma$ measures how the feature columns vary with each other. Since there are $p$ features, the covariance matrix is $p \times p$. The covariance matrix represents the pairwise  covariances between features as given below. $$\begin{bmatrix}\operatorname{Var}(X_1) & \operatorname{Cov}(X_1, X_2) & \cdots & \operatorname{Cov}(X_1, X_p) \\ \operatorname{Cov}(X_2, X_1) & \operatorname{Var}(X_2) & \cdots & \operatorname{Cov}(X_2, X_p) \\ \vdots & \vdots & \ddots & \vdots \\ \operatorname{Cov}(X_p, X_1) & \operatorname{Cov}(X_p, X_2) & \cdots & \operatorname{Var}(X_p)\end{bmatrix}$$ Solve the eigenvalue problem $$\Sigma v_j = \lambda_jv_j$$
where, $v_j \in \mathbb{R}^p$, and $j = 1, \cdots, p$ and $\lambda_j = \text{variance explained by } v_j$ and $|| v_j || = 1$. Each eigenvector $v_j$ is a principle component in each direction. Each eigenvalue $\lambda_j$ tells how much variance lies along that direction. Next we order the eigenvalues, $\lambda_1 \ge \lambda_2 \cdots \le \lambda_p$ and collect the first $k$ eigenvectors $$W_k = [v_1, v_2, \cdots, v_3]$$where $W_k \in \mathbb{R}^{p \times k}$. The largest eigenvectors correspond to the directions with the most variance. $W_k$ stores the top $k$ PCA directions. Lastly, we project the data $Z = X_cW_k$ where $Z \in \mathbb{R}^{n \times k}$. The original centred data is transformed from $p$ features into $k$ PCA coordinates $\rightarrow$ returns the data in the transformed PCA feature space  represented by $X_cW_k$. 

**PCA with SVD**
Start with the centered matrix as show above $X_c = X - 1\mu^T$ where $X_c \in \mathbb{R}^{n \times p}$ . Next, factor the centered data matrix $$X_c = U\Sigma_sV^T$$where $U \in \mathbb{R}^{n \times r}$, $\Sigma_s \in \mathbb{R}^{r \times r}$, $V \in \mathbb{R}^{p \times r}$ and $r = rank(X_c)$ (see the exponents for $r$ i.e. $U \in \mathbb{R}^{n \times r}$). 
SVD (singular value decomposition) decomposes the centred data matrix directly into three factors $\Rightarrow$  $$\text{observation coordinates} \times \text{scaling values} \times \text{feature-space directions}$$Effectively, for $X_c = U\Sigma_sV^T$ $= U \Sigma V^T$, 
- $V$ tells you the angles of the major and minor axes of that ellipse. 
- $\Sigma$ tells you the lengths of the major and minor axes (how long and how wide it is). 
- $U$ gives you the coordinates of each specific data point if the ellipse were perfectly centered and aligned with a standard graph.
The PCA direction are found from $V$ (not by explicitly forming the covariance matrix). The columns of $V$are the principle component directions. Each direction, $v_j$, is a(n) (orthogonal) unit vector, that lives in the original feature space $\mathbb{R}^p$, such that $v_j \in \mathbb{R}^p$, $$V = [v_1, v_2, \cdots, v_r]$$
Each direction vector is associated with a related length, $\sigma_j$ (the vectors, $v_j$ $\ne$ lengths, $\sigma_j$; these are two different measurements decomposed from $V^T$ and $\Sigma$, respectively). SVD automatically orders the lengths/scales
$$
\sigma_1 \ge \sigma_2 \ge \cdots \ge \sigma_r
$$
and each singular value $\sigma_j$ corresponds to a direction $v_j$ i.e. $(\sigma_1​,v1​),(\sigma_2​,v2​),\cdots,(\sigma_k​,vk​)$. We select the top $k$ largest lengths, $\sigma_k$, and their accompanied directions $v_k$ become the PCA components. Finally, we project the data, $Z = X_cV_k$ or equivalently, $$Z = U_k \Sigma_{s, k}$$where $Z \in \mathbb{R}^{n \times k}$. The top $k$ PCA components are the selected direction vectors in $V_k$. To project the original data into the lower-dimensional PCA coordinate system, multiply the centered data matrix $X_c$​ by these components $$Z=X_c​V_k​$$
where $Z$ is the transformed data with $k$ PCA coordinate columns. Effectively, we multiply our (unit) vector directions (unit vector means $origin(0, 0)$; which is why $X_c$ is centered to $zero$) by $X_c$ (the dot product of the coordinate direction and the centered matrix) i.e. for row $x_{c1} : [2, 1$] 
$$
x_{c,1} = [2, 1],
\qquad
v_1 =
\begin{bmatrix}
0.8 \\
0.6
\end{bmatrix}
$$
$$
z_1 = x_{c,1}v_1
=
[2, 1]
\begin{bmatrix}
0.8 \\
0.6
\end{bmatrix}
=
2(0.8) + 1(0.6)
=
2.2
$$
Each chosen direction vector, $v_j$, is orthogonal to the next vector such that $v_i \perp v_j$for $i \ne j$. This means for subsequent vectors, i.e. $v_1, v_2, \cdots, v_n$, the next vector, $v_{n+1}$ must be orthogonal to all the previous vectors i.e. $v_{\{1, \cdots, n\}} \perp v_{n+1}$. In order to for this to computed, each vector, $v_n$, is subsequently removed from the projection of $X_c$ onto $v_1$, leaving a residual matrix, $R_1, R_2, \cdots, R_n$ from which the remaining vectors are determined using the previous orthogonal constraints. You start by removing the projection of $X_c$ onto $v_1$, $$z_z = X_cv_1$$which gives you how much each data point lies on $v1$. Then, $$X_1 = z_1v_1^T$$This rebuilds the part of the data explained by $v_1$. Then subtract it, $$R_1 = X_c - X_1$$ so $R_1 = X_c - X_cv_1v_1^T$, or $$R_1 = X_c(I- v_1v_1^T)$$such that, "we remove the $v_1$​ direction information from the centered data- what remains is the residual data, and the next PCA direction is found from that leftover data with the orthogonal constrains built in i.e. $R_1v_1= 0$. 

`class sklearn.decomposition.PCA(n_components=None, *, copy=True, whiten=False, svd_solver='auto', tol=0.0, iterated_power='auto', n_oversamples=10, power_iteration_normalizer='auto', random_state=None)`
- uses SVD method
- `n_components`: number of component vectors to use

**overview**
PCA is decomposition method that acts as a transformation method in scikit-learn because it is technically used for dimensionality reduction. The intuition behind this method is given by the image below. 
![[ipy.general-1780371779601.webp|498]]
PCA works by projecting a dataset onto a series of orthogonal candidate vectors and then selecting those vectors which 'explain' the most amount of variance. Variance, $\sigma^2$ is measured as squared deviations from the mean, $(x_i - \mu_p)^2$. The reason all vectors are all individually orthogonal to one another so that we capture the most amount of unique variance from from the projected data. Without the orthogonal constraints, each subsequent $v_n$ will either be very close to $v_1$ or simply a linear function of $v_1$ i.e. $a v_1$ where $a$ is some constant. The orthogonal constraint, $v_i \perp v_j$ works because it ensures each subsequent candidate vector captures meaningful variation from the data. 

**sklearn.decomposition.PCA**  
Performs principal component analysis, projecting centered data onto directions of greatest variance.
```python
#signature
sklearn.decomposition.PCA(n_components=None, *, copy=True, whiten=False, svd_solver='auto', tol=0.0, iterated_power='auto', n_oversamples=10, power_iteration_normalizer='auto', random_state=None)
```
- `svd_solver` : Decomposition algorithm: `'auto'`, `'full'`, `'covariance_eigh'`, `'arpack'`, or `'randomized'`. Defaults to `'auto'`, which selects a solver based on the data shape and requested components.
## CHP46: Manifold Learning
Chapter omitted. 
## CHP47: K-Means Clustering
**standard K-means (Lloyd's algorithm)**
$$J = \sum^k_{j=1}\sum_{x_i \in C_j}\Vert x_i - \mu_j \Vert^2_2 $$
The goal is to minimize $j$. 1. Start by choose $k$ initial starting centroids, $$\mu_1, \mu_2, \ldots, \mu_k$$ Then 2 for each point $x_i$, assign it to the closest centroid, $$c_i = \arg \min_j \Vert x_i - \mu_j \Vert^2_2$$
where $\Vert x_i - \mu_j \Vert^2_2 = \sum^d_{r=1}(x_{ir}-\mu_{jr})^2$. Then 3. for every cluster $C_j$, replace its centroid with the mean of its assigned points, $$\mu_j = \frac{1}{|C_j|} \sum_{x_i \in C_j} x_i$$which coordinate-wise, looks like $\mu=(\frac{1}{n_j}\sum_ix_{i1},\frac{1}{n_j}\sum_ix_{i2},\ldots)$. Then 4. repeat the steps alternate $$\text{assign points}\rightarrow\text{recompute means}\rightarrow\text{assign points}\ldots$$
until the centroid stop changing: $\mu_j^{(t+1)} = \mu_j^{(t)}$ or the change becomes sufficiently small. K-means full pipeline looks like $$\text{nearest centroid}\rightarrow\text{mean of assigned points}\rightarrow{repeat}$$ where each iteration reduces/leaves unchanged the squared-error object $J$. 

**overview**
K-means is a popular clustering algorithm. There are different algorithmic versions of K-means which use varying heuristics for initial centroid selection. However, each K-means is some version of an $EM-\text{Expectation-Maximization}$:
1. Guess some cluster centres
2. Repeat until converged
 	a.  $\text{E-step}$: Assign points to the nearest cluster center
 	b.  $\text{M-step}$: Set the cluster centres to the mean of their assigned points. 
Based on the (standard) implementation of K-means, we update each centroid based on the average of the squared coordinate, and not the squared distance, as represented by the squared $\ell_2$ norm, $\Vert x\Vert_2^2 = (\sqrt{\sum^n_{i=1}|x_{i}|^2})^2 = \sum^n_{i=1}|x_{i}|^2$ (then averaged). Because we take the square, further away coordinates are technically 'valued' more as they contribute more to the average (distance weighting). 

**K-means circularity**
K-means uses squared euclidian distance to represent points which entails the assumed cluster shape to be circular. Distance from the centroid is is treated equally in every direction meaning that all points at distance $r$ from a centroid satisfy $\Vert x - \mu \Vert_2$, which in two dimensions defines a circle, and in higher dimensions a hypersphere (the standard equation of a circle, $(x-h)^2 + (y-k)^2 = r^2$ where $(h, k)$ is the center point of the circle and $r$ is the radius $\rightarrow$ with origin $(0, 0)$, the formula simplifies to $x^2 + y^2 = r^2$).   

**sklearn.cluster.KMeans**  
Partitions data into `n_clusters` groups by minimizing inertia—the sum of squared distances from each point to its nearest centroid.
```python
#signature
sklearn.cluster.KMeans(n_clusters=8, *, init='k-means++', n_init='auto', max_iter=300, tol=0.0001, verbose=0, random_state=None, copy_x=True, algorithm='lloyd')
```
- `init` : Initialization method: `'k-means++'`, `'random'`, a callable, or an array of initial centers with shape `(n_clusters, n_features)`. The default greedy k-means++ tests several candidates when selecting each new center and keeps the one producing the lowest inertia with the centers selected so far.

$$
J = \sum_{j=1}^{k}\sum_{x_i \in C_j}\lVert x_i-\mu_j\rVert_2^2
$$
Larger distances contribute disproportionately because they are squared.

## CHP48: Gaussian Mixture Models
GMM uses K-means for initial clusters assignments. Each centroid initializes a GMM component, which represents a Gaussian distribution, $\mathcal{N(\mu_k, \Sigma_k)}$ and has mixture weight, $\pi_k$ (mixture weight is the share of points assigned to each cluster/component i.e. if $\frac{40}{100}$ points belong to component $k_1$ then $\pi^{(0)}_k=0.4$). To begin, initialize a K-means, which gives initial hard assignments $\fbox{K chosen}$. From those assignments, retrieve, 
$$
\begin{gathered}
\pi^{(0)}_k = \frac{N_k}{n} \\
\mu^{(0)}_k = \frac{1}{N_k} \sum_{x_i \in C_k} x_i \\
\Sigma^{(0)}_k = \frac{1}{N_k}\sum_{x_i \in C_k}(x_i - \mu_k^{(0)})(x_i - \mu_k^{(0)})^T
\end{gathered}
$$
where $\pi_k^{(0)}$ is the initial mixture weight belonging to each component, $\mu^{(0)}_k$ is the mean of of gaussian component $k$, and $\Sigma^{(0)}_k$ is its variance. Next, we assign each point, $x_i$ to a gaussian component, $$p(x_i) = \mathcal{N}(x_i \mid \mu_k,  \Sigma_k)$$where $\mathcal{N}(x_i \mid \mu_k, \Sigma_k)$ is the gaussian distribution belonging to $k$. For every $x_i$ calculate membership probability in every component $$ \boxed{\gamma_{ik} = \frac{\pi_k\mathcal{N}(x_i\mid \mu_k, \Sigma_k)}{\sum^K_{j=1}\pi_j \mathcal{N}(x_i \mid \mu_j, \Sigma_j)}}$$
where $\gamma_{ik}$ is the probability that point $x_i$ belongs to Gaussian component $k$. Retrieve the effective membership,  $$N_k = \sum_i \gamma_{ik}$$and update the mixture weight, mean, and covariance respectively
$$
\begin{gathered}
\pi_k  = \frac{N_k}{n} \\
\mu_k  = \frac{\sum_i \gamma_{ik}x_i}{N_k} \\
\Sigma_k  =\frac{\sum_i \gamma_{ik} (x_i - \mu_k) (x_i-\mu_k)^T} {N_k}.
\end{gathered}
$$
Finally, evaluate likelihood if the likelihood has not converged and repeat 
$$
\begin{gathered}
\boxed{\mathcal{L} = \sum_{i=1}^n \log\left[\sum_{k=1}^K \pi_k \mathcal{N}(x_i \mid \mu_k, \Sigma_k)\right]} \\
\boxed{\text{E-Step} \rightarrow \text{M-Step} \rightarrow \text{likelihood} \rightarrow \text{repeat}}
\end{gathered}
$$
**overview**
GMM uses K-means for initial centroid assignment. The gaussian component, $\mathcal{N}(x_i \mid \mu_k, \Sigma_k)$ is equal to $$\boxed{\mathcal{N}(x_i \mid \mu_k, \Sigma_k) = \frac{1}{(2\pi)^{\frac{d}{2}}\vert \Sigma_k\vert^{\frac{1}{2}}\exp\left(-\frac{1}{2}(x_i-\mu_k)^T\Sigma_k^{-1}(x_i - \mu_k)\right)}}$$where, $\mu_k = \text{mean vector}$, $\Sigma_k = \text{covariance matrix}$ ($\vert\Sigma\vert = \text{the determinant of the covariance matrix}$), $d = \text{number of features (dimensions inside } x_i)$. The density estimation represents how likely $x_i$ is under $\mathcal{N}(x_i \mid \mu_k, \Sigma_k)$, and the weighted Gaussian density is $\pi_k\mathcal{N}(x_i \mid \mu_k, \Sigma_k)$. The initial centroid assignment is necessary to fill the values of each Gaussian component, $k$. 

GMM is a type of expectation-maximization algorithm. While K-means assigns each point $x_i$ with full certainty (hard-assignment), GMM assigns individual probabilities to each point (soft-assignment $\rightarrow$ see `predict_proba(X)` below). Because GMM generates probability distributions, you can sample it as a generative model to create output. 

**covariance type**
The covariance type parameter (`covariance_type` below) controls how the covariance matrix ( $\Sigma = \text{covariance matrix}$) is determined. 
 
1. Full covariance : Each component gets its own covariance. So each Gaussian can have its own size, elongation, and rotation. $$\begin{bmatrix} \sigma^2_1 & \operatorname{cov}_{12} \\  \operatorname{cov}_{21} & \sigma^{2}_{2} \end{bmatrix} $$
2. Tied covariance : All components share the same covariance matrix $\rightarrow$ the value is estimated using all components and all points while accounting accounting for each points responsibility to each component. Different centers, but the gaussians have the same basic shape and orientation.$$\begin{gathered} \Sigma_1 = \Sigma_2 = \ldots = \Sigma_k \\ \boxed{\Sigma = \frac{1}{n} \sum^K_{k=1} \sum^n_{i=1}\gamma_{ik}(x_i-\mu_k)(x_i-\mu_k)^T}  \end{gathered}$$
3. Diagonal : Each component gets its own covariance matrix, but off-diagonal covariances are forced to zero. Features can have difference variances, but no covariance. The Gaussian can stretch along axes, but cannot rotate.  $$\Sigma_k = \begin{bmatrix}\sigma^2_1 & 0 \\ 0 & \sigma^2_2 \end{bmatrix}$$
4. Spherical : Each component gets just one variance. Same spread in every direction, so the component is spherical. $$\Sigma_k = \sigma^2_kI$$

**sklearn.mixture.GaussianMixture**  
Fits a mixture of Gaussian distributions, supporting density estimation and probabilistic clustering.
```python
#signature
sklearn.mixture.GaussianMixture(n_components=1, *, covariance_type='full', tol=0.001, reg_covar=1e-06, max_iter=100, n_init=1, init_params='kmeans', weights_init=None, means_init=None, precisions_init=None, random_state=None, warm_start=False, verbose=0, verbose_interval=10)
```

- `n_components` : Number of Gaussian components, analogous to the number of clusters.
- `predict_proba(X)` : Returns each sample’s posterior probability of belonging to each component (responsibilities), with shape `(n_samples, n_components)`. Each row sums to 1.

## CHP49: Kernel Density Estimation
**KDE $\leftarrow$ Gaussian Kernel**
For observations $x_1, \ldots, x_n$, $$\boxed{\hat{f}_h(x) = \frac{1}{nh^d}\sum^n_{i=1}K(\frac{x-x_i}{h})}$$where, $n = \text{number of observations}$, $d=\text{number of features (dimensions)}$, $h = \text{bandwidth}$, $K = \text{kernel function}$, $x_i = \text{observed data point}$, $x = \text{location where density is being estimated}$. Substituting the Gaussian kernel $\rightarrow$ KDE, $$\boxed{\hat{f}_h(x)=\frac{1}{n(2\pi)^\frac{d}{2}h^d} \sum^n_{i=1}\exp\left(-\frac{\Vert{x-x_i}\Vert^2}{2h^2}\right)}$$
or for 1D $$\boxed{\hat{f}h(x) = \frac{1}{nh\sqrt{2\pi}}\sum^n_{i=1}\exp\left(-\frac{(x-x_i)^2}{2h^2}\right)}$$so conceptually $$\fbox{KDE density at x = average density contributed by all kernels at k}$$
**overview**
KDE estimates an unknown probability distribution by assigning a kernel to each data point and then summing values (effectively extending the idea of GMM by giving each data point a component). Any Kernel can be used to create an estimate. The following can be thought of as follows:
- $x_i \rightarrow$ contributes a small probability bump centered on itself
- $K \rightarrow$ the function or shape which each point takes on 
- $h \rightarrow$ the width of the kernel function
- at any location, $x$, KDE adds the contribution from all of the bumps $\rightarrow$ forming a smooth estimate
The bandwidth, $h$, is the most important parameter, $$\begin{aligned}h\downarrow &\Rightarrow \text{narrow, highly local kernels} \\ h\uparrow &\Rightarrow \text{wide, smoothyer kernels}\end{aligned} $$
Consider, a density estimate works by determining the concentration of points to one another. Hence the bandwidth is measure of what points are closest i.e. only points 'less than' some distance are added together. 

**KDE and histograms**
KDE can be used for visualizing data in substitution for when histograms are inaccurate or unstable based on their binning parameter. Histograms can be unreliable if the binning parameter changes as histograms simply count the number of values falling between equal divisions. 

**sklearn.neighbors.KernelDensity**  
Estimates a probability density using kernel density estimation.
```python
#signature
sklearn.neighbors.KernelDensity(*, bandwidth=1.0, algorithm='auto', kernel='gaussian', metric='euclidean', atol=0, rtol=0, breadth_first=True, leaf_size=40, metric_params=None)
```

- `bandwidth` : Kernel width; larger values produce smoother density estimates.
- `algorithm` : Tree algorithm: `'kd_tree'`, `'ball_tree'`, or `'auto'`. Defaults to automatic selection.
- `atol` : Absolute tolerance. Larger values can speed up computation at the cost of accuracy.
- `rtol` : Relative tolerance. Larger values can speed up computation at the cost of accuracy.

---
## More...
- sklearn.metrics stuff
- sklearn.metrics.confusion_matrix
- GridSearchCV
- sklearn.metrics.accuracy_score

---
# APPENDIX A
Tabular information for API reference and input params and codes for the given computing libraries, modules or submodules. 

---
## Table 1: Numpy Supported Datatypes
| Data type    | Description                                                                  |
| ------------ | ---------------------------------------------------------------------------- |
| `bool_`      | Boolean (True or False) stored as a byte                                     |
| `int_`       | Default integer type (same as C `long`; typically `int64` on 64-bit systems) |
| `intc`       | Identical to C `int` (typically `int32` or `int64`)                          |
| `intp`       | Integer used for indexing (same as C `ssize_t`)                              |
| `int8`       | Byte (−128 to 127)                                                           |
| `int16`      | Integer (−32,768 to 32,767)                                                  |
| `int32`      | Integer (−2,147,483,648 to 2,147,483,647)                                    |
| `int64`      | Integer (−9,223,372,036,854,775,808 to 9,223,372,036,854,775,807)            |
| `uint8`      | Unsigned integer (0 to 255)                                                  |
| `uint16`     | Unsigned integer (0 to 65,535)                                               |
| `uint32`     | Unsigned integer (0 to 4,294,967,295)                                        |
| `uint64`     | Unsign*ed integer (*0 to 18,446,744,073,709,551,615)                         |
| `float_`     | Shorthand for `float64`                                                      |
| `float16`    | Half-precision float (1 sign bit, 5 exponent bits, 10 mantissa bits)         |
| `float32`    | Single-precision float (1 sign bit, 8 exponent bits, 23 mantissa bits)       |
| `float64`    | Double-precision float (1 sign bit, 11 exponent bits, 52 mantissa bits)      |
| `complex_`   | Shorthand for `complex128`                                                   |
| `complex64`  | Complex number, two 32-bit floats                                            |
| `complex128` | Complex number, two 64-bit floats                                            |

---
## Table 2: Ufunc Arithmetic Operators
| Operator | Equivalent ufunc  | Description                   |
| -------- | ----------------- | ----------------------------- |
| `+`      | `np.add`          | Addition (e.g. `a + b`)       |
| `-`      | `np.subtract`     | Subtraction (e.g. `a - b`)    |
| `*`      | `np.multiply`     | Multiplication (e.g. `a * b`) |
| `/`      | `np.divide`       | True division                 |
| `//`     | `np.floor_divide` | Floor division                |
| `**`     | `np.power`        | Exponentiation                |
| `%`      | `np.mod`          | Modulus (remainder)           |
| `-a`     | `np.negative`     | Unary negation                |
| `+a`     | `np.positive`     | Unary plus                    |

---
## Table 3: Numpy Aggregation Functions
Aggregations functions are convenience wrappers around`numpy.ufunc.reduce()`. They can be expressed as either `array.aggregation` or as 'pure functions', `np.sum(array_1, array_2)`.

| Function name   | NaN-safe version   | Description                               |
| --------------- | ------------------ | ----------------------------------------- |
| `np.sum`        | `np.nansum`        | Compute sum of elements                   |
| `np.prod`       | `np.nanprod`       | Compute product of elements               |
| `np.mean`       | `np.nanmean`       | Compute mean of elements                  |
| `np.std`        | `np.nanstd`        | Compute standard deviation                |
| `np.var`        | `np.nanvar`        | Compute variance                          |
| `np.min`        | `np.nanmin`        | Find minimum value                        |
| `np.max`        | `np.nanmax`        | Find maximum value                        |
| `np.argmin`     | `np.nanargmin`     | Find index of minimum value               |
| `np.argmax`     | `np.nanargmax`     | Find index of maximum value               |
| `np.median`     | `np.nanmedian`     | Compute median of elements                |
| `np.percentile` | `np.nanpercentile` | Compute rank-based statistics of elements |
| `np.any`        | N/A                | Evaluate whether any elements are true    |
| `np.all`        | N/A                | Evaluate whether all elements are true    |

---
## Table 4: Ufunc Comparison Operators
| Operator | Equivalent ufunc   | Description                                   |
| -------- | ------------------ | --------------------------------------------- |
| `==`     | `np.equal`         | Element-wise equality comparison              |
| `!=`     | `np.not_equal`     | Element-wise inequality comparison            |
| `<`      | `np.less`          | Element-wise less-than comparison             |
| `<=`     | `np.less_equal`    | Element-wise less-than-or-equal comparison    |
| `>`      | `np.greater`       | Element-wise greater-than comparison          |
| `>=`     | `np.greater_equal` | Element-wise greater-than-or-equal comparison |

---
## Table 5: Ufunc Bitwise Operators
| Operator | Equivalent ufunc | Description              |
| -------- | ---------------- | ------------------------ |
| `&`      | `np.logical_and` | Element-wise logical AND |
| `\|`     | `np.logical_or`  | Element-wise logical OR  |
| `^`      | `np.logical_xor` | Element-wise logical XOR |
| `~`      | `np.logical_not` | Element-wise logical NOT |

---
## Table 6: Numpy Array Protocol Type Strings
 "The first character specifies the kind of data and the remaining characters specify the number of bytes per item, except for Unicode, where it is interpreted as the number of characters, and except b1 which represents boolean. The item size must correspond to an existing type, or an error will be raised (see [numpy.org](https://numpy.org/doc/stable/reference/arrays.dtypes.html#specifying-and-constructing-data-types:))
 
 Numpy primitive scalar `dtypes` are limited to at most 8 bytes per scalar value. Only exception are complex numbers and strings, which may exceed 8 bytes, but not as a single numeric primitive. 

| Character(s)  | Description                             | Typical sizes          |
| ------------- | --------------------------------------- | ---------------------- |
| `'?'`, `'b1'` | boolean                                 | `?` / `b1` (1 byte)    |
| `'b'`         | (signed) byte                           | `b1` (1 byte)          |
| `'B'`         | unsigned byte                           | `u1` (1 byte)          |
| `'i'`         | (signed) integer                        | `i1`, `i2`, `i4`, `i8` |
| `'u'`         | unsigned integer                        | `u1`, `u2`, `u4`, `u8` |
| `'f'`         | floating-point                          | `f2`, `f4`, `f8`       |
| `'c'`         | complex-floating point                  | `c8`, `c16`            |
| `'m'`         | timedelta                               | `m8[unit]`             |
| `'M'`         | datetime                                | `M8[unit]`             |
| `'O'`         | (Python) objects                        | pointer-sized          |
| `'S'`, `'a'`  | zero-terminated bytes (not recommended) | `S1`, `S10`, …         |
| `'U'`         | Unicode string                          | `U1`, `U10`, …         |
| `'V'`         | raw data (void)                         | `Vn` (n bytes)         |

---
## Table 7: Pandas `dtype` String Aliases

| Category            | String aliases                                                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| TZ-aware datetime   | `'datetime64[ns, <tz>]'`                                                                                                       |
| Categorical         | `'category'`                                                                                                                   |
| Period (time spans) | `'period[<freq>]'`, `'Period[<freq>]'`                                                                                         |
| Sparse              | `'Sparse'`, `'Sparse[int]'`, `'Sparse[float]'`                                                                                 |
| Interval            | `'interval'`, `'Interval'`, `'Interval[<numpy_dtype>]'`, `'Interval[datetime64[ns, <tz>]]'`, `'Interval[timedelta64[<freq>]]'` |
| Nullable integers   | `'Int8'`, `'Int16'`, `'Int32'`, `'Int64'`, `'UInt8'`, `'UInt16'`, `'UInt32'`, `'UInt64'`                                       |
| Nullable floats     | `'Float32'`, `'Float64'`                                                                                                       |
| Strings             | `'string'`                                                                                                                     |
| Boolean (with NA)   | `'boolean'`                                                                                                                    |

---
## Table 8: Pandas Aggregation Methods
The following are all methods of `DataFrame` and `Series` objects. 

| Aggregation Returns | Description                     |
|---------------------|---------------------------------|
| .count()            | Total number of items           |
| .first(), .last()   | First and last item             |
| .mean(), .median()  | Mean and median                 |
| .min(), .max()      | Minimum and maximum             |
| .std(), .var()      | Standard deviation and variance |
| .mad()              | Mean absolute deviation         |
| .prod()             | Product of all items            |
| .sum()              | Sum of all items                |

---
## Table 9: Pandas Time Frequency Codes
Pandas uses frequency codes to denote unit of time intervals for `freq`,  `periods`, etc. parameters for the time objects `Timestamp`, `Period`, `Timedelta` and their related index objects. The codes represent the period ending and starting by the appended letter `E`and `S` respectively. Codes below are based on Pandas 2.3.1. See [Pandas Date offsets](https://pandas.pydata.org/docs/reference/offset_frequency.html?utm_source=chatgpt.com) for more. Additionally, frequency codes can be appended with a custom month to designate the interval period starting i.e. `Y-DEC` or `W-MON`. 

| Date Offset                              | Frequency String | Description                                           |
| ---------------------------------------- | ---------------- | ----------------------------------------------------- |
| DateOffset                               | None             | Generic offset class, defaults to absolute 24 hours   |
| BDay or BusinessDay                      | 'B'              | Business day (weekday)                                |
| CDay or CustomBusinessDay                | 'C'              | Custom business day                                   |
| Week                                     | 'W'              | One week, optionally anchored on a day of the week    |
| WeekOfMonth                              | 'WOM'            | The x-th day of the y-th week of each month           |
| LastWeekOfMonth                          | 'LWOM'           | The x-th day of the last week of each month           |
| MonthEnd                                 | 'ME'             | Calendar month end                                    |
| MonthBegin                               | 'MS'             | Calendar month begin                                  |
| BMonthEnd or BusinessMonthEnd            | 'BME'            | Business month end                                    |
| BMonthBegin or BusinessMonthBegin        | 'BMS'            | Business month begin                                  |
| CBMonthEnd or CustomBusinessMonthEnd     | 'CBME'           | Custom business month end                             |
| CBMonthBegin or CustomBusinessMonthBegin | 'CBMS'           | Custom business month begin                           |
| SemiMonthEnd                             | 'SME'            | 15th (or other day_of_month) and calendar month end   |
| SemiMonthBegin                           | 'SMS'            | 15th (or other day_of_month) and calendar month begin |
| QuarterEnd                               | 'QE'             | Calendar quarter end                                  |
| QuarterBegin                             | 'QS'             | Calendar quarter begin                                |
| BQuarterEnd                              | 'BQE'            | Business quarter end                                  |
| BQuarterBegin                            | 'BQS'            | Business quarter begin                                |
| FY5253Quarter                            | 'REQ'            | Retail (aka 52-53 week) quarter                       |
| HalfYearEnd                              | 'HYE'            | Calendar half year end                                |
| HalfYearBegin                            | 'HYS'            | Calendar half year begin                              |
| BHalfYearEnd                             | 'BHYE'           | Business half year end                                |
| BHalfYearBegin                           | 'BHYS'           | Business half year begin                              |
| YearEnd                                  | 'YE'             | Calendar year end                                     |
| YearBegin                                | 'YS'             | Calendar year begin                                   |
| BYearEnd                                 | 'BYE'            | Business year end                                     |
| BYearBegin                               | 'BYS'            | Business year begin                                   |
| FY5253                                   | 'RE'             | Retail (aka 52-53 week) year                          |
| Easter                                   | None             | Easter holiday                                        |
| BusinessHour                             | 'bh'             | Business hour                                         |
| CustomBusinessHour                       | 'cbh'            | Custom business hour                                  |
| Day                                      | 'D'              | One calendar day                                      |
| Hour                                     | 'h'              | One hour                                              |
| Minute                                   | 'min'            | One minute                                            |
| Second                                   | 's'              | One second                                            |
| Milli                                    | 'ms'             | One millisecond                                       |
| Micro                                    | 'us'             | One microsecond                                       |
| Nano                                     | 'ns'             | One nanosecond                                        |

---
## Table 10: Pandas Time Format Codes
Format codes derived from `datetime` module to reformat time objects to custom formats. 

| Code | Meaning         | Example  |
| ---- | --------------- | -------- |
| `%Y` | 4-digit year    | 2026     |
| `%y` | 2-digit year    | 26       |
| `%m` | Month (01–12)   | 02       |
| `%B` | Full month name | February |
| `%b` | Abbrev month    | Feb      |
| `%d` | Day of month    | 19       |
| `%A` | Full weekday    | Thursday |
| `%a` | Abbrev weekday  | Thu      |
| `%H` | Hour (24h)      | 13       |
| `%I` | Hour (12h)      | 01       |
| `%M` | Minute          | 45       |
| `%S` | Second          | 09       |

---

## Table 11: Matplotlib Line Style Codes
Part of `Line2D` interface; `plt.plt(marker='')`. 

| Code     | Keyword     | Description              |
| -------- | ----------- | ------------------------ |
| `'-'`    | `'solid'`   | Continuous unbroken line |
| `'--'`   | `'dashed'`  | Regular dashed line      |
| `'-.'`   | `'dashdot'` | Alternating dash and dot |
| `':'`    | `'dotted'`  | Series of dots           |
| `'None'` | `'None'`    | No line drawn            |
| `''`     | `''`        | No line drawn            |

---
## Table 12: Matplotlib Colour Codes
Universal access for matplotlib objects. 

| Color Code             | Type          | Description                 | Example Usage             |
| ---------------------- | ------------- | --------------------------- | ------------------------- |
| `'b'`                  | Short code    | Blue                        | `color='b'`               |
| `'g'`                  | Short code    | Green                       | `color='g'`               |
| `'r'`                  | Short code    | Red                         | `color='r'`               |
| `'c'`                  | Short code    | Cyan                        | `color='c'`               |
| `'m'`                  | Short code    | Magenta                     | `color='m'`               |
| `'y'`                  | Short code    | Yellow                      | `color='y'`               |
| `'k'`                  | Short code    | Black                       | `color='k'`               |
| `'w'`                  | Short code    | White                       | `color='w'`               |
| `'blue'`               | Named color   | Standard CSS color name     | `color='blue'`            |
| `'orange'`             | Named color   | Standard CSS color name     | `color='orange'`          |
| `'purple'`             | Named color   | Standard CSS color name     | `color='purple'`          |
| `'#FF5733'`            | Hex code      | RGB hex format              | `color='#FF5733'`         |
| `'#1f77b4'`            | Hex code      | Default Matplotlib blue     | `color='#1f77b4'`         |
| `(0.2, 0.4, 0.6)`      | RGB tuple     | Values between 0–1          | `color=(0.2,0.4,0.6)`     |
| `(0.2, 0.4, 0.6, 0.5)` | RGBA tuple    | Includes alpha transparency | `color=(0.2,0.4,0.6,0.5)` |
| `'0.5'`                | Grayscale     | String float between 0–1    | `color='0.5'`             |
| `'tab:blue'`           | Tableau color | Default Matplotlib palette  | `color='tab:blue'`        |
| `'tab:orange'`         | Tableau color | Default Matplotlib palette  | `color='tab:orange'`      |
| `'tab:green'`          | Tableau color | Default Matplotlib palette  | `color='tab:green'`       |
| `'tab:red'`            | Tableau color | Default Matplotlib palette  | `color='tab:red'`         |
| `'tab:purple'`         | Tableau color | Default Matplotlib palette  | `color='tab:purple'`      |

---
## Table 13: Matplotlib Scatter Markers
Param :  `plt.plot(marker='')` or `plt.scatter(marker='')`, `fmt`

| Marker | Shape            | Description            |
| ------ | ---------------- | ---------------------- |
| `'.'`  | Point            | Smallest marker        |
| `','`  | Pixel            | Single pixel marker    |
| `'o'`  | Circle           | Default scatter marker |
| `'v'`  | Triangle Down    | ▼                      |
| `'^'`  | Triangle Up      | ▲                      |
| `'<'`  | Triangle Left    | ◀                      |
| `'>'`  | Triangle Right   | ▶                      |
| `'1'`  | Tri Down (thin)  | Thin triangle          |
| `'2'`  | Tri Up (thin)    | Thin triangle          |
| `'3'`  | Tri Left (thin)  | Thin triangle          |
| `'4'`  | Tri Right (thin) | Thin triangle          |
| `'s'`  | Square           | ■                      |
| `'p'`  | Pentagon         | ⬟                      |
| `'*'`  | Star             | ★                      |
| `'h'`  | Hexagon 1        | Hexagon                |
| `'H'`  | Hexagon 2        | Alternate hexagon      |
| `'+'`  | Plus             | +                      |
| `'x'`  | X                | x                      |
| `'D'`  | Diamond          | ◆                      |
| `'d'`  | Thin Diamond     | Thin diamond           |
| `'     | '`               | Vertical Line          |
| `'_'`  | Horizontal Line  | ―                      |

---
## Table 14: Matplotlib Legend Location Codes
Param : `plt.legend(loc='')`.

| Location String      | Location Code |
| -------------------- | ------------: |
| `'best'` (Axes only) |             0 |
| `'upper right'`      |             1 |
| `'upper left'`       |             2 |
| `'lower left'`       |             3 |
| `'lower right'`      |             4 |
| `'right'`            |             5 |
| `'center left'`      |             6 |
| `'center right'`     |             7 |
| `'lower center'`     |             8 |
| `'upper center'`     |             9 |
| `'center'`           |            10 |

---
## Table 15: Matplotlib FancyArrowPatch Codes
Param: `arrowstyle`. 

| Class         | Name      | Parameters                                                                                                                      |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Curve         | `-`       | None                                                                                                                            |
| CurveA        | `<-`      | head_length=0.4, head_width=0.2, widthA=1.0, widthB=1.0, lengthA=0.2, lengthB=0.2, angleA=0, angleB=0, scaleA=None, scaleB=None |
| CurveB        | `->`      | head_length=0.4, head_width=0.2, widthA=1.0, widthB=1.0, lengthA=0.2, lengthB=0.2, angleA=0, angleB=0, scaleA=None, scaleB=None |
| CurveAB       | `<->`     | head_length=0.4, head_width=0.2, widthA=1.0, widthB=1.0, lengthA=0.2, lengthB=0.2, angleA=0, angleB=0, scaleA=None, scaleB=None |
| CurveFilledA  | `<\|-`    | head_length=0.4, head_width=0.2, widthA=1.0, widthB=1.0, lengthA=0.2, lengthB=0.2, angleA=0, angleB=0, scaleA=None, scaleB=None |
| CurveFilledB  | `-\|>`    | head_length=0.4, head_width=0.2, widthA=1.0, widthB=1.0, lengthA=0.2, lengthB=0.2, angleA=0, angleB=0, scaleA=None, scaleB=None |
| CurveFilledAB | `<\|-\|>` | head_length=0.4, head_width=0.2, widthA=1.0, widthB=1.0, lengthA=0.2, lengthB=0.2, angleA=0, angleB=0, scaleA=None, scaleB=None |
| BracketA      | `]-`      | widthA=1.0, lengthA=0.2, angleA=0                                                                                               |
| BracketB      | `-[`      | widthB=1.0, lengthB=0.2, angleB=0                                                                                               |
| BracketAB     | `]-[`     | widthA=1.0, lengthA=0.2, angleA=0, widthB=1.0, lengthB=0.2, angleB=0                                                            |
| BarAB         | `\|-\|`   | widthA=1.0, angleA=0, widthB=1.0, angleB=0                                                                                      |
| BracketCurve  | `]->`     | widthA=1.0, lengthA=0.2, angleA=None                                                                                            |
| CurveBracket  | `<-[`     | widthB=1.0, lengthB=0.2, angleB=None                                                                                            |
| Simple        | `simple`  | head_length=0.5, head_width=0.5, tail_width=0.2                                                                                 |
| Fancy         | `fancy`   | head_length=0.4, head_width=0.4, tail_width=0.4                                                                                 |
| Wedge         | `wedge`   | tail_width=0.3, shrink_factor=0.5                                                                                               |

---
## Table 16: Matplotlib rcParams
You can easily access all params 

| rcParams                           | rcParams                         | rcParams                           | rcParams                           |
| ---------------------------------- | -------------------------------- | ---------------------------------- | ---------------------------------- |
| _internal.classic_mode             | agg.path.chunksize               | animation.bitrate                  | animation.codec                    |
| animation.convert_args             | animation.convert_path           | animation.embed_limit              | animation.ffmpeg_args              |
| animation.ffmpeg_path              | animation.frame_format           | animation.html                     | animation.writer                   |
| axes.autolimit_mode                | axes.axisbelow                   | axes.edgecolor                     | axes.facecolor                     |
| axes.formatter.limits              | axes.formatter.min_exponent      | axes.formatter.offset_threshold    | axes.formatter.use_locale          |
| axes.formatter.use_mathtext        | axes.formatter.useoffset         | axes.grid                          | axes.grid.axis                     |
| axes.grid.which                    | axes.labelcolor                  | axes.labelpad                      | axes.labelsize                     |
| axes.labelweight                   | axes.linewidth                   | axes.prop_cycle                    | axes.spines.bottom                 |
| axes.spines.left                   | axes.spines.right                | axes.spines.top                    | axes.titlecolor                    |
| axes.titlelocation                 | axes.titlepad                    | axes.titlesize                     | axes.titleweight                   |
| axes.titley                        | axes.unicode_minus               | axes.xmargin                       | axes.ymargin                       |
| axes.zmargin                       | axes3d.automargin                | axes3d.grid                        | axes3d.mouserotationstyle          |
| axes3d.trackballborder             | axes3d.trackballsize             | axes3d.xaxis.panecolor             | axes3d.yaxis.panecolor             |
| axes3d.zaxis.panecolor             | backend                          | backend_fallback                   | boxplot.bootstrap                  |
| boxplot.boxprops.color             | boxplot.boxprops.linestyle       | boxplot.boxprops.linewidth         | boxplot.capprops.color             |
| boxplot.capprops.linestyle         | boxplot.capprops.linewidth       | boxplot.flierprops.color           | boxplot.flierprops.linestyle       |
| boxplot.flierprops.linewidth       | boxplot.flierprops.marker        | boxplot.flierprops.markeredgecolor | boxplot.flierprops.markeredgewidth |
| boxplot.flierprops.markerfacecolor | boxplot.flierprops.markersize    | boxplot.meanline                   | boxplot.meanprops.color            |
| boxplot.meanprops.linestyle        | boxplot.meanprops.linewidth      | boxplot.meanprops.marker           | boxplot.meanprops.markeredgecolor  |
| boxplot.meanprops.markerfacecolor  | boxplot.meanprops.markersize     | boxplot.medianprops.color          | boxplot.medianprops.linestyle      |
| boxplot.medianprops.linewidth      | boxplot.notch                    | boxplot.patchartist                | boxplot.showbox                    |
| boxplot.showcaps                   | boxplot.showfliers               | boxplot.showmeans                  | boxplot.vertical                   |
| boxplot.whiskerprops.color         | boxplot.whiskerprops.linestyle   | boxplot.whiskerprops.linewidth     | boxplot.whiskers                   |
| contour.algorithm                  | contour.corner_mask              | contour.linewidth                  | contour.negative_linestyle         |
| date.autoformatter.day             | date.autoformatter.hour          | date.autoformatter.microsecond     | date.autoformatter.minute          |
| date.autoformatter.month           | date.autoformatter.second        | date.autoformatter.year            | date.converter                     |
| date.epoch                         | date.interval_multiples          | docstring.hardcopy                 | errorbar.capsize                   |
| figure.autolayout                  | figure.constrained_layout.h_pad  | figure.constrained_layout.hspace   | figure.constrained_layout.use      |
| figure.constrained_layout.w_pad    | figure.constrained_layout.wspace | figure.dpi                         | figure.edgecolor                   |
| figure.facecolor                   | figure.figsize                   | figure.frameon                     | figure.hooks                       |
| figure.labelsize                   | figure.labelweight               | figure.max_open_warning            | figure.raise_window                |
| figure.subplot.bottom              | figure.subplot.hspace            | figure.subplot.left                | figure.subplot.right               |
| figure.subplot.top                 | figure.subplot.wspace            | figure.titlesize                   | figure.titleweight                 |
| font.cursive                       | font.family                      | font.fantasy                       | font.monospace                     |
| font.sans-serif                    | font.serif                       | font.size                          | font.stretch                       |
| font.style                         | font.variant                     | font.weight                        | grid.alpha                         |
| grid.color                         | grid.linestyle                   | grid.linewidth                     | hatch.color                        |
| hatch.linewidth                    | hist.bins                        | image.aspect                       | image.cmap                         |
| image.composite_image              | image.interpolation              | image.interpolation_stage          | image.lut                          |
| image.origin                       | image.resample                   | interactive                        | keymap.back                        |
| keymap.copy                        | keymap.forward                   | keymap.fullscreen                  | keymap.grid                        |
| keymap.grid_minor                  | keymap.help                      | keymap.home                        | keymap.pan                         |
| keymap.quit                        | keymap.quit_all                  | keymap.save                        | keymap.xscale                      |
| keymap.yscale                      | keymap.zoom                      | legend.borderaxespad               | legend.borderpad                   |
| legend.columnspacing               | legend.edgecolor                 | legend.facecolor                   | legend.fancybox                    |
| legend.fontsize                    | legend.framealpha                | legend.frameon                     | legend.handleheight                |
| legend.handlelength                | legend.handletextpad             | legend.labelcolor                  | legend.labelspacing                |
| legend.loc                         | legend.markerscale               | legend.numpoints                   | legend.scatterpoints               |
| legend.shadow                      | legend.title_fontsize            | lines.antialiased                  | lines.color                        |
| lines.dash_capstyle                | lines.dash_joinstyle             | lines.dashdot_pattern              | lines.dashed_pattern               |
| lines.dotted_pattern               | lines.linestyle                  | lines.linewidth                    | lines.marker                       |
| lines.markeredgecolor              | lines.markeredgewidth            | lines.markerfacecolor              | lines.markersize                   |
| lines.scale_dashes                 | lines.solid_capstyle             | lines.solid_joinstyle              | macosx.window_mode                 |
| markers.fillstyle                  | mathtext.bf                      | mathtext.bfit                      | mathtext.cal                       |
| mathtext.default                   | mathtext.fallback                | mathtext.fontset                   | mathtext.it                        |
| mathtext.rm                        | mathtext.sf                      | mathtext.tt                        | patch.antialiased                  |
| patch.edgecolor                    | patch.facecolor                  | patch.force_edgecolor              | patch.linewidth                    |
| path.effects                       | path.simplify                    | path.simplify_threshold            | path.sketch                        |
| path.snap                          | pcolor.shading                   | pcolormesh.snap                    | pdf.compression                    |
| pdf.fonttype                       | pdf.inheritcolor                 | pdf.use14corefonts                 | pgf.preamble                       |
| pgf.rcfonts                        | pgf.texsystem                    | polaraxes.grid                     | ps.distiller.res                   |
| ps.fonttype                        | ps.papersize                     | ps.useafm                          | ps.usedistiller                    |
| savefig.bbox                       | savefig.directory                | savefig.dpi                        | savefig.edgecolor                  |
| savefig.facecolor                  | savefig.format                   | savefig.orientation                | savefig.pad_inches                 |
| savefig.transparent                | scatter.edgecolors               | scatter.marker                     | svg.fonttype                       |
| svg.hashsalt                       | svg.id                           | svg.image_inline                   | text.antialiased                   |
| text.color                         | text.hinting                     | text.hinting_factor                | text.kerning_factor                |
| text.latex.preamble                | text.parse_math                  | text.usetex                        | timezone                           |
| tk.window_focus                    | toolbar                          | webagg.address                     | webagg.open_in_browser             |
| webagg.port                        | webagg.port_retries              | xaxis.labellocation                | xtick.alignment                    |
| xtick.bottom                       | xtick.color                      | xtick.direction                    | xtick.labelbottom                  |
| xtick.labelcolor                   | xtick.labelsize                  | xtick.labeltop                     | xtick.major.bottom                 |
| xtick.major.pad                    | xtick.major.size                 | xtick.major.top                    | xtick.major.width                  |
| xtick.minor.bottom                 | xtick.minor.ndivs                | xtick.minor.pad                    | xtick.minor.size                   |
| xtick.minor.top                    | xtick.minor.visible              | xtick.minor.width                  | xtick.top                          |
| yaxis.labellocation                | ytick.alignment                  | ytick.color                        | ytick.direction                    |
| ytick.labelcolor                   | ytick.labelleft                  | ytick.labelright                   | ytick.labelsize                    |
| ytick.left                         | ytick.major.left                 | ytick.major.pad                    | ytick.major.right                  |
| ytick.major.size                   | ytick.major.width                | ytick.minor.left                   | ytick.minor.ndivs                  |
| ytick.minor.pad                    | ytick.minor.right                | ytick.minor.size                   | ytick.minor.visible                |
| ytick.minor.width                  | ytick.right                      |                                    |                                    |


---

# APPENDIX B
Excursus on various theories pertaining to the given computing library or module. Additionally, contains information on concepts related to given libraries or modules. 

---
## Numpy Random Generator
The Numpy Random `Generator` is the primary (pseudo) random sampling class. It uses an internal BitGenerator to produce random values. The `Generator` class object exposes various methods for sampling distributions. 

Use `numpy.random.default_rng(seed=None)` constructor to return a `Generator` class to which the following methods apply. 

*Simple Random Functions*

| Function                                       | Description                                                                                                                                       |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `integers(low[, high, size, dtype, endpoint])` | Return random integers from `low` (inclusive) to `high` (exclusive). If `endpoint=True`, the interval is `low` (inclusive) to `high` (inclusive). |
| `random([size, dtype, out])`                   | Return random floats drawn from the half-open interval `[0.0, 1.0)`.                                                                              |
| `choice(a[, size, replace, p, axis, shuffle])` | Generate a random sample from a given array or array-like object.                                                                                 |
| `bytes(length)`                                | Return random bytes of the specified length.                                                                                                      |

---
## Scalar logic
This section explores how `True` and `False` values resolve depending on the boolean operator. These expressions represent the semantic product of the syntactic bitwise operators in the numpy library (i.e. `and` is `&`). 

*Boolean Operators*
These are python boolean operators and do not work element-wise on arrays as they are not part of the numpy library.

| Operator | Name     | Meaning                   | Components | Function                  |
| -------- | -------- | ------------------------- | ---------- | ------------------------- |
| `not`    | NOT      | Invert truth              | Unary      | Inverts truth             |
| `and`    | AND      | Both must be true         | Binary     | Detects agreement         |
| `or`     | OR       | At least one must be true | Binary     | Detects presence of truth |
| `xor`    | EXCL. OR | Only one can be true      | Binary     | Detects disagreement      |
*Truth Table*

| A   | B   | `A and B` | `A or B` | `A ^ B` | `not A` |
| --- | --- | --------- | -------- | ------- | ------- |
| T   | T   | T         | T        | F       | F       |
| T   | F   | F         | T        | T       | F       |
| F   | T   | F         | T        | T       | T       |
| F   | F   | F         | F        | F       | T       |

---
## Split-Apply-Combine Strategy

<div style="background:rgba(10,10,12,.35); color:rgba(245,255,252,.95); padding:12px 14px; border:1px solid rgba(255,255,255,.14); border-radius:14px; font-family:Arial, Helvetica, sans-serif; backdrop-filter:blur(14px) saturate(1.2); -webkit-backdrop-filter:blur(14px) saturate(1.2); box-shadow:0 0 0 1px rgba(255,255,255,.05) inset, 0 12px 30px rgba(0,0,0,.30);">
<div style="font-weight:800; margin-bottom:6px;">Split-Apply-Combine Strategy</div>
<div style="line-height:1.55; color:rgba(235,255,252,.88);">This strategy, popularized by Wickham (2011), is commonly referred to as the <strong>groupby</strong> method (e.g., in SQL and Pandas). It works by splitting data according to a key and grouping table rows that share the same key value.  
<p>
In Pandas, <code>groupby</code> is a method native to <code>pd.Series</code> and <code>pd.DataFrame</code> objects. Calling <code>.groupby()</code> does not immediately compute results; instead, it returns a <code>GroupBy</code> object, which acts as a lazy iterator that supports aggregation, transformation, and filtering operations before the final combine step.</p>
</div>
</div>

![[dshb_fig21-2.png ]]

---
## 64-Bit-TimeSpan Tradeoff
A comparison of the amount of time relative to the time encoding format. 

| Code | Meaning     | Max Time span (relative) | Max Time span (absolute) |
| ---- | ----------- | ------------------------ | ------------------------ |
| Y    | Year        | ± 9.2e18 years           | [9.2e18 BC, 9.2e18 AD]   |
| M    | Month       | ± 7.6e17 years           | [7.6e17 BC, 7.6e17 AD]   |
| W    | Week        | ± 1.7e17 years           | [1.7e17 BC, 1.7e17 AD]   |
| D    | Day         | ± 2.5e16 years           | [2.5e16 BC, 2.5e16 AD]   |
| h    | Hour        | ± 1.0e15 years           | [1.0e15 BC, 1.0e15 AD]   |
| m    | Minute      | ± 1.7e13 years           | [1.7e13 BC, 1.7e13 AD]   |
| s    | Second      | ± 2.9e12 years           | [2.9e9 BC, 2.9e9 AD]     |
| ms   | Millisecond | ± 2.9e9 years            | [2.9e6 BC, 2.9e6 AD]     |
| us   | Microsecond | ± 2.9e6 years            | [290301 BC, 294241 AD]   |
| ns   | Nanosecond  | ± 292 years              | [1678 AD, 2262 AD]       |
| ps   | Picosecond  | ± 106 days               | [1969 AD, 1970 AD]       |
| fs   | Femtosecond | ± 2.6 hours              | [1969 AD, 1970 AD]       |
| as   | Attosecond  | ± 9.2 seconds            | [1969 AD, 1970 AD]       |

---
## Raster Graphics
A raster graphic is a digital image made up of individual pixels, where each pixel stores an RGB tuple. An RGB tuple is a three-value structure, `(R, G, B)`, with each value typically expressed as a float between `0` and `1`, representing the proportion of red, green, and blue in the final composite colour.

Each value indicates how much of that primary colour is present. For example, the tuple `(0.85, 0.25, 0.25)` represents a bright but slightly muted red: red = 85%, green = 25%, and blue = 25%.

Raster graphics, which are plotted with `plt.imshow()`, take a two-or-three-dimensional array. For `x = np.arange(6).reshape(2,3)`, $\rightarrow$ `plt.imshow(x)` normalizes each ``x`` value to $[0, 1] ~ \text{with}~ \rightarrow u=\frac{x-vmin}{vmax-vmin}$, where $vmin$ and $vmax$ are the minimum and maximum values in `x`. The results for $x =[0, 1, 2, 3, 4, 5]$, where  $vmin=0$, $vmax=5$  $\Rightarrow u=\frac{0-0}{5}, \frac{1-0}{5}, \frac{2-0}{5}...\frac{5-5}{5}$, are passed to `plt.cm.color_map(u)` where `u` represents a  fractional position along a colormap,`cmap`, i.e. `plt.cm.viridis(0.5)` $\text{yielding} ~ \rightarrow$ RGB tuple `(0.127568, 0.566949, 0.550556, 1.0)` which is a greenish-blue.

Note, because RGB tuples are interpreted by colormaps in matplotlib, image data may be represented as float-point values accompanied by a corresponding colormap. 

![[plt.imshow(x).png]]
