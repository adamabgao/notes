
---
# Contents
[[#1. Introduction]]
- [[#Preface]]
- [[#Testing Frameworks]]

[[#2. Data Structures]]
- [[#Data Structures]]
- [[#Array Type Codes]]
- [[#Operator Support]]
- [[#Special Operators]]

[[#3. Data Model]]
- [[#Special Methods]]
- [[#Special Attributes]]

[[#4. Syntax]]
- [[#Keywords]]
- [[#Scopes & Enclosures]]
- [[#Slicing]]
-  [[#`str` Escape Sequences]]
- [[#`str` Literal Prefixes]]
- [[#Format Specification Mini-Language]]
- [[#Legacy Formats]]
 
 [[#APPENDIX]]
 -  [[#Functions]]
	 - [[#Built-In Functions]]
	 -  [[#Standard Library Functions]]
	 - [[#3rd-Party Functions]]
 - [[#Built-in Methods]]
 - [[#Error Map]]
---
# 1. Introduction

### Preface
- **Paradigm & Typing:** Multi-paradigm (OOP, procedural, functional, imperative, declarative, metaprogramming). Interpreted (CPython, PyPy, Jython, IronPython, MicroPython). Dynamically typed, duck-typed, strongly typed (no implicit type coercion).
- **Execution & Performance:** Interpreted, platform-independent (Windows, macOS, Linux, iOS, Android). GIL restricts true multithreading (multiprocessing for parallelism). Automatic garbage collection (reference counting + cycle detection). JIT via PyPy.
- **Extensibility & Interoperability:** C/C++ (CPython, ctypes, Cython), Java (Jython), .NET/C# (IronPython), R (rpy2), Fortran (f2py). Python can extend or embed within other applications.
- **Concurrency & Asynchronous Execution:** AsyncIO for non-blocking operations. Threading (GIL-limited), multiprocessing for true parallelism.
- **Memory & Runtime Management:** Automatic garbage collection. Supports manual memory management (`gc` module).
- **General-Purpose Use Cases:** Web dev (Django, Flask), data science (Pandas, NumPy, TensorFlow, PyTorch), automation (scripting, DevOps, CI/CD), cybersecurity (Scapy, Pwntools), embedded systems (Raspberry Pi, MicroPython), game dev (Pygame, Panda3D), blockchain (cryptography, smart contracts), database management (SQLAlchemy, SQLite).
- **Standard Library & Ecosystem:** Extensive built-in modules (`os`, `sys`, `json`, `datetime`). 450K+ third-party packages (PyPI).
- **Syntax & Readability:** Indentation-based, human-readable, high-level abstraction. Secure (`hashlib`, `secrets`).
- **Conclusion:** Python is a **multi-paradigm, dynamically typed, high-level, interpreted, cross-platform, general-purpose language** with strong extensibility, interoperability, memory management, and a vast ecosystem. Versatile across domains.### OVERVIEW: Object Oriented Programming
- **Core Principles:** Encapsulation (data hiding, controlled access via methods), Abstraction (exposing only essential details, hiding complexity), Inheritance (hierarchical code reuse, avoids duplication), Polymorphism (interchangeable object behavior, method overriding, method overloading).
- **Design Philosophy:** Models real-world entities via objects (state + behavior). Promotes modularity, reusability, scalability, and maintainability.
- **Advantages:** Code modularity (self-contained objects), reusability (class inheritance, object composition), flexibility (polymorphism, abstraction), maintainability (structured hierarchy, decoupling), security (encapsulation).
- **Disadvantages:** Increased complexity (planning required), performance overhead (dynamic dispatch, memory use), overengineering risk (unnecessary hierarchies), limited fit (not ideal for procedural, functional, or low-level applications).
### Testing Frameworks
- **Testing frameworks** are used to manage how programs handle operate handling errors and other issues they encounter. There are many different testing frameworks and testing methods. 
- **Unit tests** are automated tests that verify the smallest functional parts of a program, typically individual functions or methods. They ensure that each unit of code behaves as expected in isolation, without dependencies on external systems like databases or APIs. By writing unit tests, developers can catch bugs early, improve code maintainability, and facilitate refactoring with confidence. In Python, unit testing is commonly done using frameworks like `unittest` or `pytest`, which provide tools to define test cases, assert expected outcomes, and run tests efficiently. Well-written unit tests contribute to robust, reliable software and streamline the development process.
	- Unit testing in `pytest` involves writing test cases to verify individual functions or components in isolation. To implement a unit test, you import the function from the program you want to test and create a separate test file. The test file should start with `test_`, and the function inside the test file should also start with `test_` so that `pytest` can automatically discover and run the tests. Without these naming conventions, tests won’t be detected automatically, and you’ll have to manually specify the test file or function when running `pytest`, such as `pytest my_tests.py` or `pytest test_file.py::custom_test_function`. Inside the test function, you call the imported function with sample inputs, store the result, and use `assert` to compare it with the expected output. For example, if you have a function `def add(a, b): return a + b` in `math_operations.py`, a corresponding test file `test_math_operations.py` would contain `from math_operations import add` followed by `def test_add(): assert add(2, 3) == 5`. Running `pytest` in the terminal will execute all test functions that follow this structure. `pytest` is typically run from the terminal. After installing it with `pip install pytest`, navigate to the directory containing your test files and run `pytest`. By default, `pytest` automatically discovers test files and functions that follow the `test_` naming convention. To run a specific test file, use `pytest test_file.py`, or to run a specific test function within a file, use `pytest test_file.py::test_function_name`.
- **Assertions** can take many different forms. For instance, commonly used assertions in tests include assert two values are equal (`assert a == b`), that two values are not equal (`assert a != b`), that `a` evaluates to to `True` (`assert a`), that `a` evaluates to `False` (`assert not a`), that an element is in a list (`assert element in list`), that an element is not in a list (`assert element not in list`). These are just a few example, as technically anything can be expressed as a conditional statement can be used in a test. 
---
# 2. Data Structures

## Data Structures

| Data Structure | Immutable | Hashable | Indexable                                     | Duplicatable | Supports Other Data Structures |
| -------------- | --------- | -------- | --------------------------------------------- | ------------ | ------------------------------ |
| `list`         | No        | No       | Yes                                           | Yes          | Any                            |
| `tuple`        | Yes       | Yes*     | Yes                                           | Yes          | Any                            |
| `set`          | No        | No       | No                                            | No           | Hashable Types Only            |
| `frozenset`    | Yes       | Yes      | No                                            | No           | Hashable Types Only            |
| `dict`         | No        | No       | Keys: No [unless w/`items()`] Values: Yes<br> | No           | Keys: Hashable, Values: Any    |
| `str`          | Yes       | Yes      | Yes                                           | Yes          | N/A                            |
| `bytes`        | Yes       | Yes      | Yes                                           | Yes          | N/A                            |
| `bytearray`    | No        | No       | Yes                                           | Yes          | N/A                            |
| `int`          | Yes       | Yes      | N/A                                           | N/A          | N/A                            |
| `float`        | Yes       | Yes      | N/A                                           | N/A          | N/A                            |
| `complex`      | Yes       | Yes      | N/A                                           | N/A          | N/A                            |

| Data Structure | Special Information                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Tuple          | *Hashability of `tuple` depends on whether all its elements are hashable.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Dict           | Dict iteration can be tricky. KEYS are **NOT** indexable, but *key-values* ARE. Generally, dicts i.e. **hashables** are constructed using structured nested dicts in a main dict so that values can be looped by exploiting the same key name a loop i.e. `for key in dict:` `print(key['nested_key1]['nested_key2'])`. This structure allows you to iterate dictionary key-values. However, if the structure of the nested dictionaries is not equal, then the values cannot be accessed. |
| Array          | The array is a lean C array. An array of float values does not fold full `float` instances, but only the packed bytes representing their machine values. You provide the typecode when creating an array to determine the underlying C type used to store each item in the array. Arrays are part of the standard library i.e. `import array as array` or just `array.array()`. An array is optimal compared to a list if it holds only numbers.                                           |
## Array Type Codes

| Type Code | C Type             | Python Type       | Minimum size in bytes |
| --------- | ------------------ | ----------------- | --------------------- |
| `'b'`     | signed char        | int               | 1                     |
| `'B'`     | unsigned char      | int               | 1                     |
| `'u'`     | wchar_t            | Unicode character | 2                     |
| `'h'`     | signed short       | int               | 2                     |
| `'H'`     | unsigned short     | int               | 2                     |
| `'i'`     | signed int         | int               | 2                     |
| `'I'`     | unsigned int       | int               | 2                     |
| `'l'`     | signed long        | int               | 4                     |
| `'L'`     | unsigned long      | int               | 4                     |
| `'q'`     | signed long long   | int               | 8                     |
| `'Q'`     | unsigned long long | int               | 8                     |
| `'f'`     | float              | float             | 4                     |
| `'d'`     | double             | float             | 8                     |
## Operator Support

| Data Structure | Operations Support                                                                                                                                                                                     |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `list`         | Supports concatenation (`+` → `[1,2] + [3] = [1,2,3]`), repetition (`*` → `[1]*3 = [1,1,1]`), element-wise operations require looping                                                                  |
| `tuple`        | Supports concatenation (`+` → `(1,2) + (3,) = (1,2,3)`), repetition (`*` → `(1,)*3 = (1,1,1)`)                                                                                                         |
| `set`          | Supports union (`\|` → `{1,2} \| {3} = {1,2,3}`), intersection (`&` → `{1,2,3} & {2,3} = {2,3}`), difference (`-` → `{1,2,3} - {2} = {1,3}`), symmetric difference (`^` → `{1,2} ^ {2,3} = {1,3}`) \|` |
| `frozenset`    | Supports union (`\|`), intersection (`&`), difference (`-`), symmetric difference (`^`) \|                                                                                                             |
| `dict`         | Supports key-value updates (`update()`), dictionary merging.                                                                                                                                           |
| `str`          | Supports concatenation (`+` → `"a" + "b" = "ab"`), repetition (`*` → `"a"*3 = "aaa"`), slicing (`[:]`), character access (`str[index]`)                                                                |
| `bytes`        | Supports concatenation (`+` → `b"abc" + b"def" = b"abcdef"`), repetition (`*`), slicing (`[:]`), index access (`bytes[index]`)                                                                         |
| `bytearray`    | Supports concatenation (`+`), repetition (`*`), slicing (`[:]`), and mutable in-place modifications (`bytearray[index] = value`)                                                                       |
| `int`          | Supports arithmetic (`+`, `-`, `*`, `/`, `//`, `%`, `**`), bitwise operations (`&`, `                                                                                                                  |
| `float`        | Supports arithmetic (`+`, `-`, `*`, `/`, `//`, `%`, `**`)                                                                                                                                              |
| `complex`      | Supports arithmetic (`+`, `-`, `*`, `/`, `**`), real and imaginary component manipulation (`complex.real`, `complex.imag`)                                                                             |
## Special Operators

| Special<br>Operator      | Type     | Description                                                                                                                                                                                                    | Example                                        |
| ------------------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| `variable := expression` | Operator | Assigns a value to a variable as part of an expression. Useful to avoid repetition and improve readability by combining assignment and evaluation in one step. Introduced in Python 3.8 (aka Walrus Operator). | `if (n := len(my_list)) > 5: print(n)`         |
| `&`                      | Operator | Bitwise AND operator. Returns 1 in each bit position where both operands have 1.                                                                                                                               | `x = 5 & 3  # Result is 1 (0101 & 0011)`       |
| `\|`                     | Operator | Bitwise OR operator. Returns 1 in each bit position where at least one operand has 1.                                                                                                                          | `x = 5 \| 3  # Result is 7 (0101 \| 0011)`     |
| `\|=`                    | Operator | Bitwise OR assignment operator. Performs bitwise OR and assigns the result to the left operand.                                                                                                                | `x = 5; x \|= 3  # x becomes 7 (0101 \| 0011)` |
| `%`                      | Operator | Modulus operator. Returns the remainder after division.                                                                                                                                                        | `x = 10 % 3  # Result is 1`                    |
| `^`                      | Operator | Bitwise XOR operator. Returns 1 in each bit position where only one operand has 1.                                                                                                                             | `x = 5 ^ 3  # Result is 6 (0101 ^ 0011)`       |

---
# 3. Data Model

## Special Methods

| Special Method       | Operator                       | Usage/Description                            |
| -------------------- | ------------------------------ | -------------------------------------------- |
| `__new__`            | `Class(...)`                   | Creates a new instance.                      |
| `__init__`           | `Class(...)`                   | Initializes a new instance.                  |
| `__del__`            | Object destruction             | Finalizes an object before destruction.      |
| `__repr__`           | `repr(obj)`                    | Returns developer/debug representation.      |
| `__str__`            | `str(obj)`, `print(obj)`       | Returns user-facing representation.          |
| `__bytes__`          | `bytes(obj)`                   | Returns byte representation.                 |
| `__format__`         | `format(obj, spec)`, f-strings | Controls formatted representation.           |
| `__lt__`             | `<`                            | Implements less-than comparison.             |
| `__le__`             | `<=`                           | Implements less-than-or-equal comparison.    |
| `__eq__`             | `==`                           | Implements equality comparison.              |
| `__ne__`             | `!=`                           | Implements inequality comparison.            |
| `__gt__`             | `>`                            | Implements greater-than comparison.          |
| `__ge__`             | `>=`                           | Implements greater-than-or-equal comparison. |
| `__hash__`           | `hash(obj)`                    | Returns hash value.                          |
| `__bool__`           | `bool(obj)`                    | Defines truth value.                         |
| `__getattr__`        | `obj.name`                     | Handles missing attributes.                  |
| `__getattribute__`   | `obj.name`                     | Intercepts all attribute access.             |
| `__setattr__`        | `obj.name = value`             | Intercepts attribute assignment.             |
| `__delattr__`        | `del obj.name`                 | Intercepts attribute deletion.               |
| `__dir__`            | `dir(obj)`                     | Defines listed attributes.                   |
| `__get__`            | `obj.name`                     | Descriptor attribute retrieval.              |
| `__set__`            | `obj.name = value`             | Descriptor attribute assignment.             |
| `__delete__`         | `del obj.name`                 | Descriptor attribute deletion.               |
| `__set_name__`       | Class creation                 | Gives descriptor its assigned name.          |
| `__init_subclass__`  | Class inheritance              | Customizes subclass creation.                |
| `__mro_entries__`    | Class inheritance              | Supplies replacement base classes.           |
| `__prepare__`        | Class creation                 | Creates the class namespace.                 |
| `__instancecheck__`  | `isinstance(obj, cls)`         | Customizes instance checks.                  |
| `__subclasscheck__`  | `issubclass(cls, base)`        | Customizes subclass checks.                  |
| `__class_getitem__`  | `Class[key]`                   | Supports generic class subscription.         |
| `__call__`           | `obj(...)`                     | Makes an instance callable.                  |
| `__len__`            | `len(obj)`                     | Returns object length.                       |
| `__length_hint__`    | `operator.length_hint(obj)`    | Estimates iterable length.                   |
| `__getitem__`        | `obj[key]`                     | Retrieves an item or slice.                  |
| `__setitem__`        | `obj[key] = value`             | Assigns an item or slice.                    |
| `__delitem__`        | `del obj[key]`                 | Deletes an item or slice.                    |
| `__missing__`        | `dict_subclass[key]`           | Handles missing dictionary keys.             |
| `__iter__`           | `iter(obj)`, `for`             | Returns an iterator.                         |
| `__next__`           | `next(obj)`                    | Returns next iterator item.                  |
| `__reversed__`       | `reversed(obj)`                | Provides reverse iteration.                  |
| `__contains__`       | `in`, `not in`                 | Implements membership testing.               |
| `__add__`            | `+`                            | Implements addition.                         |
| `__sub__`            | `-`                            | Implements subtraction.                      |
| `__mul__`            | `*`                            | Implements multiplication.                   |
| `__matmul__`         | `@`                            | Implements matrix multiplication.            |
| `__truediv__`        | `/`                            | Implements true division.                    |
| `__floordiv__`       | `//`                           | Implements floor division.                   |
| `__mod__`            | `%`                            | Implements modulo.                           |
| `__divmod__`         | `divmod()`                     | Implements quotient and remainder.           |
| `__pow__`            | `**`, `pow()`                  | Implements exponentiation.                   |
| `__lshift__`         | `<<`                           | Implements left bit shift.                   |
| `__rshift__`         | `>>`                           | Implements right bit shift.                  |
| `__and__`            | `&`                            | Implements bitwise AND.                      |
| `__xor__`            | `^`                            | Implements bitwise XOR.                      |
| `__or__`             | `\|`                           | Implements bitwise OR.                       |
| `__radd__`           | `other + obj`                  | Implements reflected addition.               |
| `__rsub__`           | `other - obj`                  | Implements reflected subtraction.            |
| `__rmul__`           | `other * obj`                  | Implements reflected multiplication.         |
| `__rmatmul__`        | `other @ obj`                  | Implements reflected matrix multiplication.  |
| `__rtruediv__`       | `other / obj`                  | Implements reflected true division.          |
| `__rfloordiv__`      | `other // obj`                 | Implements reflected floor division.         |
| `__rmod__`           | `other % obj`                  | Implements reflected modulo.                 |
| `__rdivmod__`        | `divmod(other, obj)`           | Implements reflected `divmod`.               |
| `__rpow__`           | `other ** obj`                 | Implements reflected exponentiation.         |
| `__rlshift__`        | `other << obj`                 | Implements reflected left shift.             |
| `__rrshift__`        | `other >> obj`                 | Implements reflected right shift.            |
| `__rand__`           | `other & obj`                  | Implements reflected bitwise AND.            |
| `__rxor__`           | `other ^ obj`                  | Implements reflected bitwise XOR.            |
| `__ror__`            | `other \| obj`                 | Implements reflected bitwise OR.             |
| `__iadd__`           | `+=`                           | Implements in-place addition.                |
| `__isub__`           | `-=`                           | Implements in-place subtraction.             |
| `__imul__`           | `*=`                           | Implements in-place multiplication.          |
| `__imatmul__`        | `@=`                           | Implements in-place matrix multiplication.   |
| `__itruediv__`       | `/=`                           | Implements in-place true division.           |
| `__ifloordiv__`      | `//=`                          | Implements in-place floor division.          |
| `__imod__`           | `%=`                           | Implements in-place modulo.                  |
| `__ipow__`           | `**=`                          | Implements in-place exponentiation.          |
| `__ilshift__`        | `<<=`                          | Implements in-place left shift.              |
| `__irshift__`        | `>>=`                          | Implements in-place right shift.             |
| `__iand__`           | `&=`                           | Implements in-place bitwise AND.             |
| `__ixor__`           | `^=`                           | Implements in-place bitwise XOR.             |
| `__ior__`            | `\|=`                          | Implements in-place bitwise OR.              |
| `__neg__`            | `-obj`                         | Implements unary negation.                   |
| `__pos__`            | `+obj`                         | Implements unary positive.                   |
| `__abs__`            | `abs(obj)`                     | Returns absolute value/magnitude.            |
| `__invert__`         | `~obj`                         | Implements bitwise inversion.                |
| `__complex__`        | `complex(obj)`                 | Converts to `complex`.                       |
| `__int__`            | `int(obj)`                     | Converts to `int`.                           |
| `__float__`          | `float(obj)`                   | Converts to `float`.                         |
| `__index__`          | Integer contexts               | Provides exact integer representation.       |
| `__round__`          | `round(obj)`                   | Implements rounding.                         |
| `__trunc__`          | `math.trunc(obj)`              | Truncates toward zero.                       |
| `__floor__`          | `math.floor(obj)`              | Returns mathematical floor.                  |
| `__ceil__`           | `math.ceil(obj)`               | Returns mathematical ceiling.                |
| `__enter__`          | `with`                         | Enters a context manager.                    |
| `__exit__`           | `with`                         | Exits a context manager.                     |
| `__buffer__`         | `memoryview(obj)`              | Exposes the buffer protocol.                 |
| `__release_buffer__` | Buffer release                 | Releases exported buffer resources.          |
| `__annotate__`       | Lazy annotations               | Evaluates annotations on demand.             |
| `__await__`          | `await obj`                    | Makes an object awaitable.                   |
| `__aiter__`          | `async for`                    | Returns an asynchronous iterator.            |
| `__anext__`          | `anext(obj)`                   | Returns next async iterator item.            |
| `__aenter__`         | `async with`                   | Asynchronously enters a context.             |
| `__aexit__`          | `async with`                   | Asynchronously exits a context.              |
| `__fspath__`         | `os.fspath(obj)`               | Returns filesystem path representation.      |

## Special Attributes


| **Function Type**                | **Description**                                                                        | **Example Syntax**                            |
| -------------------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------- |
| **User-Defined Functions**       | Functions created by the programmer using `def`.                                       | `def my_func(): ...`                          |
| **Anonymous Functions (Lambda)** | Functions without a name, created using `lambda` keyword. One-line, single-expression. | `lambda x: x + 1`                             |
| **Built-in Functions**           | Functions provided by Python by default (no need to import).                           | `len()`, `print()`, `sum()`                   |
| **Higher-Order Functions**       | Functions that take other functions as arguments or return functions.                  | `map()`, `filter()`, `sorted()`               |
| **Recursive Functions**          | Functions that call themselves.                                                        | `def factorial(n): return n * factorial(n-1)` |
| **Generator Functions**          | Functions that yield values one at a time using `yield` instead of `return`.           | `def gen(): yield x`                          |
| **Coroutine Functions**          | Special generator-like functions for asynchronous code, defined with `async def`.      | `async def coro(): await something()`         |
| **Instance Methods**             | Functions defined inside a class, operating on class instances.                        | `def method(self): ...`                       |
| **Class Methods**                | Functions bound to the class, taking `cls` as first parameter.                         | `@classmethod`                                |
| **Static Methods**               | Methods without `self` or `cls`, defined using `@staticmethod`.                        | `@staticmethod`                               |

---
# 4. Syntax

## Keywords

| Keywords   | Description                                                                                                     |
| ---------- | --------------------------------------------------------------------------------------------------------------- |
| `False`    | Represents the boolean value false.                                                                             |
| `None`     | Represents the absence of a value or a null value.                                                              |
| `True`     | Represents the boolean value true.                                                                              |
| `and`      | A logical operator used to combine conditional statements; returns true if both statements are true.            |
| `as`       | Used to create an alias when importing a module.                                                                |
| `assert`   | For debugging purposes; tests if a condition is true.                                                           |
| `async`    | Declares an asynchronous function.                                                                              |
| `await`    | Waits for the result of an asynchronous function.                                                               |
| `break`    | Terminates the current loop. Must be `for`, `while`, or `nested-type` loop.                                     |
| `class`    | Declares a class.                                                                                               |
| `continue` | Skips the current iteration of a loop and proceeds to the next iteration.                                       |
| `def`      | Declares a function or method.                                                                                  |
| `del`      | Deletes objects, variables, or items from a list or dictionary.                                                 |
| `elif`     | Stands for "else if"; used in conditional statements.                                                           |
| `else`     | Specifies a block of code to be executed if the condition in the `if` statement is false.                       |
| `except`   | Handles exceptions in a `try` block.                                                                            |
| `finally`  | Executes a block of code regardless of whether an exception occurred or not.                                    |
| `for`      | Creates a for loop to iterate over a sequence.                                                                  |
| `from`     | Imports specific parts of a module.                                                                             |
| `global`   | Declares a global variable inside a function.                                                                   |
| `if`       | Creates a conditional statement.                                                                                |
| `import`   | Imports a module.                                                                                               |
| `in`       | Checks if a value is present in a sequence or iterable.                                                         |
| `is`       | Tests for object identity; checks if two variables refer to the same object.                                    |
| `lambda`   | Creates an anonymous function.                                                                                  |
| `nonlocal` | Declares a non-local variable inside a function.                                                                |
| `not`      | A logical operator that inverts the value of a boolean expression.                                              |
| `or`       | A logical operator used to combine conditionals.                                                                |
| `while`    | A control flow statement used to create loops.                                                                  |
| `whith`    | Used to wrap the execution of a block with methods defined by a context manager (i.e., __enter__ and __exit__). |

## Scopes & Enclosures

| Scope Type         | Where It Exists                              | What It Controls                             | Relevant Topic              |
| ------------------ | -------------------------------------------- | -------------------------------------------- | --------------------------- |
| Local              | Inside a function                            | Names defined within the function            | Functions / Call stack      |
| Enclosing          | Outer function of a nested function          | Names captured by closures                   | Closures / Nested functions |
| Global             | Top-level of a module                        | Names accessible module-wide                 | Modules / Scripts           |
| Built-in           | Python runtime                               | Names like `len`, `sum`, `Exception`         | Python built-ins / Stdlib   |
| Dynamic (ChainMap) | Custom scopes in interpreters (e.g., lis.py) | Scoped resolution across nested environments | Interpreter design / DSLs   |

## Slicing

| Function/Method             | Type     | Description                                                                                                                                                                                                     | Example                                                                                             |
| --------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| `sequence[start:stop:step]` | Syntax   | Extracts elements from a sequence by slicing, using indices `start`, `stop`, and `step`. All parameters are optional: default `start` is `0`, default `stop` is the end of the sequence, default `step` is `1`. | `my_list[1:10:2]` → selects every second element from index `1` up to but not including index `10`. |
| `sequence[start:stop]`      | Syntax   | Extracts a subsequence starting at `start`, up to but not including `stop`. Defaults: `start = 0`, `stop = len(sequence)`.                                                                                      | `my_list[2:5]` → elements at indices 2, 3, and 4                                                    |
| `sequence[start:]`          | Syntax   | Extracts a subsequence starting from index `start` to the end of the sequence.                                                                                                                                  | `my_list[3:]` → elements from index 3 onwards                                                       |
| `sequence[:stop]`           | Syntax   | Extracts elements from the beginning of the sequence up to but not including index `stop`.                                                                                                                      | `my_list[:3]` → elements at indices 0, 1, and 2                                                     |
| `sequence[::step]`          | Syntax   | Extracts elements at intervals defined by `step`, from the beginning to the end of the sequence.                                                                                                                | `my_list[::2]` → every second element from the entire sequence                                      |
| `sequence[::-1]`            | Syntax   | Reverses a sequence by selecting elements from the end towards the start with a step of `-1`.                                                                                                                   | `my_list[::-1]` → reverses the sequence                                                             |
| `slice(start, stop, step)`  | Function | Creates a reusable slice object that can be applied to multiple sequences.                                                                                                                                      | `my_slice = slice(1,10,2)`<br>`my_list[my_slice]` → every second element from index 1 to 9          |
## `str` Escape Sequences

| Escape Sequence | Meaning                                                                 |
| --------------- | ----------------------------------------------------------------------- |
| `\n`            | `Newline` - moves to the next line                                      |
| `\t`            | `Horizontal Tab` - adds a tab space                                     |
| `\\`            | `Backslash` - to use a backslash itself                                 |
| `\'`            | `Single Quote`                                                          |
| `\"`            | `Double Quote`                                                          |
| `\N{}`          | `Unicode Escape` - insert a Unicode character i.e. `\N{INFINITY}` → `∞` |
| `\r`            | `Carriage Return `– moves to the beginning of the line                  |
## `str` Literal Prefixes

| Prefix      | Type             | Description                                                 | Example               | Output               |
| ----------- | ---------------- | ----------------------------------------------------------- | --------------------- | -------------------- |
| *(none)*    | Standard string  | Escape sequences are processed                              | `"Line 1\nLine 2"`    | `Line 1`<br>`Line 2` |
| `r` / `R`   | Raw string       | Backslashes are literal; escape sequences are not processed | `r"Line 1\nLine 2"`   | `Line 1\\nLine 2`    |
| `b` / `B`   | Bytes string     | Immutable sequence of bytes                                 | `b"hello"`            | `b'hello'`           |
| `f` / `F`   | Formatted string | Allows embedded expressions using `{}`                      | `f"2 + 2 = {2 + 2}"`  | `2 + 2 = 4`          |
| `fr` / `rf` | Raw f-string     | Combines raw and formatted string behavior (Python 3.6+)    | `fr"C:\name\{2 + 2}"` | `C:\name\4`          |
| `br` / `rb` | Raw bytes string | Bytes with raw escape handling                              | `br"line1\\nline2"`   | `b'line1\\\\nline2'` |

## Format Specification Mini-Language
1. **Placeholder Syntax Table** (Atomic Format Tokens)
	*⚠️ Use this to understand the individual characters used in format strings. Each symbol controls a specific behaviour.*

| Placeholder | Meaning                                                | Example                                                  |
| ----------- | ------------------------------------------------------ | -------------------------------------------------------- |
| `:`         | Start of format spec                                   | `"{:10}" → 'word '`                                      |
| `<`         | Left-align the field                                   | `"{:<10}".format('cat') → 'cat '`                        |
| `>`         | Right-align the field                                  | `"{:>10}".format('cat') → ' cat'`                        |
| `^`         | Center-align the field                                 | `"{:^10}".format('cat') → ' cat '`                       |
| `=`         | Pad after the sign, before digits (for numbers)        | `"{:=+10}".format(123) → '+ 123'`                        |
| `+`         | Explicit sign for positive and negative numbers        | `"{:+}".format(42) → '+42'`                              |
| `-`         | Only negative numbers show `-`, positives are blank    | `"{:-}".format(42) → '42'`                               |
| (space)     | Leading space for positive numbers, minus for negative | `"{: }".format(42) → ' 42'`                              |
| `,`         | Use comma as a thousands separator                     | `"{:,}".format(1234567) → '1,234,567'`                   |
| `_`         | Use underscore as a thousands separator                | `"{:_}".format(1234567) → '1_234_567'`                   |
| `b`         | Binary format                                          | `"{:b}".format(5) → '101'`                               |
| `c`         | Character format (from integer)                        | `"{:c}".format(65) → 'A'`                                |
| `d`         | Decimal integer                                        | `"{:d}".format(42) → '42'`                               |
| `o`         | Octal format                                           | `"{:o}".format(8) → '10'`                                |
| `x` / `X`   | Hexadecimal format (lowercase/uppercase)               | `"{:x}".format(255) → 'ff'`, `"{:X}".format(255) → 'FF'` |
| `e` / `E`   | Scientific notation (lowercase/uppercase)              | `"{:e}".format(123456) → '1.234560e+05'`                 |
| `f`         | Fixed-point number (floats and decimals)               | `"{:f}".format(3.14159) → '3.141590'`                    |
| `F`         | Fixed-point number (uppercase, for Decimals)           | `"{:F}".format(Decimal('3.14159')) → '3.141590'`         |
| `%`         | Percentage (value multiplied by 100, with % sign)      | `"{:.2%}".format(0.075) → '7.50%'`                       |
| `.N`        | Precision control, N digits after decimal              | `"{:.2f}".format(3.14159) → '3.14'`                      |
| `#`         | Alternate form (adds prefix for `b`, `o`, `x`)         | `"{:#x}".format(255) → '0xff'`                           |
| `0`         | Pad numeric field with zeros                           | `"{:05}".format(42) → '00042'`                           |
> **Cautionary Note on Field Widths (e.g., `:10`)**  
> When using format specifications like `{:10}`, the number sets a **minimum field width**, not padding with zeros or truncating text.  
> - If no alignment (`<`, `>`, `^`) is specified, **right-alignment** (`>`) is the default.  
> - The text will be **right-aligned** and **padded with spaces** on the left if needed.  
> - Content is **never truncated** — if the value is wider than the field, Python expands to fit it.  
> **Example:**  
> `"{:10}".format('word') → '      word'`  
> Always remember: **field width controls spacing, not the value itself**.

2. **Format Spec Components Table** (Semantic Breakdown)
	*⚠️ This breaks down the full format mini-language. Use it to assemble complex format specs correctly.*

| Part         | Purpose                                                        | Example Usage                               | Example Result             |
| :----------- | :------------------------------------------------------------- | :------------------------------------------ | :------------------------- |
| `fill`       | (Optional) Character used for padding                          | `'0>10'`                                    | Pads with `0` to width 10  |
| `align`      | (Optional) Alignment of text/numbers: `<`, `>`, `^`, `=`       | `'>'` → right-align, `'='` → pad after sign |                            |
| `sign`       | (Optional) How signs are shown: `+`, `-`, `' '`                | `'+'` → always show `+` or `-`              |                            |
| `#`          | (Optional) Alternate form (prefix for binary, octal, hex)      | `'#x'`                                      | `'0xff'` instead of `'ff'` |
| `0`          | (Optional) Shortcut: fill with zeros (`fill='0'`, `align='='`) | `'05'`                                      | `'00042'`                  |
| `width`      | (Optional) Minimum field width                                 | `'10'`                                      | `'    1234'`               |
| `,` or `_`   | (Optional) Thousands separator: comma or underscore            | `','` → `'1,000'`, `'_'` → `'1_000'`        |                            |
| `.precision` | (Optional) Digits after decimal point (for floats, Decimals)   | `'.2f'`                                     | `'3.14'`                   |
| `type`       | (Optional) Presentation type: `d`, `f`, `e`, `g`, `%`, etc.    | `'f'` (fixed-point), `'e'` (scientific)     |                            |
3.0 **Type Code Table** (Presentation Types Only)
	*⚠️ These control _how_ values are displayed (decimal, binary, percent, etc.). Required at the end of a format spec.*

| Type Code | Meaning                                            | Example Usage                         | Example Result                           |
| :-------- | :------------------------------------------------- | :------------------------------------ | :--------------------------------------- |
| `d`       | Decimal integer                                    | `"{:d}".format(42)`                   | `'42'`                                   |
| `b`       | Binary format                                      | `"{:b}".format(5)`                    | `'101'`                                  |
| `o`       | Octal format                                       | `"{:o}".format(8)`                    | `'10'`                                   |
| `x`       | Hexadecimal (lowercase)                            | `"{:x}".format(255)`                  | `'ff'`                                   |
| `X`       | Hexadecimal (UPPERCASE)                            | `"{:X}".format(255)`                  | `'FF'`                                   |
| `n`       | Decimal with locale separator                      | `"{:n}".format(1000)`                 | `'1,000'` or `'1000'` (locale-dependent) |
| `f`       | Fixed-point decimal                                | `"{:.2f}".format(3.14159)`            | `'3.14'`                                 |
| `F`       | Fixed-point decimal (uppercase special values)     | `"{:.2F}".format(Decimal('3.14159'))` | `'3.14'`                                 |
| `e`       | Scientific notation (lowercase `e`)                | `"{:.2e}".format(123456)`             | `'1.23e+05'`                             |
| `E`       | Scientific notation (uppercase `E`)                | `"{:.2E}".format(123456)`             | `'1.23E+05'`                             |
| `g`       | General format (fixed or scientific, shorter form) | `"{:g}".format(0.0001234)`            | `'0.0001234'`                            |
| `G`       | General format (uppercase if scientific)           | `"{:G}".format(0.0001234)`            | `'0.0001234'`                            |
| `%`       | Percentage (value × 100 with `%` sign)             | `"{:.1%}".format(0.075)`              | `'7.5%'`                                 |
| `s`       | String format                                      | `"{:s}".format('hello')`              | `'hello'`                                |
| `c`       | Single character (from int/char)                   | `"{:c}".format(65)`                   | `'A'`                                    |
| `None`    | No type specified → default type chosen            | `f"{42}"`, `f"{3.14}"`                | `'42'`, `'3.14'`                         |
4.0 **Composed Format Patterns Table** (Full Examples)
	*⚠️ Refer to these for practical formatting examples that combine multiple specifiers in real use.*

| Format Syntax | Description                                   | Example                       | Result         |
| ------------- | --------------------------------------------- | ----------------------------- | -------------- |
| `{:9.6f}`     | Float, width 9, 6 decimal places              | `"{:9.6f}".format(3.1415926)` | `' 3.141593'`  |
| `{:>10}`      | Right-align text in 10-character field        | `"{:>10}".format('cat')`      | `'       cat'` |
| `{:<10}`      | Left-align text in 10-character field         | `"{:<10}".format('cat')`      | `'cat       '` |
| `{:^10}`      | Center-align text in 10-character field       | `"{:^10}".format('cat')`      | `'   cat    '` |
| `{:.2%}`      | Percentage with 2 decimal places              | `"{:.2%}".format(0.1234)`     | `'12.34%'`     |
| `{:0>5}`      | Pad numeric with zeros on the left to width 5 | `"{:0>5}".format(42)`         | `'00042'`      |
5.0 **Legacy Formatting Conversion**
	*⚠️ Legacy formatting with `%` is still supported but discouraged. Use `str.format()` or f-strings for clarity, flexibility, and modern compatibility.*

| Formatting<br>Conversion in f-strings | Calls        | Usage Scenario                                                | Example                       |
| ------------------------------------- | ------------ | ------------------------------------------------------------- | ----------------------------- |
| `!r`                                  | `repr(obj)`  | Invokes `__repr__` method for debugging representation.       | `f"{'Adam'!r}"` → `'Adam'`    |
| `!s`                                  | `str(obj)`   | Invokes `__str__` method for pretty print method.             | `f"{obj!s}"`                  |
| `!a`                                  | `ascii(obj)` | `ASCII-safe` representation (escapes `non-ASCII` characters)` | `f"{'Café'!a}"` → `'Caf\xe9'` |

## Legacy Formats

> **Cautionary Note on Legacy `%` Formatting**
>
> The table below refers to the **older `%`-style string formatting**, which predates the modern `str.format()` and f-string approaches.
>
> - This syntax is **not part of the Format Specification Mini-Language**.
> - It is still commonly seen in older code and quick scripts.
> - The format specifiers differ in style and capabilities from `{:}` formatting.

| Placeholder | Legacy Format Meaning            | Example                                |
| ----------- | -------------------------------- | -------------------------------------- |
| `%s`        | String (calls `str()`)           | `'Hello %s' % 'Adam'` → `'Hello Adam'` |
| `%d`        | Integer                          | `'%d' % 42` → `'42'`                   |
| `%f`        | Float (default 6 decimal places) | `'%.2f' % 3.14159` → `'3.14'`          |
| `%r`        | Calls `repr()`                   | `'%r' % 'hi'` → `"'hi'"`               |

---
# APPENDIX

## Functions
### Built-In Functions
| **Function**                       | **Type**         | **Description**                         | **Example**                         |
| ---------------------------------- | ---------------- | --------------------------------------- | ----------------------------------- |
| `abs(x)`                           | Function         | Returns absolute value or magnitude.    | `abs(-5)` → `5`                     |
| `aiter(async_iterable)`            | Function         | Returns an asynchronous iterator.       | `it = aiter(obj)`                   |
| `all(iterable)`                    | Function         | `True` if every item is truthy.         | `all([1, True, 3])` → `True`        |
| `anext(async_iterator[, default])` | Function         | Returns next asynchronous item.         | `await anext(it)`                   |
| `any(iterable)`                    | Function         | `True` if any item is truthy.           | `any([0, False, 3])` → `True`       |
| `ascii(obj)`                       | Function         | Returns escaped ASCII representation.   | `ascii('café')` → `"'caf\\xe9'"`    |
| `bin(x)`                           | Function         | Converts integer to binary string.      | `bin(10)` → `'0b1010'`              |
| `bool(x)`                          | Type/Constructor | Converts value to Boolean.              | `bool(0)` → `False`                 |
| `breakpoint()`                     | Function         | Enters the debugger.                    | `breakpoint()`                      |
| `bytearray(source)`                | Type/Constructor | Creates mutable byte sequence.          | `bytearray(b'abc')`                 |
| `bytes(source)`                    | Type/Constructor | Creates immutable byte sequence.        | `bytes('abc', 'utf-8')`             |
| `callable(obj)`                    | Function         | Tests whether object is callable.       | `callable(print)` → `True`          |
| `chr(i)`                           | Function         | Converts code point to character.       | `chr(65)` → `'A'`                   |
| `classmethod(func)`                | Decorator        | Makes method receive the class.         | `@classmethod`                      |
| `compile(source, filename, mode)`  | Function         | Compiles source into code object.       | `compile('2+2', '', 'eval')`        |
| `complex(real[, imag])`            | Type/Constructor | Creates a complex number.               | `complex(2, 3)` → `(2+3j)`          |
| `delattr(obj, name)`               | Function         | Deletes a named attribute.              | `delattr(obj, 'x')`                 |
| `dict(...)`                        | Type/Constructor | Creates a dictionary.                   | `dict(a=1, b=2)`                    |
| `dir([obj])`                       | Function         | Lists available attribute names.        | `dir(str)`                          |
| `divmod(a, b)`                     | Function         | Returns quotient and remainder.         | `divmod(7, 3)` → `(2, 1)`           |
| `enumerate(iterable[, start])`     | Type/Constructor | Yields index-value pairs.               | `list(enumerate(['a','b']))`        |
| `eval(source)`                     | Function         | Evaluates a Python expression.          | `eval('2 + 3')` → `5`               |
| `exec(source)`                     | Function         | Executes Python statements.             | `exec('x = 5')`                     |
| `filter(function, iterable)`       | Type/Constructor | Lazily keeps matching items.            | `filter(bool, [0,1,2])`             |
| `float(x)`                         | Type/Constructor | Converts value to floating point.       | `float('3.5')` → `3.5`              |
| `format(value[, spec])`            | Function         | Formats a value.                        | `format(3.14159, '.2f')` → `'3.14'` |
| `frozenset(iterable)`              | Type/Constructor | Creates immutable set.                  | `frozenset([1,2,2])`                |
| `getattr(obj, name[, default])`    | Function         | Retrieves a named attribute.            | `getattr(obj, 'x', 0)`              |
| `globals()`                        | Function         | Returns global namespace dictionary.    | `globals()`                         |
| `hasattr(obj, name)`               | Function         | Tests whether attribute exists.         | `hasattr(obj, 'x')`                 |
| `hash(obj)`                        | Function         | Returns object's hash value.            | `hash('abc')`                       |
| `help([obj])`                      | Function         | Displays documentation/help.            | `help(list)`                        |
| `hex(x)`                           | Function         | Converts integer to hexadecimal string. | `hex(255)` → `'0xff'`               |
| `id(obj)`                          | Function         | Returns object identity integer.        | `id(x)`                             |
| `input([prompt])`                  | Function         | Reads user input as a string.           | `name = input('Name: ')`            |
| `int(x[, base])`                   | Type/Constructor | Converts value to integer.              | `int('101', 2)` → `5`               |
| `isinstance(obj, classinfo)`       | Function         | Tests object's type membership.         | `isinstance(3, int)` → `True`       |
| `issubclass(cls, classinfo)`       | Function         | Tests class inheritance.                | `issubclass(bool, int)` → `True`    |
| `iter(obj[, sentinel])`            | Function         | Returns an iterator.                    | `iter([1,2,3])`                     |
| `len(obj)`                         | Function         | Returns number of items.                | `len([1,2,3])` → `3`                |
| `list(iterable)`                   | Type/Constructor | Creates a mutable list.                 | `list('abc')` → `['a','b','c']`     |
| `locals()`                         | Function         | Returns current local namespace.        | `locals()`                          |
| `map(function, iterable, ...)`     | Type/Constructor | Lazily transforms iterable items.       | `map(str, [1,2,3])`                 |
| `max(...)`                         | Function         | Returns largest item.                   | `max([2,7,3])` → `7`                |
| `memoryview(obj)`                  | Type/Constructor | Creates zero-copy memory view.          | `memoryview(b'abc')`                |
| `min(...)`                         | Function         | Returns smallest item.                  | `min([2,7,3])` → `2`                |
| `next(iterator[, default])`        | Function         | Returns next iterator item.             | `next(iter([1,2]))` → `1`           |
| `object()`                         | Type/Constructor | Creates base Python object.             | `object()`                          |
| `oct(x)`                           | Function         | Converts integer to octal string.       | `oct(8)` → `'0o10'`                 |
| `open(file[, mode, ...])`          | Function         | Opens a file.                           | `open('data.txt', 'r')`             |
| `ord(c)`                           | Function         | Returns character code point.           | `ord('A')` → `65`                   |
| `pow(base, exp[, mod])`            | Function         | Performs exponentiation.                | `pow(2, 3)` → `8`                   |
| `print(*objects, ...)`             | Function         | Writes objects as text.                 | `print('Hello')`                    |
| `property(...)`                    | Type/Decorator   | Creates managed attribute.              | `@property`                         |
| `range(start, stop[, step])`       | Type/Constructor | Creates integer range sequence.         | `range(0, 10, 2)`                   |
| `repr(obj)`                        | Function         | Returns developer representation.       | `repr('abc')` → `"'abc'"`           |
| `reversed(sequence)`               | Type/Constructor | Returns reverse iterator.               | `reversed([1,2,3])`                 |
| `round(number[, ndigits])`         | Function         | Rounds a number.                        | `round(3.14159, 2)` → `3.14`        |
| `set(iterable)`                    | Type/Constructor | Creates mutable set.                    | `set([1,1,2])` → `{1, 2}`           |
| `setattr(obj, name, value)`        | Function         | Assigns a named attribute.              | `setattr(obj, 'x', 5)`              |
| `slice(start, stop[, step])`       | Type/Constructor | Creates a slice object.                 | `slice(1, 5, 2)`                    |
| `sorted(iterable, ...)`            | Function         | Returns new sorted list.                | `sorted([3,1,2])` → `[1,2,3]`       |
| `staticmethod(func)`               | Decorator        | Creates non-bound class method.         | `@staticmethod`                     |
| `str(obj)`                         | Type/Constructor | Creates string representation.          | `str(123)` → `'123'`                |
| `sum(iterable[, start])`           | Function         | Sums iterable values.                   | `sum([1,2,3])` → `6`                |
| `super([type[, obj]])`             | Type/Constructor | Delegates to superclass implementation. | `super().__init__()`                |
| `tuple(iterable)`                  | Type/Constructor | Creates immutable tuple.                | `tuple([1,2])` → `(1, 2)`           |
| `type(obj)`                        | Type/Constructor | Returns object's type.                  | `type(3)` → `<class 'int'>`         |
| `vars([obj])`                      | Function         | Returns object's `__dict__`.            | `vars(obj)`                         |
| `zip(*iterables, strict=False)`    | Type/Constructor | Iterates over iterables in parallel.    | `list(zip([1,2], ['a','b']))`       |
| `__import__(name, ...)`            | Function         | Low-level import mechanism.             | `__import__('math')`                |



### Standard Library Functions
For more on The Standard Python Library visit, https://pymotw.com/3/

#### #files | os.path

| **Function/Method**                | **Type** | **Description**                                                                                             | **Example**                                        |
| ---------------------------------- | -------- | ----------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| `os.getcwd()`                      | Method   | Returns the current working directory.                                                                      | `os.getcwd()` → `'/home/user/project'`             |
| `os.path.abspath(path)`            | Method   | Converts a relative path to an absolute path (i.e. path appendment)                                         | `os.path.abspath('example.txt')`                   |
| `os.listdir(path)`                 | Method   | Lists the contents of a directory. Defaults to current directory if no path is given.                       | `os.listdir('.')` → `['file.txt', 'data']`         |
| `os.path.exists(path)`             | Method   | Checks if the specified path exists.                                                                        | `os.path.exists('example.txt')` → `True`           |
| `os.path.isdir(path)`              | Method   | Checks if the given path is a directory.                                                                    | `os.path.isdir('my_folder')` → `True`              |
| `os.path.isfile(path)`             | Method   | Checks if the given path is a file.                                                                         | `os.path.isfile('example.txt')` → `True`           |
| `os.path.join(*paths)`             | Method   | Joins one or more path components intelligently to create a valid path.                                     | `os.path.join('dir', 'file.txt')` → `dir/file.txt` |
| `os.makedirs(path, exist_ok=True)` | Method   | Creates a directory and any intermediate directories if they don’t exist. Does nothing if `exist_ok=False`. | `os.makedirs('new_folder', exist_ok=True)`         |

#### #files | pathlib

| Function/Method                           | Type     | Description                                                                                                                                                                                                                                                                    | Example                                              |
| ----------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------- |
| `Path(path)`                              | Class    | Creates a `Path` object representing a filesystem path.                                                                                                                                                                                                                        | `p = Path('/home/user')`                             |
| `Path.cwd()`                              | Method   | Returns the current working directory as a `Path` object.                                                                                                                                                                                                                      | `p = Path.cwd()`                                     |
| `Path.home()`                             | Method   | Returns the user’s home directory as a `Path` object.                                                                                                                                                                                                                          | `p = Path.home()`                                    |
| `Path.exists()`                           | Method   | Checks if the path exists.                                                                                                                                                                                                                                                     | `p.exists()`                                         |
| `Path.is_file()`                          | Method   | Checks if the path points to a file.                                                                                                                                                                                                                                           | `p.is_file()`                                        |
| `Path.is_dir()`                           | Method   | Checks if the path points to a directory.                                                                                                                                                                                                                                      | `p.is_dir()`                                         |
| `Path.mkdir(parents=True, exist_ok=True)` | Method   | Creates a directory, including parent directories if needed.                                                                                                                                                                                                                   | `p.mkdir(parents=True, exist_ok=True)`               |
| `Path.rmdir()`                            | Method   | Removes an empty directory.                                                                                                                                                                                                                                                    | `p.rmdir()`                                          |
| `Path.unlink()`                           | Method   | Deletes a file.                                                                                                                                                                                                                                                                | `p.unlink()`                                         |
| `Path.iterdir()`                          | Method   | Returns an iterator of `Path` objects inside a directory.                                                                                                                                                                                                                      | `for f in p.iterdir(): print(f)`                     |
| `Path.glob('*.txt')`                      | Method   | Returns an iterator of files matching the pattern.                                                                                                                                                                                                                             | `list(p.glob('*.txt'))`                              |
| `Path.read_text()`                        | Method   | Reads the contents of a file as a string.                                                                                                                                                                                                                                      | `content = p.read_text()`                            |
| `Path.write_text(data)`                   | Method   | Writes a string to a file.                                                                                                                                                                                                                                                     | `p.write_text('Hello, world!')`                      |
| `string.splitlines(keepends=False)`       | Method   | Splits a string into a list of lines, breaking at line boundaries. If `keepends=True`, line breaks are included.                                                                                                                                                               | `"Hello\nWorld".splitlines()` → `['Hello', 'World']` |
| `Path.name`                               | Property | Returns the final component of the path.                                                                                                                                                                                                                                       | `p.name`                                             |
| `Path.stem`                               | Property | Returns the filename without the extension.                                                                                                                                                                                                                                    | `p.stem`                                             |
| `Path.suffix`                             | Property | Returns the file extension.                                                                                                                                                                                                                                                    | `p.suffix`                                           |
| `Path.parent`                             | Property | Returns the parent directory of the path.                                                                                                                                                                                                                                      | `p.parent`                                           |
| `Path.parts`                              | Property | Returns the path as a tuple of components.                                                                                                                                                                                                                                     | `p.parts`                                            |
| `Path.absolute()`                         | Method   | Returns the absolute path.                                                                                                                                                                                                                                                     | `p.absolute()`                                       |
| `Path.rename(target)`                     | Method   | Renames the file or directory to the given target.                                                                                                                                                                                                                             | `p.rename('new_name.txt')`                           |
| `Path.write_bytes(data)`                  | Method   | Writes the specified bytes-like object `data` directly to the file represented by the `Path` object. Overwrites the file if it exists, or creates it if it does not. Operates in binary mode without needing to explicitly open the file. Returns the number of bytes written. | `Path('example.bin').write_bytes(b'hello world')`    |
#### #files | csv

| Function/Method                    | Type     | Description                                             | Example                                                                       |
| ---------------------------------- | -------- | ------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `csv.reader(file)`                 | Function | Reads a CSV file and returns an iterable reader object. | `reader = csv.reader(open('file.csv', 'r'))`                                  |
| `csv.writer(file)`                 | Function | Writes to a CSV file, returning a writer object.        | `writer = csv.writer(open('file.csv', 'w'))`                                  |
| `csv.DictReader(file)`             | Class    | Reads a CSV file into a dictionary format.              | `reader = csv.DictReader(open('file.csv', 'r'))`                              |
| `csv.DictWriter(file, fieldnames)` | Class    | Writes dictionary objects to a CSV file.                | `writer = csv.DictWriter(open('file.csv', 'w'), fieldnames=['col1', 'col2'])` |
| `csv.field_size_limit(new_limit)`  | Function | Sets the maximum field size allowed.                    | `csv.field_size_limit(1024*1024)`                                             |
| `csv.QUOTE_ALL`                    | Constant | Quotes all fields in the output.                        | `csv.writer(file, quoting=csv.QUOTE_ALL)`                                     |

#### #files | json 

| Function/Method        | Type     | Description                                                                                                                                                                                | Example                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `json.loads(s)`        | Function | Converts a JSON-formatted **string** into a Python object (e.g., dict, list, int, float, etc.). Note, a JSON-formatted string is a **STRING** object and therefore must be parsed somehow. | `json.loads('{"name": "John", "age": 30}')` → `{'name': 'John', 'age': 30}`<br><br>*NOTE: Inverse Function -`json.dumps()`*<br><br>*IMPORTANT: A JSON-Formatted string is different from a Python 'stringified' dict in that some inputs like `False` are as lowercase `false`. The `loads()` function will not work with incorrect entries like `False` if they are entered as values.  |
| `json.dumps(obj)`      | Function | Converts a Python object into a JSON-formatted **pretty string**. The **pretty string** is primarily designed for readability and serialization.                                           | `json.dumps({"name": "John", "age": 30})` → `'{"name": "John", "age": 30}'`<br><br>*NOTE: Inverse Function -`json.loads()`*                                                                                                                                                                                                                                                              |
| `json.load(file)`      | Function | Reads a JSON **file** and converts it into a Python object.                                                                                                                                | `data = json.load(open("file.json"))`<br><br>*NOTE: Converts File- NOT String*                                                                                                                                                                                                                                                                                                           |
| `json.dump(obj, file)` | Function | Writes a Python object as JSON to a **file**.                                                                                                                                              | `json.dump({"name": "John"}, open("file.json", "w"))`<br><br>*NOTE: Converts File- NOT String*                                                                                                                                                                                                                                                                                           |

#### #files | Shelve 
The `shelve` module in Python is used for persistent storage of Python objects. It provides a dictionary-like interface to store and retrieve objects using keys. Unlike a regular dictionary, the data is stored on disk, so it persists even after the program ends.

| Function/Method                                                        | Type            | Description                                                                                                                             | Example                                         |
| ---------------------------------------------------------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| `shelve.open(filename)`<br><br>or <br><br>`shelve.open(filename, 'n')` | Function        | Opens a shelve database file. The file is created if it doesn’t exist. Returns a dictionary-like object. 'n' creates new shelve object. | `db = shelve.open('mydata.db')`                 |
| `db[key] = value`                                                      | Method (Write)  | Stores the given `value` in the shelve database with the associated `key`.                                                              | `db['name'] = 'Alice'`                          |
| `value = db[key]`                                                      | Method (Read)   | Retrieves the `value` associated with the given `key`.                                                                                  | `print(db['name'])  # Output: 'Alice'`          |
| `del db[key]`                                                          | Method (Delete) | Removes the entry with the specified `key` from the shelve database.                                                                    | `del db['name']`                                |
| `db.keys()`                                                            | Method          | Returns a view object containing all the keys in the shelve database.                                                                   | `print(list(db.keys()))`                        |
| `db.values()`                                                          | Method          | Returns a view object containing all the values in the shelve database.                                                                 | `print(list(db.values()))`                      |
| `db.items()`                                                           | Method          | Returns a view object containing all key-value pairs in the shelve database.                                                            | `print(list(db.items()))`                       |
| `db.close()`                                                           | Method          | Closes the shelve database. Must be called to ensure data is saved correctly.                                                           | `db.close()`                                    |
| `writeback=True`                                                       | Argument        | Ensures changes to mutable objects are saved automatically. Slower and uses more memory.                                                | `db = shelve.open('mydata.db', writeback=True)` |

#### #typing | collections 

| Function/Method                         | Type              | Description                                                                                       | Example |
|----------------------------------------|-------------------|---------------------------------------------------------------------------------------------------|---------|
| `defaultdict(default_factory)`         | Class             | Subclass of `dict` that returns default values for missing keys using a factory function.         | `d = defaultdict(list); d['key'].append(1)` |
| `.default_factory`                     | Attribute         | Holds the factory function used by `defaultdict` to generate default values.                      | `d.default_factory = int` |
| `namedtuple(typename, fields)`         | Factory Function  | Returns a tuple subclass with named fields accessible by attribute.                              | `Point = namedtuple('Point', ['x', 'y']); p = Point(1, 2)` |
| `.make(iterable)`                      | Class Method      | Builds a `namedtuple` from an iterable.                                                           | `Point._make([1, 2])` |
| `._asdict()`                           | Instance Method   | Returns a `dict` representation of the `namedtuple`.                                              | `p._asdict()` |
| `._fields`                             | Class Attribute   | Tuple of field names defined in the `namedtuple`.                                                 | `Point._fields` |
| `._field_defaults`                     | Class Attribute   | Dict of default values for fields (Python 3.7+).                                                  | `Point._field_defaults` |
| `deque([iterable], maxlen=None)`       | Class             | Thread-safe double-ended queue optimized for fast append and pop operations on either end.        | `dq = deque([1,2,3]); dq.appendleft(0)` |
| `Counter([iterable])`                  | Class             | Subclass of `dict` to count occurrences of hashable items; supports arithmetic and multiset ops.  | `Counter('banana')` |
| `Counter.most_common(n)`               | Method            | Returns a list of the `n` most common elements and their counts.                                  | `Counter('banana').most_common(2)` |
| `OrderedDict()`                        | Class             | Subclass of `dict` that preserves insertion order and provides reordering methods.                | `od = OrderedDict(); od['a'] = 1` |
| `OrderedDict.move_to_end(key, last)`   | Method            | Moves an entry to either end of the dictionary (`last=True` default).                             | `od.move_to_end('a', last=False)` |
| `ChainMap(*maps)`                      | Class             | Groups multiple mappings into a single view without merging them.                                 | `ChainMap(d1, d2)` |
| `UserDict([initialdata])`             | Class             | Wrapper around regular dictionary objects for easier subclassing.                                 | `from collections import UserDict` |
| `UserList([list])`                     | Class             | Wrapper around list objects, simplifies subclassing the list type.                                | `from collections import UserList` |
| `UserString(seq)`                      | Class             | Wrapper around string objects, useful for customizing string behavior via subclassing.            | `from collections import UserString` |

#### #typing | collections.abc 

| Function/Method   | Type | Description                                                                                 | Example              |
| ----------------- | ---- | ------------------------------------------------------------------------------------------- | -------------------- |
| `Container`       | ABC  | Base class requiring `__contains__` to check membership using `in`.                         | `x in container`     |
| `Hashable`        | ABC  | Base class requiring `__hash__` for hash table compatibility.                               | `hash(obj)`          |
| `Iterable`        | ABC  | Provides iteration protocol via `__iter__`.                                                 | `for x in obj:`      |
| `Iterator`        | ABC  | Extends `Iterable` and requires `__next__` and `__iter__`.                                  | `next(it)`           |
| `Reversible`      | ABC  | Requires `__reversed__` for reverse iteration.                                              | `reversed(seq)`      |
| `Generator`       | ABC  | Extends `Iterator`; adds `send`, `throw`; mixins: `close`, `__iter__`, `__next__`.          | `g.send(value)`      |
| `Sized`           | ABC  | Requires `__len__` for measuring container length.                                          | `len(container)`     |
| `Callable`        | ABC  | Requires `__call__` to make object callable.                                                | `obj()`              |
| `Collection`      | ABC  | Combines `Sized`, `Iterable`, `Container`; requires all three corresponding dunder methods. | –                    |
| `Sequence`        | ABC  | Extends `Collection` and `Reversible`; adds `__getitem__`, `__len__`.                       | `seq[0]`, `len(seq)` |
| `MutableSequence` | ABC  | Extends `Sequence`; adds mutators like `__setitem__`, `__delitem__`, `insert`.              | `seq.append(x)`      |
| `ByteString`      | ABC  | Subclass of `Sequence` for bytes; immutable.                                                | `b'abc'[1]`          |
| `Set`             | ABC  | Unordered collection; requires `__contains__`, `__iter__`, `__len__`; rich comparisons.     | `x in set`, `a & b`  |
| `MutableSet`      | ABC  | Extends `Set`; adds `add`, `discard`, and in-place ops like `__ior__`, `__iand__`.          | `s.add(x)`           |
#### #system | sys 

| Function/Method | Type | Description                                                                                                                             | Example                              |
| --------------- | ---- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `sys.argv`      | List | Contains the command-line arguments passed to a Python script. The first element is the script name itself, and the rest are arguments. | `import sys`<br>`print(sys.argv[1])` |
#### #other | copy 

| Function/Method      | Type     | Description                                                                                                                                    | Example                                                 |
| -------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `copy.copy(obj)`     | Function | Returns a **shallow copy** of `obj`. Changes to mutable objects inside `obj` affect both the original and the copy.                            | `import copy; new_list = copy.copy(old_list)`           |
| `copy.deepcopy(obj)` | Function | Returns a **deep copy** of `obj`. Recursively copies all nested mutable objects, ensuring that changes to the copy do not affect the original. | `import copy; deep_copy_list = copy.deepcopy(old_list)` |
#### #other | itertools

| Function/Method                                     | Type     | Description                                                                                                                                                           | Example                                                                                  |
| --------------------------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `itertools.zip_longest(*iterables, fillvalue=None)` | Function | Returns an iterator that aggregates elements from each of the iterables. If the iterables are of unequal length, missing values are filled with the `fillvalue`.      | `list(zip_longest('AB', 'xyz', fillvalue='-'))` → `[('A', 'x'), ('B', 'y'), ('-', 'z')]` |
| `itertools.cycle(iterable)`                         | Function | Returns an iterator that cycles through the elements of the iterable indefinitely, restarting from the beginning when the end is reached.                             | `cycle(['A', 'B', 'C'])` yields `'A', 'B', 'C', 'A', 'B', 'C', ...`                      |
| `itertools.chain(*iterables)`                       | Function | Lazily yields every item from the first iterable, followed by every item from each subsequent iterable. Requires `from itertools import chain` or `import itertools`. | `list(chain([1, 2], ['a', 'b']))` → `[1, 2, 'a', 'b']`                                   |


#### #other | datetime 

| Function/Method                          | Type        | Description                                                           | Example                                                                          |
| ---------------------------------------- | ----------- | --------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| `datetime.datetime()`                    | Constructor | Creates a `datetime` object representing a specific date and time.    | `import datetime`<br>`dt = datetime.datetime(2023, 3, 10, 15, 30, 45)`           |
| `datetime.strptime(date_string, format)` | Method      | Parses a string into a `datetime` object based on a specified format. | `import datetime`<br>`dt = datetime.datetime.strptime("2023-03-10", "%Y-%m-%d")` |

#### #other | operator

| Function/Method      | Type     | Description                                                                                                                                                                                                    | Example                                                     |
| -------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `itemgetter(*items)` | Function | Returns a callable that fetches the given item(s) from its operand (e.g., index in a list or key in a dict). Often used for sorting or extracting values. Commonly passed to `sorted()` as the `key` argument. | `getter = itemgetter(1); getter(['a', 'b', 'c']) → 'b'`     |
| `attrgetter(*attrs)` | Function | Returns a callable that fetches the given attribute(s) from its operand. Useful for sorting or accessing nested attributes. Commonly passed to `sorted()` as the `key` argument.                               | `getter = attrgetter('name'); getter(person) → person.name` |
| `operator.xor(a, b)` | Function | Performs bitwise XOR (exclusive OR) operation on two integers `a` and `b`, equivalent to `a ^ b`. Returns the result as an integer.                                                                            | `operator.xor(10, 4)` → `14`                                |

#### #other | functools

| Function/Method                                   | Type     | Description                                                                                                                                                      | Example                                                |
| ------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| `functools.reduce(func, iterable[, initializer])` | Function | Applies `func` cumulatively to the items of `iterable`, reducing it to a single value. If `initializer` is given, it is placed before the items of the iterable. | `reduce(lambda x, y: x + y, [1, 2, 3,                  |
| `functools.partial(func, /, *args, **kwargs)`     | Function | Returns a new function with fixed arguments and/or keywords, allowing partial application of `func`.                                                             | `double = partial(pow, exp=2); double(3)  # Output: 9` |
#### #other | inspect

| Function/Method                           | Type                         | Description                                                                                                             | Example                                        |     |
| ----------------------------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | --- |
| `inspect.getmembers(obj, predicate=None)` | Function                     | Returns all members of an object (functions, variables, etc.) as `(name, value)` pairs. Can be filtered by a predicate. | `inspect.getmembers(math, inspect.isfunction)` |     |
| `inspect.isfunction(object)`              | Function                     | Returns `True` if the object is a user-defined Python function (not built-in or method).                                | `inspect.isfunction(lambda x: x + 1)`          |     |
| `inspect.isclass(obj)`                    | Function (`inspect`, stdlib) | Returns `True` if `obj` is a class object (instance of `type` or a metaclass); otherwise `False`.                       | `inspect.isclass(MyClass)`                     |     |
| `inspect.ismethod(obj)`                   | Function (`inspect`, stdlib) | Returns True if obj is a bound method (a function accessed through an instance, producing a method object).             | `inspect.ismethod(obj.method)`                 |     |
| `inspect.ismodule(obj)`                   | Function (`inspect`, stdlib) | Returns True if obj is a module object.                                                                                 | `inspect.ismodule(math)`                       |     |
| `inspect.isdatadescriptor(obj)`           | Function (`inspect`, stdlib) | Returns True if obj is a data descriptor (implements `__get__` and `__set__` and/or `__delete__`).                      | `inspect.isdatadescriptor(type(obj).attr)`     |     |
#### #other | random

| **Function/Method**     | **Type** | **Description**                                                                            | **Example**                                       |
| ----------------------- | -------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------- |
| `random.seed(a=None)`   | Function | Initializes the random number generator with the given seed. Ensures reproducible results. | `random.seed(10)`                                 |
| `random.random()`       | Function | Returns a random float between 0.0 and 1.0.                                                | `random.random()` → `0.74`                        |
| `random.randint(a, b)`  | Function | Returns a random integer between `a` and `b` (inclusive).                                  | `random.randint(1, 10)` → `7`                     |
| `random.uniform(a, b)`  | Function | Returns a random float between `a` and `b`.                                                | `random.uniform(1.5, 7.5)` → `5.3`                |
| `random.choice(seq)`    | Function | Returns a random element from a non-empty sequence.                                        | `random.choice([1, 2, 3])` → `2`                  |
| `random.shuffle(seq)`   | Function | Shuffles the sequence in place.                                                            | `random.shuffle(my_list)`                         |
| `random.sample(seq, k)` | Function | Returns a list with `k` unique elements from the sequence.                                 | `random.sample(range(10), 5)` → `[2, 5, 1, 7, 9]` |

#### #other | time
| Function/Method         | Type     | Description                                                                                                                                       | Example                                                               |
| ----------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `time.sleep(secs)`      | Function | Suspends execution of the current thread for the given number of seconds. Accepts float for subsecond precision.                                  | `time.sleep(0.5)`                                                     |
| `time.time()`           | Function | Returns the current time in seconds since the Unix epoch (Jan 1, 1970, UTC). Used to measure wall-clock time.                                     | `elapsed = time.time() - start`                                       |
| `time.perf_counter()`   | Function | Returns a high-resolution performance timer (includes sleep). Ideal for benchmarking. Use with subtraction.                                       | `start = time.perf_counter(); elapsed = time.perf_counter() - start`  |
| `time.process_time()`   | Function | Returns CPU time used by the current process (excludes sleep). Use with subtraction to measure CPU-bound work.                                    | `start = time.process_time(); cpu_time = time.process_time() - start` |
| `time.strftime(format)` | Function | Returns the current time formatted as a string according to the specified format (e.g., `%H:%M:%S` for `23:15:42`). Useful for timestamping logs. | `strftime('%H:%M:%S')` → `'15:42:01'`                                 |

#### #other | re

| Function/Method                | Type     | Description                                                                                                           | Example                                                                                                                                        |
| ------------------------------ | -------- | --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `re.finditer(pattern, string)` | Function | Scans the string for matches to the regex pattern and returns an iterator of match objects, one for each match found. | `matches = re.finditer(r'\d+', 'Item 23, 45, and 67.')`<br><br>`pattern.finditer(text)` where pattern is `re.compile()`- regex matching object |
| `match.group()`                | Method   | Returns the matched string from a match object.                                                                       | `for m in matches: print(m.group())`                                                                                                           |
| `match.start()`                | Method   | Returns the starting index of the match within the input string.                                                      | `for m in matches: print(m.start())`                                                                                                           |
### 3rd-Party Functions
#### #http | requests

| **Function/Method**             | **Type**  | **Description**                                                     | **Example**                                                                            |
| ------------------------------- | --------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| `requests.get(url)`             | Function  | Sends a GET request to the specified URL and returns the response.  | `response = requests.get('https://api.example.com/data')`                              |
| `requests.post(url, data=data)` | Function  | Sends a POST request to the specified URL with the provided data.   | `response = requests.post('https://api.example.com/post', data={'key': 'value'})`      |
| `requests.put(url, data=data)`  | Function  | Sends a PUT request to update a resource at the specified URL.      | `response = requests.put('https://api.example.com/update', data={'key': 'new_value'})` |
| `requests.delete(url)`          | Function  | Sends a DELETE request to remove a resource at the specified URL.   | `response = requests.delete('https://api.example.com/delete')`                         |
| `requests.head(url)`            | Function  | Sends a HEAD request to retrieve response headers without the body. | `response = requests.head('https://api.example.com')`                                  |
| `requests.json()`               | Method    | Parses the JSON response from a request.                            | `data = response.json()`                                                               |
| `requests.status_code`          | Attribute | Returns the HTTP status code of the response.                       | `print(response.status_code)`                                                          |
| `requests.text`                 | Attribute | Returns the response body as a string.                              | `print(response.text)`                                                                 |
| `requests.params`               | Parameter | Allows sending URL query parameters in a request.                   | `response = requests.get('https://api.example.com', params={'q': 'search'})`           |

## Built-in Methods

### `int`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`int.as_integer_ratio()`|Method|Returns exact integer ratio.|`(10).as_integer_ratio()` → `(10, 1)`|
|`int.bit_count()`|Method|Counts `1` bits.|`(13).bit_count()` → `3`|
|`int.bit_length()`|Method|Returns bits needed for magnitude.|`(10).bit_length()` → `4`|
|`int.conjugate()`|Method|Returns complex conjugate; unchanged for int.|`(5).conjugate()` → `5`|
|`int.from_bytes(bytes, byteorder='big', *, signed=False)`|Class method|Builds integer from bytes.|`int.from_bytes(b'\x01\x00')` → `256`|
|`int.is_integer()`|Method|Always `True` for integers.|`(5).is_integer()` → `True`|
|`int.to_bytes(length=1, byteorder='big', *, signed=False)`|Method|Converts integer to bytes.|`(65).to_bytes()` → `b'A'`|

### `float`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`float.as_integer_ratio()`|Method|Returns exact integer ratio.|`(0.5).as_integer_ratio()` → `(1, 2)`|
|`float.conjugate()`|Method|Returns conjugate; unchanged for float.|`(2.5).conjugate()` → `2.5`|
|`float.fromhex(string)`|Class method|Creates float from hexadecimal string.|`float.fromhex('0x1p2')` → `4.0`|
|`float.from_number(x)`|Class method|Creates float from numeric object.|`float.from_number(5)` → `5.0`|
|`float.hex()`|Method|Returns hexadecimal float string.|`(3.5).hex()` → `'0x1.c000000000000p+1'`|
|`float.is_integer()`|Method|Tests whether value is integral.|`(3.0).is_integer()` → `True`|

### `complex`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`complex.conjugate()`|Method|Returns complex conjugate.|`(2+3j).conjugate()` → `(2-3j)`|
|`complex.from_number(x)`|Class method|Creates complex value from numeric object.|`complex.from_number(5)` → `(5+0j)`|

### `bool`

`bool` inherits its public numeric methods from `int`.

### `list`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`list.append(x)`|Method|Adds item to end.|`a.append(4)`|
|`list.clear()`|Method|Removes all items.|`a.clear()`|
|`list.copy()`|Method|Returns shallow copy.|`b = a.copy()`|
|`list.count(x)`|Method|Counts matching values.|`[1,1,2].count(1)` → `2`|
|`list.extend(iterable)`|Method|Appends all iterable items.|`a.extend([3,4])`|
|`list.index(x[, start[, end]])`|Method|Returns first matching index.|`['a','b'].index('b')` → `1`|
|`list.insert(i, x)`|Method|Inserts item at index.|`a.insert(1, 'x')`|
|`list.pop([i])`|Method|Removes and returns item.|`a.pop()`|
|`list.remove(x)`|Method|Removes first matching value.|`a.remove('x')`|
|`list.reverse()`|Method|Reverses list in place.|`a.reverse()`|
|`list.sort(*, key=None, reverse=False)`|Method|Sorts list in place.|`a.sort(reverse=True)`|

### `tuple`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`tuple.count(x)`|Method|Counts matching values.|`(1,1,2).count(1)` → `2`|
|`tuple.index(x[, start[, end]])`|Method|Returns first matching index.|`('a','b').index('b')` → `1`|

### `range`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`range.count(x)`|Method|Counts occurrences of value.|`range(5).count(3)` → `1`|
|`range.index(x)`|Method|Returns index of value.|`range(2,10,2).index(6)` → `2`|

### `str`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`str.capitalize()`|Method|Capitalizes first character.|`'python'.capitalize()` → `'Python'`|
|`str.casefold()`|Method|Aggressively lowercases text.|`'Straße'.casefold()` → `'strasse'`|
|`str.center(width[, fillchar])`|Method|Centers with padding.|`'a'.center(3, '-')` → `'-a-'`|
|`str.count(sub[, start[, end]])`|Method|Counts substring occurrences.|`'banana'.count('an')` → `2`|
|`str.encode(encoding='utf-8', errors='strict')`|Method|Encodes string as bytes.|`'abc'.encode()` → `b'abc'`|
|`str.endswith(suffix[, start[, end]])`|Method|Tests suffix.|`'file.py'.endswith('.py')` → `True`|
|`str.expandtabs(tabsize=8)`|Method|Expands tabs.|`'a\tb'.expandtabs(4)` → `'a b'`|
|`str.find(sub[, start[, end]])`|Method|Returns index or `-1`.|`'abc'.find('b')` → `1`|
|`str.format(*args, **kwargs)`|Method|Formats replacement fields.|`'{} {}'.format('a','b')` → `'a b'`|
|`str.format_map(mapping)`|Method|Formats using mapping.|`'{x}'.format_map({'x':1})` → `'1'`|
|`str.index(sub[, start[, end]])`|Method|Returns index or raises error.|`'abc'.index('b')` → `1`|
|`str.isalnum()`|Method|Tests alphanumeric characters.|`'abc123'.isalnum()` → `True`|
|`str.isalpha()`|Method|Tests alphabetic characters.|`'abc'.isalpha()` → `True`|
|`str.isascii()`|Method|Tests ASCII-only characters.|`'abc'.isascii()` → `True`|
|`str.isdecimal()`|Method|Tests decimal characters.|`'123'.isdecimal()` → `True`|
|`str.isdigit()`|Method|Tests digit characters.|`'²'.isdigit()` → `True`|
|`str.isidentifier()`|Method|Tests identifier syntax.|`'my_var'.isidentifier()` → `True`|
|`str.islower()`|Method|Tests lowercase form.|`'abc'.islower()` → `True`|
|`str.isnumeric()`|Method|Tests numeric characters.|`'Ⅳ'.isnumeric()` → `True`|
|`str.isprintable()`|Method|Tests printable characters.|`'abc'.isprintable()` → `True`|
|`str.isspace()`|Method|Tests whitespace.|`' \t'.isspace()` → `True`|
|`str.istitle()`|Method|Tests title case.|`'Hello World'.istitle()` → `True`|
|`str.isupper()`|Method|Tests uppercase form.|`'ABC'.isupper()` → `True`|
|`str.join(iterable)`|Method|Joins strings.|`','.join(['a','b'])` → `'a,b'`|
|`str.ljust(width[, fillchar])`|Method|Left-justifies text.|`'a'.ljust(3, '-')` → `'a--'`|
|`str.lower()`|Method|Returns lowercase copy.|`'ABC'.lower()` → `'abc'`|
|`str.lstrip([chars])`|Method|Removes leading characters.|`' a'.lstrip()` → `'a'`|
|`str.maketrans(...)`|Static method|Creates translation mapping.|`str.maketrans({'a':'b'})`|
|`str.partition(sep)`|Method|Splits into three parts.|`'a=b'.partition('=')` → `('a','=','b')`|
|`str.removeprefix(prefix)`|Method|Removes matching prefix.|`'TestCase'.removeprefix('Test')` → `'Case'`|
|`str.removesuffix(suffix)`|Method|Removes matching suffix.|`'file.py'.removesuffix('.py')` → `'file'`|
|`str.replace(old, new[, count])`|Method|Replaces substrings.|`'aaa'.replace('a','b',2)` → `'bba'`|
|`str.rfind(sub[, start[, end]])`|Method|Returns last index or `-1`.|`'abca'.rfind('a')` → `3`|
|`str.rindex(sub[, start[, end]])`|Method|Returns last index or errors.|`'abca'.rindex('a')` → `3`|
|`str.rjust(width[, fillchar])`|Method|Right-justifies text.|`'a'.rjust(3, '-')` → `'--a'`|
|`str.rpartition(sep)`|Method|Partitions at last separator.|`'a=b=c'.rpartition('=')`|
|`str.rsplit(sep=None, maxsplit=-1)`|Method|Splits from right.|`'a,b,c'.rsplit(',',1)`|
|`str.rstrip([chars])`|Method|Removes trailing characters.|`'a '.rstrip()` → `'a'`|
|`str.split(sep=None, maxsplit=-1)`|Method|Splits into substrings.|`'a,b'.split(',')` → `['a','b']`|
|`str.splitlines(keepends=False)`|Method|Splits into lines.|`'a\nb'.splitlines()`|
|`str.startswith(prefix[, start[, end]])`|Method|Tests prefix.|`'python'.startswith('py')` → `True`|
|`str.strip([chars])`|Method|Removes edge characters.|`' a '.strip()` → `'a'`|
|`str.swapcase()`|Method|Swaps letter case.|`'Py'.swapcase()` → `'pY'`|
|`str.title()`|Method|Returns title case.|`'hello world'.title()`|
|`str.translate(table)`|Method|Translates characters.|`'abc'.translate({97:120})` → `'xbc'`|
|`str.upper()`|Method|Returns uppercase copy.|`'abc'.upper()` → `'ABC'`|
|`str.zfill(width)`|Method|Pads with zeros.|`'42'.zfill(5)` → `'00042'`|

### `bytes`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`bytes.capitalize()`|Method|Capitalizes ASCII first byte.|`b'abc'.capitalize()`|
|`bytes.center(width[, fillbyte])`|Method|Centers with padding.|`b'a'.center(3,b'-')`|
|`bytes.count(sub[, start[, end]])`|Method|Counts subsequences.|`b'banana'.count(b'an')`|
|`bytes.decode(encoding='utf-8', errors='strict')`|Method|Decodes to string.|`b'abc'.decode()` → `'abc'`|
|`bytes.endswith(suffix[, start[, end]])`|Method|Tests suffix.|`b'a.py'.endswith(b'.py')`|
|`bytes.expandtabs(tabsize=8)`|Method|Expands tabs.|`b'a\tb'.expandtabs(4)`|
|`bytes.find(sub[, start[, end]])`|Method|Returns index or `-1`.|`b'abc'.find(b'b')`|
|`bytes.fromhex(string)`|Class method|Creates bytes from hex.|`bytes.fromhex('41 42')` → `b'AB'`|
|`bytes.hex([sep[, bytes_per_sep]])`|Method|Returns hexadecimal string.|`b'AB'.hex()` → `'4142'`|
|`bytes.index(sub[, start[, end]])`|Method|Returns index or errors.|`b'abc'.index(b'b')`|
|`bytes.isalnum()`|Method|Tests ASCII alphanumeric bytes.|`b'abc123'.isalnum()`|
|`bytes.isalpha()`|Method|Tests ASCII alphabetic bytes.|`b'abc'.isalpha()`|
|`bytes.isascii()`|Method|Tests ASCII bytes.|`b'abc'.isascii()`|
|`bytes.isdigit()`|Method|Tests ASCII digits.|`b'123'.isdigit()`|
|`bytes.islower()`|Method|Tests ASCII lowercase.|`b'abc'.islower()`|
|`bytes.isspace()`|Method|Tests ASCII whitespace.|`b' \t'.isspace()`|
|`bytes.istitle()`|Method|Tests ASCII title case.|`b'Hello'.istitle()`|
|`bytes.isupper()`|Method|Tests ASCII uppercase.|`b'ABC'.isupper()`|
|`bytes.join(iterable)`|Method|Joins byte sequences.|`b','.join([b'a',b'b'])`|
|`bytes.ljust(width[, fillbyte])`|Method|Left-justifies bytes.|`b'a'.ljust(3,b'-')`|
|`bytes.lower()`|Method|Lowercases ASCII bytes.|`b'ABC'.lower()`|
|`bytes.lstrip([bytes])`|Method|Removes leading bytes.|`b' a'.lstrip()`|
|`bytes.maketrans(from, to)`|Static method|Creates translation table.|`bytes.maketrans(b'a',b'b')`|
|`bytes.partition(sep)`|Method|Splits into three parts.|`b'a=b'.partition(b'=')`|
|`bytes.removeprefix(prefix)`|Method|Removes matching prefix.|`b'Test'.removeprefix(b'T')`|
|`bytes.removesuffix(suffix)`|Method|Removes matching suffix.|`b'a.py'.removesuffix(b'.py')`|
|`bytes.replace(old, new[, count])`|Method|Replaces subsequences.|`b'aaa'.replace(b'a',b'b',2)`|
|`bytes.rfind(sub[, start[, end]])`|Method|Returns last index or `-1`.|`b'abca'.rfind(b'a')`|
|`bytes.rindex(sub[, start[, end]])`|Method|Returns last index or errors.|`b'abca'.rindex(b'a')`|
|`bytes.rjust(width[, fillbyte])`|Method|Right-justifies bytes.|`b'a'.rjust(3,b'-')`|
|`bytes.rpartition(sep)`|Method|Partitions at last separator.|`b'a=b=c'.rpartition(b'=')`|
|`bytes.rsplit(sep=None, maxsplit=-1)`|Method|Splits from right.|`b'a,b,c'.rsplit(b',',1)`|
|`bytes.rstrip([bytes])`|Method|Removes trailing bytes.|`b'a '.rstrip()`|
|`bytes.split(sep=None, maxsplit=-1)`|Method|Splits byte sequence.|`b'a,b'.split(b',')`|
|`bytes.splitlines(keepends=False)`|Method|Splits into lines.|`b'a\nb'.splitlines()`|
|`bytes.startswith(prefix[, start[, end]])`|Method|Tests prefix.|`b'abc'.startswith(b'a')`|
|`bytes.strip([bytes])`|Method|Removes edge bytes.|`b' a '.strip()`|
|`bytes.swapcase()`|Method|Swaps ASCII case.|`b'Py'.swapcase()`|
|`bytes.title()`|Method|Returns ASCII title case.|`b'hello world'.title()`|
|`bytes.translate(table, /, delete=b'')`|Method|Translates/deletes bytes.|`b'abc'.translate(None,b'b')`|
|`bytes.upper()`|Method|Uppercases ASCII bytes.|`b'abc'.upper()`|
|`bytes.zfill(width)`|Method|Pads numeric bytes with zeros.|`b'42'.zfill(5)`|

### `bytearray`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`bytearray.append(x)`|Method|Appends byte value.|`b.append(65)`|
|`bytearray.capitalize()`|Method|Capitalizes first ASCII byte.|`bytearray(b'abc').capitalize()`|
|`bytearray.center(width[, fillbyte])`|Method|Centers with padding.|`bytearray(b'a').center(3,b'-')`|
|`bytearray.clear()`|Method|Removes all bytes.|`b.clear()`|
|`bytearray.copy()`|Method|Returns shallow copy.|`c = b.copy()`|
|`bytearray.count(sub[, start[, end]])`|Method|Counts subsequences.|`bytearray(b'banana').count(b'an')`|
|`bytearray.decode(encoding='utf-8', errors='strict')`|Method|Decodes to string.|`bytearray(b'abc').decode()`|
|`bytearray.endswith(suffix[, start[, end]])`|Method|Tests suffix.|`bytearray(b'a.py').endswith(b'.py')`|
|`bytearray.expandtabs(tabsize=8)`|Method|Expands tabs.|`bytearray(b'a\tb').expandtabs(4)`|
|`bytearray.extend(iterable)`|Method|Appends byte values.|`b.extend(b'cd')`|
|`bytearray.find(sub[, start[, end]])`|Method|Returns index or `-1`.|`bytearray(b'abc').find(b'b')`|
|`bytearray.fromhex(string)`|Class method|Creates bytearray from hex.|`bytearray.fromhex('41 42')`|
|`bytearray.hex([sep[, bytes_per_sep]])`|Method|Returns hexadecimal string.|`bytearray(b'AB').hex()`|
|`bytearray.index(sub[, start[, end]])`|Method|Returns index or errors.|`bytearray(b'abc').index(b'b')`|
|`bytearray.insert(i, x)`|Method|Inserts byte value.|`b.insert(0,65)`|
|`bytearray.isalnum()`|Method|Tests ASCII alphanumeric bytes.|`bytearray(b'abc1').isalnum()`|
|`bytearray.isalpha()`|Method|Tests alphabetic bytes.|`bytearray(b'abc').isalpha()`|
|`bytearray.isascii()`|Method|Tests ASCII bytes.|`bytearray(b'abc').isascii()`|
|`bytearray.isdigit()`|Method|Tests digit bytes.|`bytearray(b'123').isdigit()`|
|`bytearray.islower()`|Method|Tests lowercase form.|`bytearray(b'abc').islower()`|
|`bytearray.isspace()`|Method|Tests whitespace bytes.|`bytearray(b' ').isspace()`|
|`bytearray.istitle()`|Method|Tests title case.|`bytearray(b'Hello').istitle()`|
|`bytearray.isupper()`|Method|Tests uppercase form.|`bytearray(b'ABC').isupper()`|
|`bytearray.join(iterable)`|Method|Joins byte sequences.|`bytearray(b',').join([b'a',b'b'])`|
|`bytearray.ljust(width[, fillbyte])`|Method|Left-justifies bytes.|`bytearray(b'a').ljust(3,b'-')`|
|`bytearray.lower()`|Method|Lowercases ASCII bytes.|`bytearray(b'ABC').lower()`|
|`bytearray.lstrip([bytes])`|Method|Removes leading bytes.|`bytearray(b' a').lstrip()`|
|`bytearray.maketrans(from, to)`|Static method|Creates translation table.|`bytearray.maketrans(b'a',b'b')`|
|`bytearray.partition(sep)`|Method|Splits into three parts.|`bytearray(b'a=b').partition(b'=')`|
|`bytearray.pop([i])`|Method|Removes and returns byte.|`b.pop()`|
|`bytearray.remove(x)`|Method|Removes first byte value.|`b.remove(65)`|
|`bytearray.removeprefix(prefix)`|Method|Removes matching prefix.|`bytearray(b'Test').removeprefix(b'T')`|
|`bytearray.removesuffix(suffix)`|Method|Removes matching suffix.|`bytearray(b'a.py').removesuffix(b'.py')`|
|`bytearray.replace(old, new[, count])`|Method|Replaces subsequences.|`bytearray(b'aaa').replace(b'a',b'b',2)`|
|`bytearray.reverse()`|Method|Reverses in place.|`b.reverse()`|
|`bytearray.rfind(sub[, start[, end]])`|Method|Returns last index or `-1`.|`bytearray(b'abca').rfind(b'a')`|
|`bytearray.rindex(sub[, start[, end]])`|Method|Returns last index or errors.|`bytearray(b'abca').rindex(b'a')`|
|`bytearray.rjust(width[, fillbyte])`|Method|Right-justifies bytes.|`bytearray(b'a').rjust(3,b'-')`|
|`bytearray.rpartition(sep)`|Method|Partitions at last separator.|`bytearray(b'a=b=c').rpartition(b'=')`|
|`bytearray.rsplit(sep=None, maxsplit=-1)`|Method|Splits from right.|`bytearray(b'a,b,c').rsplit(b',',1)`|
|`bytearray.rstrip([bytes])`|Method|Removes trailing bytes.|`bytearray(b'a ').rstrip()`|
|`bytearray.split(sep=None, maxsplit=-1)`|Method|Splits byte sequence.|`bytearray(b'a,b').split(b',')`|
|`bytearray.splitlines(keepends=False)`|Method|Splits into lines.|`bytearray(b'a\nb').splitlines()`|
|`bytearray.startswith(prefix[, start[, end]])`|Method|Tests prefix.|`bytearray(b'abc').startswith(b'a')`|
|`bytearray.strip([bytes])`|Method|Removes edge bytes.|`bytearray(b' a ').strip()`|
|`bytearray.swapcase()`|Method|Swaps ASCII case.|`bytearray(b'Py').swapcase()`|
|`bytearray.title()`|Method|Returns ASCII title case.|`bytearray(b'hello world').title()`|
|`bytearray.translate(table, /, delete=b'')`|Method|Translates/deletes bytes.|`bytearray(b'abc').translate(None,b'b')`|
|`bytearray.upper()`|Method|Uppercases ASCII bytes.|`bytearray(b'abc').upper()`|
|`bytearray.zfill(width)`|Method|Pads numeric bytes with zeros.|`bytearray(b'42').zfill(5)`|

### `memoryview`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`memoryview.cast(format[, shape])`|Method|Casts format or shape.|`memoryview(b'abcd').cast('B')`|
|`memoryview.hex([sep[, bytes_per_sep]])`|Method|Returns hexadecimal representation.|`memoryview(b'AB').hex()`|
|`memoryview.release()`|Method|Releases underlying buffer.|`view.release()`|
|`memoryview.tobytes(order='C')`|Method|Copies contents to bytes.|`memoryview(b'ab').tobytes()`|
|`memoryview.tolist()`|Method|Converts values to lists.|`memoryview(b'ab').tolist()`|
|`memoryview.toreadonly()`|Method|Returns read-only view.|`view.toreadonly()`|

### `set`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`set.add(elem)`|Method|Adds element.|`s.add(3)`|
|`set.clear()`|Method|Removes all elements.|`s.clear()`|
|`set.copy()`|Method|Returns shallow copy.|`t = s.copy()`|
|`set.difference(*others)`|Method|Returns non-shared elements.|`{1,2}.difference({2})`|
|`set.difference_update(*others)`|Method|Removes shared elements.|`s.difference_update(t)`|
|`set.discard(elem)`|Method|Removes element if present.|`s.discard(3)`|
|`set.intersection(*others)`|Method|Returns common elements.|`{1,2}.intersection({2,3})`|
|`set.intersection_update(*others)`|Method|Keeps common elements.|`s.intersection_update(t)`|
|`set.isdisjoint(other)`|Method|Tests no overlap.|`{1}.isdisjoint({2})`|
|`set.issubset(other)`|Method|Tests subset relation.|`{1}.issubset({1,2})`|
|`set.issuperset(other)`|Method|Tests superset relation.|`{1,2}.issuperset({1})`|
|`set.pop()`|Method|Removes arbitrary element.|`s.pop()`|
|`set.remove(elem)`|Method|Removes element or errors.|`s.remove(3)`|
|`set.symmetric_difference(other)`|Method|Returns non-common elements.|`{1,2}.symmetric_difference({2,3})`|
|`set.symmetric_difference_update(other)`|Method|Updates to symmetric difference.|`s.symmetric_difference_update(t)`|
|`set.union(*others)`|Method|Returns combined elements.|`{1}.union({2})`|
|`set.update(*others)`|Method|Adds elements from others.|`s.update([2,3])`|

### `frozenset`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`frozenset.copy()`|Method|Returns shallow copy.|`frozenset({1}).copy()`|
|`frozenset.difference(*others)`|Method|Returns non-shared elements.|`frozenset({1,2}).difference({2})`|
|`frozenset.intersection(*others)`|Method|Returns common elements.|`frozenset({1,2}).intersection({2})`|
|`frozenset.isdisjoint(other)`|Method|Tests no overlap.|`frozenset({1}).isdisjoint({2})`|
|`frozenset.issubset(other)`|Method|Tests subset relation.|`frozenset({1}).issubset({1,2})`|
|`frozenset.issuperset(other)`|Method|Tests superset relation.|`frozenset({1,2}).issuperset({1})`|
|`frozenset.symmetric_difference(other)`|Method|Returns non-common elements.|`frozenset({1,2}).symmetric_difference({2,3})`|
|`frozenset.union(*others)`|Method|Returns combined elements.|`frozenset({1}).union({2})`|

### `dict`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`dict.clear()`|Method|Removes all items.|`d.clear()`|
|`dict.copy()`|Method|Returns shallow copy.|`e = d.copy()`|
|`dict.fromkeys(iterable, value=None)`|Class method|Creates dictionary from keys.|`dict.fromkeys(['a','b'],0)`|
|`dict.get(key[, default])`|Method|Gets value without `KeyError`.|`d.get('x',0)`|
|`dict.items()`|Method|Returns key-value view.|`d.items()`|
|`dict.keys()`|Method|Returns key view.|`d.keys()`|
|`dict.pop(key[, default])`|Method|Removes and returns value.|`d.pop('x',None)`|
|`dict.popitem()`|Method|Removes last pair.|`d.popitem()`|
|`dict.setdefault(key[, default])`|Method|Gets or inserts default.|`d.setdefault('x',0)`|
|`dict.update([other], **kwargs)`|Method|Merges items in place.|`d.update({'x':1})`|
|`dict.values()`|Method|Returns value view.|`d.values()`|

### `dict_keys`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`dict_keys.isdisjoint(other)`|Method|Tests whether views are disjoint.|`d.keys().isdisjoint({'x'})`|

### `dict_items`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`dict_items.isdisjoint(other)`|Method|Tests whether views are disjoint.|`d.items().isdisjoint({('x',1)})`|

### `slice`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`slice.indices(length)`|Method|Normalizes slice indices.|`slice(None,None,-1).indices(5)`|

### `property`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`property.getter(fget)`|Method|Replaces property getter.|`prop = prop.getter(func)`|
|`property.setter(fset)`|Method|Replaces property setter.|`prop = prop.setter(func)`|
|`property.deleter(fdel)`|Method|Replaces property deleter.|`prop = prop.deleter(func)`|

### `type`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`type.mro()`|Method|Returns method-resolution order.|`MyClass.mro()`|

### `mappingproxy`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`mappingproxy.copy()`|Method|Returns dictionary copy.|`C.__dict__.copy()`|
|`mappingproxy.get(key[, default])`|Method|Gets value without error.|`C.__dict__.get('x')`|
|`mappingproxy.items()`|Method|Returns key-value view.|`C.__dict__.items()`|
|`mappingproxy.keys()`|Method|Returns key view.|`C.__dict__.keys()`|
|`mappingproxy.values()`|Method|Returns value view.|`C.__dict__.values()`|

### `generator`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`generator.send(value)`|Method|Sends value into generator.|`gen.send(None)`|
|`generator.throw(value)`|Method|Raises exception inside generator.|`gen.throw(ValueError())`|
|`generator.close()`|Method|Terminates generator.|`gen.close()`|

### `coroutine`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`coroutine.send(value)`|Method|Resumes coroutine with value.|`coro.send(None)`|
|`coroutine.throw(value)`|Method|Raises exception inside coroutine.|`coro.throw(ValueError())`|
|`coroutine.close()`|Method|Closes coroutine.|`coro.close()`|

### `async_generator`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`async_generator.asend(value)`|Method|Sends value asynchronously.|`await agen.asend(None)`|
|`async_generator.athrow(value)`|Method|Raises exception asynchronously.|`await agen.athrow(ValueError())`|
|`async_generator.aclose()`|Method|Closes async generator.|`await agen.aclose()`|

### `code`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`code.co_lines()`|Method|Iterates source-line mappings.|`func.__code__.co_lines()`|
|`code.co_positions()`|Method|Iterates source positions.|`func.__code__.co_positions()`|
|`code.replace(**changes)`|Method|Returns modified code copy.|`func.__code__.replace(co_name='new')`|

### `frame`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`frame.clear()`|Method|Clears frame references.|`frame.clear()`|

### `BaseException`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`BaseException.add_note(note)`|Method|Adds exception note.|`exc.add_note('details')`|
|`BaseException.with_traceback(tb)`|Method|Attaches traceback.|`exc.with_traceback(tb)`|

### `BaseExceptionGroup`

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`BaseExceptionGroup.derive(excs)`|Method|Creates replacement exception group.|`group.derive(errors)`|
|`BaseExceptionGroup.split(condition)`|Method|Splits exceptions by condition.|`group.split(ValueError)`|
|`BaseExceptionGroup.subgroup(condition)`|Method|Returns matching subgroup.|`group.subgroup(ValueError)`|

## Error Map

BaseException
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception
    ├── ArithmeticError
    │   ├── FloatingPointError
    │   ├── OverflowError
    │   └── ZeroDivisionError
    ├── AssertionError
    ├── AttributeError
    ├── BufferError
    ├── EOFError
    ├── ImportError
    │   └── ModuleNotFoundError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── MemoryError
    ├── NameError
    │   └── UnboundLocalError
    ├── OSError
    │   ├── BlockingIOError
    │   ├── ChildProcessError
    │   ├── ConnectionError
    │   │   ├── BrokenPipeError
    │   │   ├── ConnectionAbortedError
    │   │   ├── ConnectionRefusedError
    │   │   └── ConnectionResetError
    │   ├── FileExistsError
    │   ├── FileNotFoundError
    │   ├── InterruptedError
    │   ├── IsADirectoryError
    │   ├── NotADirectoryError
    │   ├── PermissionError
    │   ├── ProcessLookupError
    │   └── TimeoutError
    ├── ReferenceError
    ├── RuntimeError
    │   ├── NotImplementedError
    │   └── RecursionError
    ├── StopIteration
    ├── StopAsyncIteration
    ├── SyntaxError
    │   └── IndentationError
    │       └── TabError
    ├── SystemError
    ├── TypeError
    ├── ValueError
    │   └── UnicodeError
    │       ├── UnicodeDecodeError
    │       ├── UnicodeEncodeError
    │       └── UnicodeTranslateError
    └── Warning
        ├── DeprecationWarning
        ├── PendingDeprecationWarning
        ├── RuntimeWarning
        ├── SyntaxWarning
        ├── UserWarning
        ├── FutureWarning
        ├── ImportWarning
        ├── UnicodeWarning
        └── ResourceWarning

