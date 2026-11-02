# Contents
[[#I Python Data Model]]
- [[#1 data model]]
- [[#2 array of sequences ✪]]
- [[#3 dictionaries and sets ✪]]
- [[#4 unicode and binary]]
- [[#5 data class builders]]
- [[#6 unicode and binary]]

[[#II Functions as Objects]]
- [[#7 functions as first class objects]]
- [[#8 functions as first class objects]]
- [[#9 functions as first class objects]]
- [[#10 design patterns with first-class functions]]

[[#III Classes and Protocols]]
- [[#11 a pythonic object]]
- [[#12 special methods for sequences ✪]]
- [[#13 special methods for sequences ✪]]
- [[#14 inheritance]]
- [[#15 more on type hints ✪]]
- [[#16 operator overloading]]

[[#IV Control Flow]]
- [[#17 iterators, generators, classic coroutines ✪]]
- [[#18 with, match, else, scopes ✪]]
- [[#19 concurrency models]]
- [[#20 concurrent executors]]

[[#V Metaprogramming]]
- [[#21 asynchronous programming]]
- [[#22 dynamic attributes and properties ✪]]
- [[#23 attribute descriptors ✪]]
- [[#24 class metaprogramming]]

[[#VI Appendix]]
- [[#object methods]]
- [[#built-in functions]]

---
# I: Python Data Model
> Python is a programming language written or implemented mostly in C where it is called `CPython`. Cython is a separate reference language written in C used to convert Python or Cython into C (for speed or integration). 

**internals**
- Python largely constructed in C $\rightarrow$ `CPython`
- `CPython` is the Python interpreter/compiler (C is a compiled language)

**execution pipeline**
1. The native python script runs 
2. The script is compiled to `bytecode` by the compiler $\rightarrow$ `CPython`
3. The `bytecode` is interpreted via the Python Virtual Machine (PVM), which consists of `C` functions that execute actions on the CPU as machine code. 
$$\boxed{\text{python script runs} \rightarrow \text{compiled to bytecode}\rightarrow \text{bytecode is executed in the machine}}$$
---
## 1: data model
#python #fluentpython #chapter1 #python-data-model #special-methods #dunder-methods #object-model #collections #sequence-protocol #operator-overloading #object-representation #repr #str #truthiness #bool #len #getitem #iterables #collection-api #python-protocols
> The data model consists of special methods (also called dunder methods), which are a series of functions, attributes, or protocols that may be used to create specific objects. 
> 
> The python application program interface (API) can be considered all those variations of special methods forming protocols, which allow python to interact indirectly with the computers CPU. 

- Special methods are effectively the python API 
- The python interpreter primarily interprets all actions and objects as special methods
- The interpreter never interacts with you object directly. 
- You call the interpreter using the special methods
- The interpreter does everything in CPython 
---
## 2: array of sequences ✪
#python #fluentpython #chapter2 #sequences #sequence-types #lists #tuples #list-comprehensions #generator-expressions #sequence-unpacking #pattern-matching #slicing #sorting #arrays #memoryview #numpy #deque #queues #mutability #immutability #container-sequences #flat-sequences
> You can categorize python sequences by container vs. flat sequences or mutable vs. immutable sequences. 
> 
> - *Container Sequences*: Hold an arbitrary number of references. They may also hold nested objects. These include `list`, `tuple`. Because container sequences are arbitrary, they hold internal pointers to pythonic objects meaning$\rightarrow$ the objects are not held in memory. 
> 
> - *Flat Sequences*: Hold a fixed number of references stored in memory for one simply item type i.e. `str`, `bytes`, `array.array`. These items are held directly in memory. Flat sequences are more compact but can only hold primitive machine values i.e. `bytes`, `int`, `floats`. 
> 
> - *Mutable Sequences*: Can be changed in memory (if they exist in memory in the first place) i.e. `list`. 
> 
> - *Immutable Sequences*: Cannot be changed in memory i.e. `tuple`, `str`, `bytes`. 
>   
> The important (native) sequence types are explained below. 
> 

**lists**
Lists are mutable container sequences that use pointers to internal objects. Although lists are mutable, their internal objects may immutable. Lists have a series of in-built qualities that can make them powerful.
- *List comprehensions*: More readable way of writing for-loops when (syntactic sugar). 
```python
#example
names = ['peter', 'louis', 'bryan']
[i + '!' for i in names]
```
```python
>>> ['peter!', 'louis!', 'bryan!']
```

- *Walrus Operator*: Added to smoothen assignment syntax. Often used in conditional statements and list comprehensions
```python
#example
results = [y for x in (y:= exp_func(x) > 0)]

if (n := len(data)) > 10
	print(n)
```

**tuples**
Tuples are immutable flat sequences. The length of an immutable sequence does not change. Items in tuples do not change unless they are immutable objects, themselves. Tuples generally use less memory than a list of the same length and allow python to do some optimizations. 

The `tuple` often stores critical (field) values i.e. an immutable primitive stored in a tuple is useful record data structure. 

Tuples require trailing commas for singular items to indicate the given entry is a tuple, not a int $\rightarrow$ `(55, )` (`(55)` registers as `int`).

**arrays**
The primary array comes from `array` (`array.array`) and can be specified with varying types (`'b'`, `'B'`, `'u'`, `'w'`, `'h'`, `'H'`, `'i'`, `'I'`, `'l'`, `'L'`, `'q'`, `'Q'`, `'f'`, `'d'`$\rightarrow$ see [[note.basic_python]] for more). 

**generator expressions**
A generator expression (genexp) is similar to a list comprehension but saves memory because it yields items one by one using the iterator protocol instead of building a while list just to feed to another constructor. Syntactically it is similar to `tuple`, but uses the syntax of a list comprehension. 
```python
#example
(i for i in range(10))
```
```python
>>> <generator object <genexpr> at 0x10ce54450>
```
Note, the generator expression does not produce values as it yields them sequentially. In other words, the expression is designed to be fed into another expression which takes an iterable (to consume a generator expression, an function must take an iterable).
```python
#example
sum(i for i in range(10))
```
```python
>>> 45
```
A generator expression is a runtime construct: It produces values one at a time, lazily, and doesn’t store them all at once. See more about this in [[#17 Iterators, Generators, Classic Coroutines ✪]]. 

**case/match**
The case and match keywords were introduced in python 3.10 for structural pattern matching made easier. The `match` keywords precedes a series of cases matching a sequential structure of arbitrary arguments. 
```python
#example
def find_location(location):
	match location:
		case ['Toronto', code]: #code is an arbitrary placeholders
			print(f'CITY_NAME: {location[0]} | CITY_CODE:{code}

find_location(('Toronto', 'TO'))
```
```python
>>> CITY_NAME: Toronto | CODE:TO
```
Note, `code` is a placeholder in the sequence structure representing the secondary input term (the name can be anything). The elements of the case sequence must match their respective place in the input (`location`). You can assign multiple terms to match. 

```python
#example
records = [
    ("Toronto", "Canada", 2_794_356, (43.6532, -79.3832)),
    ("Berlin", "Germany", 3_755_251, (52.5200, 13.4050)),
    ("Tokyo", "Japan", 14_094_034, (35.6762, 139.6503)),
    ("Sydney", "Australia", 5_297_089, (-33.8688, 151.2093)),
]

def main():
    for record in records:
        match record:
            case [city, country, _ , (lat, lon)] if lon>0:
                print(f'CODE:\t{city[:3].upper()} [{lat},{lon}]')
            case unmatched :
                print(f'{unmatched} Value not found.')

```

```python
>>> ('Toronto', 'Canada', 2794356, (43.6532, -79.3832)) Value not found.
>>> CODE:	BER [52.52,13.405]
>>> CODE:	TOK [35.6762,139.6503]
>>> CODE:	SYD [-33.8688,151.2093]
```

The term `_` can be used as an arbitrary placeholder. Additionally, `unmatched` is an arbitrary placeholder for a catch-all case when the value does not match. 

As seen above, you can append an optional guard clause (i.e. `if`)  to the back of a sequence pattern. The optional guard evaluates if the pattern matches and the nested block with the action below runs only if the pattern matches and the guard expression is truthy. 

**slice objects and sequences**
The basics of slicing are handled in a separate note. Because sequences can be sliced, it must be noted that the the target of slice object assignment must be an iterable i.e. `[100]` and not `100`. In other words `l[2:5] = 100` causes an error but `l[2:5] = [100]` is fine. 

**using `*`for sequences** 
The asterisk, `*`,`**`  is used to pack or unpack values. 

The single `*args`: The input field takes endless positional arguments, which are sequentially unpacked as a sequence i.e. `def function(*args)` takes `(1, 2, 3, 4, 5, ...)`. Values are unpacked iteratively as a sequence i.e. `fam = {'adam': 25, 'avery': 6}` is unpacked,`print(*fam)` $\rightarrow$ `adam, avery`.

 1. `*` unpacks **iterables** (lists, tuples, strings, etc.) into positional elements.

The double `**kwargs`: The input field takes any number of keyword arguments and unpacks them as parameter inputs i.e. `def name_age(**kwargs)` takes `adam = 25`, `avery = 6`, etc. Keyword arguments are unpacked as parameterized arguments, i.e. `fam = {'adam': 25, 'avery': 6}` $\rightarrow$ `adam = 25, avery = 6`  (which is why `**fam` cannot be printed through `print()`). 

 2. `**` unpacks **mappings** (dicts) into keyword arguments or key–value pairs.

To unpack an object, it must hold the `__iter__` special method or use `__getitem__` as a fallback method. 

**memoryview objects**
`memoryview` is a (memory) efficient way of viewing objects without recalling their entire code by creating a view object. The given object needs to support the buffer protocol. 

The buffer protocol wraps access to an underlying memory array or buffer. Such objects include the built-in `bytes`, `bytearray`, or `array.array` (assuming the `array.array` is configured correctly i.e. type `'l'`). Python provides such facility from two sides:
- on the producer side, a type can export a “buffer interface” which allows objects of that type to expose information about their underlying buffer. This interface is described in the section Buffer Object Structures; for Python see Emulating buffer types.
- on the consumer side, several means are available to obtain a pointer to the raw underlying data of an object (for example a method parameter). For Python see `memoryview`.

```python
#signature
Init signature: memoryview(object)
Docstring:      Create a new memoryview object which references the given object.
Type:           type
Subclasses:  
```

**more**
See the [[#APPENDIX]] for all special methods on list, tuples, etc. 

---
## 3: dictionaries and sets ✪
#Dictionaries #Sets #Mappings #Hashing #Hashable #DictComprehensions #MappingUnpacking #MappingMerging #MappingPatterns #SetDefault #DefaultDict #MissingKeys #UserDict #ChainMap #Counter #ImmutableMappings #DictionaryViews #SetTheory #SetComprehensions #SetOperations

**dict**  
The dictionary is the fundamental mapping type in python. Dictionaries are mutable unhashable mapping types, signified by the use of `__getitem__`, `__setitem__`, `__delitem__`, and  `__iter__` (the dict is unhashable because it cannot itself be used as a key). These special methods define a mapping type like dict: 
1. `__getitem__`
2. `__setitem__`
3. `__delitem__`
4. `__iter__`

Modern syntax includes `dictcomps`, which are simpler ways to establish a dictionary:
```python
#example
dict = {x : x**2 for x in range(5)}
```
```python
>>> `{0 : 0, 1 : 1, 2 : 4, 3 : 9, 4 : 16, 5 : 25}`
```
```python
#example
dict = {key : value for key, value in list}` or the values can be rearranged
```
```python
>>> dict_reverse = {key : value for value, key in list}
```
some other properties of `dict` are described below $\ldots$

**dictionary views**
Dictionary view objects include `dict.keys`, `dict.items`, `dict.values`. These are memory efficient view objects functioning over the `__iter__` protocol. These views implement `__reversed__`, which supports reverse iteration i.e. `reversed(d.keys())`. Dictionary views do not support `d.keys()[0]` because they are not sequences $\rightarrow$ they do not hold `__getitem__`. In other words, you cannot uses slice-based indexing on non-sequences. The following are a description of the view objects. 
- `dict.keys()`: A view object returned by `.keys()` method.
- `dict.values()`: A view object returned by `.values()` method.
- `dict.items()`: A view object returned by `.items()` method.

Views have native support for the following useful operations: Intersection `&`, union `|`, difference `-`, and symmetric difference `^`. You can use `|` to merge a mapping and `|=` to merge a mapping inplace
```python
#example
d1 = {'a':1, 'b':2}
d2 = {'a':5, 'b':2, 'c':3}
d1 | d2
```
```python
>>> {'d':5, 'b':2, 'c':3}
```
```python
#example
d1 = {'a':1, 'b':2}
d2 = {'c':3, 'd':2, 'e':3}
d1 |= d2
d1
```
```python
>>> {'a': 1, 'b': 2, 'c': 3, 'd': 2, 'e': 3}
```

**contains protocol #protocol**
`__contains__` is the special method to determine if an item is `in` something. For subclass dictionary objects (inherit from `dict`), `__contains__` should return `key in self.keys()` so that `__contains__` is not called recursively (`RecursionError`). 

**hashability**
Hashability is a property of an object that signifies it has a hash value, determined by Python’s internal hash() function that remains constant during the object’s lifetime. Hash values are designed to distribute objects efficiently, but they are not guaranteed to be unique. A hash collision occurs when two different objects produce the same hash value. Python handles these collisions internally, though excessive collisions can reduce lookup efficiency.

An object is hashable if it has a hash code, which never changes during its lifetime (for which it needs a `__hash__` method), and can be compared to other objects (it needs an `__eq__` method). Hashable objects which compare equal must have the same has code. 
 
The method `dict.setdefault(key, default_value)` is a useful method to avoid using an if statement when entering a mapping in a dict. `dict.setdefault(key, default_value)` lets you retrieve a key’s value, while inserting a default value if the key does not yet exist avoiding the following:
```python
#example
if key not in d:
    d[key] = []
d[key].append(value)
```

**dictionary and hashability**
A dictionary is unhashable datatype which stores hashable values. A dictionaries keys must be hashable. All of the following are hashable on default (natively hashable):

| Type        | Hashable by default?              |
| ----------- | --------------------------------- |
| `int`       | Yes                               |
| `float`     | Yes                               |
| `complex`   | Yes                               |
| `bool`      | Yes                               |
| `str`       | Yes                               |
| `bytes`     | Yes                               |
| `tuple`     | Yes, if all elements are hashable |
| `frozenset` | Yes                               |
| `range`     | Yes                               |
| `NoneType`  | Yes                               |
| `type`      | Yes                               |
| `object`    | Yes                               |
**collections.defaultdict**
   `defaultdict` employs `__missing__` which is called if the `defaultdict` constructor, `__getitem__`, cannot find the key. In that case, you can define `__missing__` as a fallback value. 
   
   If you return a `dict` index in `__missing__`, it will call `__getitem__` and then `__missing__` recursively $\rightarrow$ there should be a conditional exit criteria.
   
**collections.UserDict**
   Efficient class for user-defined dict subclass with none of the issues of inheriting from `dict` which leads to less error and overwriting.

**sets**
Sets are extremely useful for eliminating duplication or membership testing. 
Sets are created using the constructor `set()` or `{}`. Consider, `x = {5}`, $\rightarrow$ `type(x)` is `set` but, `x = {5 : `1} registers as `dict`.

Set elements must be hashable objects and optimize for membership testing (`x in s`) rather than sequential access (or memory compactness- arrays are better for memory when possible). 
- *setcomps* : setcomps are similar to listcomps but for sets. setcomps are constructed just like listcomps but using `{}` notation (note the lack of the use of colons, which distinguish setcomps from dictcomps). 
```python
#example
t = {i for i in range(10)}
type(t), t
```
```python
>>> (set, {0, 1, 2, 3, 4, 5, 6, 7, 8, 9})
```

****
## 4: unicode and binary
#Unicode #TextVsBytes #CodePoints #Encoding #Decoding #UnicodeErrors #BOM #TextFiles #EncodingDefaults #UnicodeNormalization #CaseFolding #Diacritics #UnicodeSorting #UnicodeCollation #UnicodeDatabase #RegularExpressions #Bytes #Str

**strings and binary**
All string (key) characters are binary codes as interpreted by the computer. Binary is a numerical schema composed of combinatorial sequences of zeros and ones. 
- A bit is the shortest possible binary unit of information representing zero or one (`0 | 1`). 
- A byte consists of eight bits (`00000000`). 
- Computers only understand zero and one.

The bit length of a computer is called the word size or bitness. More precisely, it often refers to the CPU’s native register width. Examples include
- 32-bit system $\rightarrow$ 32-bit word size / 32 bit-architecture
- 64-bit system $\rightarrow$ 64-bit word size / 64 bit-architecture

The character encoding of a given process on a computer varies according to its independent structure. A character encoding is usually called the encoding width. Popular examples include
- ASCII: seven-bit code width [7 bits]
- utf-8 stores information in one to four bytes [8-32 bits] $\approx$ most of the internet
- utf-16 stores information in two or four bytes [16 or 32 bits]

Character encodings can be variable-width encodings or fixed-width encodings. If the encoding is fixed width, then the total amount of unique characters is$$\text{unique character encodings}_{\text{(fixed-encoding)}} = 2^n$$where $n$ is the width of the encoding $\rightarrow$ four bit system has $2^4=16$, number of unique characters. If the encoding is variable-width, then the total number of encodings is $$\text{unique character encodings}_{\text{(variable-encoding)}} = \sum_{i=n}^m2^i$$For example, for variable lengths one-to-four: $2^1+2^2+2^3+2^4=30$. 

**unicode**
Unicode is a universal standard that assigns characters unique identifiers called code points. Encodings such as uft-8 and utf-16 then represent those code points as binary sequences i.e.`A` → unicode code point `U+0041` → utf-8 binary `01000001`. The term U+ means unicode point whereas everything thereafter, XXXX is hexadecimal (unicode uses hexadecimal). 

---
⚠️ The next sections are unimportant as they are mostly about niche details, which do not interact with day-to-day programming in python.

---

**hexadecimal**
A hex digit, short for hexadecimal digit, is one of the 16 symbols `0–9` and `A–F` used in the base-16 hexadecimal number system, where each symbol represents a value from 0 to 15. Hex is a binary-code representation in which each hexadecimal digit corresponds to half a byte, or a nibble, consisting of 4 bits. Therefore, a two-byte code point can be represented using four hexadecimal digits.

Hex can also be understood as a base-16 integer mapping for binary inputs. The hexadecimal symbols correspond to integer values as follows: `1=1`, `2=2`, `3=3`, `4=4`, `5=5`, `6=6`, `7=7`, `8=8`, `9=9`, `A=10`, `B=11`, `C=12`, `D=13`, `E=14`, and `F=15`. Each hexadecimal digit therefore corresponds to a particular 4-bit binary nibble; for example, `4` corresponds to `0100`.

Hex and `<bytes>` objects: Hexadecimal values in Python are often represented through `bytes` objects. A `bytes` object is not itself a sequence of "pure" hexadecimal digits; rather, `bytes` has its own representation scheme for displaying the underlying byte values.

`bytes`: Python `bytes` objects display byte values corresponding to hexadecimal `0x20–0x7E` as their ASCII characters when possible. Other byte values are displayed using the escape indicator `\x`, followed by the two hexadecimal digits representing that byte.

**Little/big-endian**
Endianness refers to the order in which the bytes of multi-byte data are arranged. This becomes relevant when values require more than one byte. A UTF-16 encoded `bytes` object may begin with a BOM, or byte-order mark, indicating the byte ordering used. The sequence `\xff\xfe` indicates little-endian UTF-16. More specific encodings such as `UTF-16LE` and `UTF-16BE` explicitly specify little-endian and big-endian ordering respectively, in which case a BOM does not need to be generated. Some programs also include a BOM with UTF-8; for example, the UTF-8 BOM is represented by `\xef\xbb\xbf`.

**Decimal** 
Decimal provides an ordinary base-10 integer representation of byte values. Because a single byte contains 8 bits, it has 28=2562^8 = 256 possible values, which are represented in decimal as integers from `0` through `255`.

**Normalizing Unicode**
Unicode normalization is used to make canonically equivalent Unicode sequences consistent for comparison. The main normalization forms for this purpose are NFC and NFD.

**Case folding** 
Case folding is used for internationalized string matching. Its goal is to make comparisons more reliable across languages by reducing case-related differences, which is especially useful for user input, search systems, NLP, and other applications involving multilingual text.

**Sorting Unicode text** 
Sorting Unicode text can be complicated by accented characters, diacritics, and language-specific ordering rules. Unicode collation provides a standardized approach for sorting text according to culturally appropriate character-ordering rules.

**Unicode module** 
Unicode-related functionality in Python is largely provided through modules such as `unicodedata`, which exposes information and utilities for working with Unicode characters, including normalization and character metadata.

---
## 5: data class builders
#DataClassBuilders #NamedTuple #TypedNamedTuple #TypeHints #VariableAnnotations #Dataclass #Fields #PostInit #ClassAttributes #InitVar #PatternMatching #ClassPatterns #KeywordPatterns #PositionalPatterns

> A data class is a class used to store information easily. A data class builder is a function used to create a data class without boilerplate code.  Data classes usually include the following boilerplate methods, `__init__()`, `.__repr__()`, and `.__eq__()`. Additionally, data class builders can use type hint syntax for simpler construction. 

> There are three primary data class builders in the python standard library:
> 1. `collections.namedtuple` (from namedtuple)
> 2. `typing.NamedTuple` (from typing)
> 3. `@dataclass` (stock decorator function)
> 
> Usually a data class allows you to implement a field $\rightarrow$ a field is a value that allows you to store information in the data class using typing notation $\rightarrow$ `value : int`. The type hint value has no runtime enforcement but is notated under `__annotations__`. 

**interpreter**
Runs, executes, and compiles your code to machine instructions (CPython $\rightarrow$ C $\rightarrow$ compiled language).  

**static type checker**
Analyzes your code to ensure data types are correct (mypy, pylance). The static type checker does not run your code. It reads and analyzes your code to make suggestions in the background (technically, the static type checker runs continuously in the background as it must at some point analyze your code). The static type checker has no runtime influence. 

**type hints**
Metadata you add to your code to give the static type checker more context (also no runtime influence as its an extension of the static type checker interface). 

**collections.namedtuple()**
```python 
#signature
collections.namedtuple(typename, field_names, *, rename=False, defaults=None, module=None)¶
```

- factory function, pseudoclass tuple object
- can be relatively cumbersome (still better than default `class`)
- constructor takes str, int,  or space-delimited string inputs (`'country province lon lat'` $\approx$ ` ['country', 'province', 'lon', 'lat', 'lat']`)
- does not take kwargs at all
- the types have no runtime support
- Why use? Useful for brevity $\star$ no type checking, lightweight, single-line declaration $\star$ doesn't require creating a full class body, making it cleaner for quick scripts or small utility $\star$ immutable
 
```python
#example
from collections import namedtuple

groceries = namedtuple('Groceries', ['item1', 'item2'])
groceries('cereal', 'cheese')
```
```python
>>> Groceries(item1='cereal', item2='cheese')
```

**typing.NamedTuple**
```python
#signature
NamedTuple(typename, fields=None, /, **kwargs)
```
- Typed version of `namedtuple`. Data class inherits from `NamedTuple`. 
- Use fields (typed values in the class to assign values)
- `NamedTuple` instances are subclasses of `tuple`
- Field names and types are defined using variable annotations inside the class body.
- Fields are immutable after instantiation (like regular tuples)
- Instances support dot-notation (`nt.fieldname`) as well as indexing (`nt[0]`)
- Reading from`__annotations__` is not recommended $\rightarrow$ use `inspect.get_annotations(MyClass)` or `typing.get_type_hints(MyClass)`
- Why use? Useful for lightweight, typed, immutable record structures $\star$ need type hints for static type checking $\star$ Want default values or methods in the tuple class $\star$ Are building more complex data structures that benefit from clearer typing and IDE support $\star$ Immutable
```python
#example
from typing import NamedTuple
class GroceryItem(NamedTuple):
    item : str
    price : float
    tax: float = 1.13 #basic field with a default value

cheese = GroceryItem('BlackDiamondCheese', 5.99)
cheese
    
```
```python
>>> GroceryItem(item='BlackDiamondCheese', price=5.99, tax=1.13)
```
```python
#example
GroceryItem.__annotations__
```
```python
>>> {'item': str, 'price': int, 'tax': int}
```

**dataclasses.dataclass** (`@dataclass`)
```python
#signature
@dataclasses.dataclass(*, init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False, match_args=True, kw_only=False, slots=False, weakref_slot=False)
```
- decorator function (function that takes another function)
- autogenerates special methods `__init__`, `__repr__`, `__eq__` based on type-annotated fields 
- class attributes can be instantiated regularly using assignment `class_value = value`, and will still share the assignment (as they are defined on the class)
- class attributes should be type hinted using `ClassVar`,  which static type checkers can read and understand i.e. `class_var : ClassVar[str] = 'bread'` (class attributes must be bound to a value even if the value is `None`and can only be accessed through the class instance object $\rightarrow$ leaving a class attribute unassigned causes error)
- you cannot assign mutable objects (ValueError) to attributes (class or instance) as it will be shared between different instances $\rightarrow$ to assign lists use the constructor `dataclasses.field*`
- Why Use? mutable $\star$ default values, methods, or inheritance $\star$ greater control over behaviour $\star$ more than just a record $\star$ scaffolding $\rightarrow$ quickly start a new project or module $\star$ intermediate representation $\rightarrow$ convert to dict-like objects for serialization

```python
#example
from dataclasses import dataclass
from typing import ClassVar
@dataclass
class GroceryItem2():
    item : str
    price : int
    store_tax: int = 0.05
    federal_tax : ClassVar[int] = 0.13 #class attribute
    
bread = GroceryItem2('WonderBread', 10.00)
cheese = GroceryItem2('BlackDiamond', 6.90)
bread, bread.federal_tax, cheese.federal_tax

```
```python
>>> (GroceryItem2(item='WonderBread', price=10.0, store_tax=0.05), 0.13, 0.13)
```

The difference between `typing.NamedTuple` and `@dataclass` is that `@dataclass` allows defaults, default factories, and optional fields, whereas `NamedTuple` requires all non-default fields for instantiation. `@dataclass` also allows for class inheritance, while inheritance with `NamedTuple` is discouraged. Finally, `@dataclass` is a full decorated class, whereas `NamedTuple` inherits from `tuple` and is generally smaller than a `@dataclass`.

**typing.ClassVar**
Special type construct to mark class variables. Tells the `@dataclass` decorator to hide typed assignments from `__init__`, `__repr__` and `__eq__` (respectively, don’t expect it as an argument when creating instances, don’t show it in `__repr__`, don’t use it in equality comparisons (`__eq__`). Note `ClassVar` is functionally similar to normal assignment (`anime = set()`) with the exceptions listed above (ex. syntax `anime : ClassVar[set[string]] = set()`) . Additionally, it uses generics syntax i.e. `[]`. 

**typing.InitVar**
`InitVar[]` (from `dataclasses`) is used in `@dataclass` to pass temporary initialization-only values into the special `__post_init__` hook. These values are not stored on the instance and are typically used for validation, dependency injection, or other setup logic.

**\__post_init__**
The special method `__post_init__` is a user-defined hook that is automatically called after the generated `__init__`. It can access all instance attributes and any `InitVar[]` inputs passed during initialization. It is commonly used for validation, transformation, derived-field setup, or other post-initialization logic.

**dataclasses.field**
```python
#signature
dataclasses.field(*, default=MISSING, default_factory=MISSING, init=True, repr=True, hash=None, compare=True, metadata=None, kw_only=MISSING, doc=None)
```
- default : general constructor
- default_factory : mutable type
---
## 6: references, mutability, recycling
#ObjectReferences #Mutability #ObjectIdentity #Equality #Aliasing #ShallowCopy #DeepCopy #FunctionParameters #MutableDefaults #DefensiveProgramming #GarbageCollection #ObjectRecycling #Immutability
**variable names**
Variable names are labels assigned to objects. Any assignment is form of pointer to an actual object and not the object itself.  We assign names (labels) to objects although the syntax is reverse $\rightarrow$ `x = 5`. Objects can hold multiple names such that changes to an underlying object are represented in multiple names pointing to the same object as seen below.

```python
#example
a = (1, 2, [5, 6])
b = a
a[-1].append(7)
```
```python
>>> b = (1, 2, [5, 6, 7,])
```

**equality operator \==**
The equality operator, \==, is syntactic sugar for `__eq__`, which is used for equality comparisons. Always use \== when possible as is has complex behaviour. See below. 

**is keyword**
The `is` keyword is ID validation $\rightarrow$ returns an objects memory address. In CPython values between -5 and 256 are interned, meaning that they refer to the same object in memory as seen below. Try to avoid using `is` and use it primarily for validation of `None`, `True`, and `False` and singletons. 

```python
#example
a = 5
c = 5
a is c
```
```python
>>> True
```

**assignment** 
Assignment is fundamentally a reference to an object and does create a copy or a new object in the process. If you assign b $\rightarrow$ a and change a, then b will be affected. 

**augmented assignment**
In this context, augmented assignment refers to assignment using the += or \*= operators which are syntactically equal to the below pseudo code. 
```pseudocode
#pseudocode example
x += 10 is equal to x = x + 10
```

Augmented assignment creates new objects if the lefthand variable is (assigned to) an immutable object but may modify a mutable object in-place. 

Consider that in the the following example, the augmented assignment does not change the original object `(1, 2, 3)`, but generates a new tuple object which points to `(1, 2, 3, 4, 5, 6)` and (in the absence of any previous assignment) the object is collected by the garbage collector (compiler) and deleted from memory. In other words, the original object

```python
#example
a = (1, 2, 3) # collected by garbage collector ~ destroyed
a =+ (4, 5, 6) # creates new object with variable name a
```

**python garbage collector**
The garbage collector works in the compiler. The `del` statement (not a function)  deletes references and not objects. Objects in python are only ever deleted insofar as all references to them are deleted and the object is unreachable. 

**function parameters**
Function parameters work as aliases. When you pass a variable to a function, you create a new reference (this is called call by sharing in programming terms). 

A function may change any mutable object passed as a parameter but it cannot change the identity of those objects. Mutable variables passed to functions will not have their memory address changed even if they are changed. 

Mutable objects keep their ID even when changed in place,  whereas immutable objects cannot be changed, so any change returns a new object and the variable then refers to a new object with a new ID.
```python
#example
x = [1, 2]
id(x)
```
```python
>>> 554998
```
```python
#example
x += [3]
id(x)
```
```python
>>> 554998
```

**shallow vs deep copies**
When an object is copied, Python creates a shallow copy by default, meaning only the outer mutable object is copied. Objects contained inside it are not recursively copied; instead, the new object keeps references to the same nested objects. For example, if a list containing a tuple is copied, the outer list can be modified without affecting the original list, but the nested objects may still be shared between both copies.

A deep copy can be created with `copy.deepcopy()`. Unlike a shallow copy, it recursively copies nested objects instead of keeping references to the same inner objects. Types such as `list`, `dict`, `tuple`, `set`, and custom class instances can contain nested objects, and the effective “depth” depends on how many layers of nested objects they contain.

**mutable defaults**
In this context mutable defaults refer to  function assignments $\rightarrow$`def function(x=mutable)`. Avoid default assignments to mutable objects as it risks sharing objects between different instances. 

```python
#example
class RandClass:
    def __init__(self, items=[]):
        self.items = items

tt = RandClass()
tt.items.append('x')
tt.items
ff = RandClass()
tt.items, ff.items
```
```python
>>> (['x'], ['x'])
```

Never use mutable defaults. Instead, assign a parameters default to`None` and use a conditional statement.

```python
#example
class RandClass:
    def __init__(self, items=None):
        self.items = [] if items is None else items

tt = RandClass()
tt.items.append('x')
tt.items
ff = RandClass()
tt.items, ff.items
```
```python
>>> (['x'], [])
```

Additionally, you can also use `self.items = [] if items is None else list(items)`, because `list(items)` creates a new list, however this will only make shallow copy. To avoid this you can use a deep copy $\rightarrow$ `self.items = [] if items is None else copy.deepcopy(items)`. 

---
# II : Functions as Objects
---
## 7 : functions as first class objects
#FirstClassFunctions #FunctionObjects #HigherOrderFunctions #AnonymousFunctions #CallableObjects #CallableTypes #KeywordOnlyParameters #PositionalOnlyParameters #FunctionalProgramming #OperatorModule #Functools #PartialFunctions

> Functions are first-class objects $\rightarrow$ they do not just produce values, but operate on functions, iterables, etc. A function that takes a function or returns a function is a higher order function. See example below. 
```python  
#example
import reduce from functools

map(func, iterable) # applies fnc to iterable

filter(func, iterable) #return iterator yielding only those items for which function(item) is true (often used with lambda)

reduce(func, iterable) #combines elements cumulatively using binary func(func must be a first-class object)

```
```python
# example
t = filter(lambda x: x > 5, range(10))
list(t)
```
```python
>>> [6, 7, 8, 9]
```

> Note, the implication of functions as first-class-objects (or citizens) means that functions adhere to all to the other rights of objects (`object`) and can be passed in other functions which are higher-order-functions, can be decorated (i.e. behaviour modified and can be reassigned. 

```python
#example
adds = sum 
adds([1, 5])
```
```python
>>> 6
```

**lambda**
The keyword `lambda` is syntactic sugar for creating a small anonymous function without using `def`. A lambda expression creates a function object just like a function defined with `def`. Lambda is intentionally limited to a single expression, so it becomes awkward or impossible to use for complex multi-step logic $\rightarrow$ `lambda word : word[::-1]` $\rightarrow$ reverses all arguments passed to `word`. 

```pseudocode
#pseudocode example
lambda x: x * 2

is equal to 

def func(x): 
	return x * 2
```

The following shows how to use the lambda keyword. The assignment is necessary to use the function generally, but lambda is also commonly passed to a function parameter to avoid needing to write out the whole function. 

```python
#example
make_float = lambda l : float(l)
make_float(100)
```
```python
>>> 100.00
```

**callable**
Any object that uses `__call__` $\rightarrow$ syntax $\rightarrow$ `()` i.e. `object()`. Example of callables include functions, methods, classes, class instances with `__call__` (`functools.partial`). You can use use `callable()` to determine if an object has`__call__` on it $\rightarrow$ returns a bool. 

```python
#signature
callable(obj, /)
Return whether the object is callable (i.e., some kind of function).
```

**\__call__**
Special method allows you to turn function/method into callable i.e. `car` $\rightarrow$ `car()`. 


**/**
The / in a function signature separates positional-only parameters (before /) from parameters that may be passed positionally or by keyword (after /).
```python
#examle
map(self, /, *args, **kwargs)
```

**operator module**
useful for arithmetic operators as functions (quite exhaustive support- see reference)

**operator.itemsgetter**
```python
#signature
itemgetter(self, /, *args, **kwargs)
Return a callable object that fetches the given item(s) from its operand.
```
```python
#example
itemgetter(1)('ABCDEFG')
```
```python
>>>'B'
```
```python
#example
itemgetter(1, 3, 5)('ABCDEFG')
```
```python
>>> ('B', 'D', 'F')
```

---
## 8: type hints in functions
#GradualTyping #Mypy #StrictTyping #TypeHints #Annotations #Any #UnionTypes #OptionalTypes #GenericCollections #TupleTypes #GenericMappings #ABCs #Iterable #TypeVar #Generics #StaticProtocols #Callable #NoReturn #VariadicParameters

> Type hints are code annotations used by the static type checker as metadata to suggest corrections to code. Pythons primary static type checker is mypy. The below are type hints used for specific annotations. 

**Mypy**
Mypy is the default structural type reader. This is the static type checker which runs in the background to analyze and read types. You can configure mypy in varying ways. 

1. `mypy project_file.py` performs a standard static type check.
2. `mypy --disallow-incomplete-defs project_file.py` reports functions that have some annotations but are not fully annotated. 
3. `mypy --disallow-untyped-defs project_file.py` requires every function definition to be fully annotated. 
4. `mypy --strict project_file.py` enables a larger collection of strict checks, including both of these options.

**using "\[]" with types**
You can type annotate the contents of a specific container type using using `[]` $\rightarrow$ `tuple[str, float]`, `list[int]`, or `set[str]` $\ldots$ rule $\rightarrow$ `[T]` where `T` is `type(object)`. 

**type aliasing**
Type annotations can be assigned to variables for brevity i.e. `x = Tuple[float, float]` s.t. you can do `y : x` where `x` is `Tuple[float, float]`. 

**arrow ->**
Use the arrow term `-> (type)` to type hint the return type of a function. 

```python
#example
def geohash(lat_lon: tuple[float, float]) -> str:...
```

---
> Certain libraries have specific type hints to import and be used for more specific metadata for the static type checker to use. 

**collections.abc.Iterable**
`Iterable[T]` accepts any object that can be iterated over and yields values of type `T`, regardless of the concrete container type. 
```python
#example
Iterable[tuple[str, str]]
```

**collections.abc.mappings**
Use `Mapping[K, V]` when a function only needs read-only mapping behaviour rather than requiring a concrete `dict`. This makes the annotation more general because any compatible mapping type may be accepted.
```python
#example
def process(data: Mapping[str, int]) -> None:
```

**typing.TypeVar**
`TypeVar`, from `typing`, represents a type that must remain consistent across annotations. It is useful when the return type must correspond to the type supplied as input (without that input being specified explicitly beforehand ).

Additionally, you can parameterize `TypeVar` $\rightarrow$ a constrained `TypeVar` restricts `T` to specific alternatives whereas $\rightarrow$ a bounded `TypeVar` accepts a specified type and compatible subtypes:

```python
#example
T = TypeVar("T")
T = TypeVar("T", float, Decimal) #constrained
T = TypeVar("T", bound=Hashable) #bounded

def sample(a: Sequence[T], b: int) -> list[T]: #type(T) must be same in Sequence and list
```

**typing.Protocol**
Provides static structural subtyping (typing patterns explained below). An object is accepted based on the operations it supports rather than explicit inheritance. In the example below, a class does not need to inherit from `SupportsLessThan` $\rightarrow$ it only needs a compatible `__lt__` method. A protocol can also be used as a `TypeVar` bound:

```python
#example
class SupportsLessThan(Protocol):
    def __lt__(self, other: Any) -> bool: ...
    
T = TypeVar("T", bound=SupportsLessThan) #does the same as above
```

**collections.abc.Callable**
Import with `from collections.abc import Callable`. Below, `fn` must accept an `int` and return a `bool` (any object implementing `__call__` is a callable). 

```python
#signature
Callable[[ParamType1, ParamType2, ...], ReturnType] #<no docstring>
```
```python
#example
def process(
    data: list[int],
    fn: Callable[[int], bool]
) -> list[int]:
    ...
```

**reveal_type()**
**`reveal_type()`** is a special static type-checker construct used to display the type inferred for an expression. Mypy reports the inferred type during static analysis. It is commonly placed inside `if TYPE_CHECKING:`, where `TYPE_CHECKING` from `typing` is treated as `True` by static type checkers but evaluates to `False` during normal runtime execution.

```python
#example
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    reveal_type(function_output)
```
```
>>> Revealed type is "typing.Iterator[tuple[builtins.int, builtins.str]]"
```

---
> The next topics cover theoretical aspects of typing and typing systems. Some of these concepts extend beyond different programming languages. 

**subtyping systems**
The subtyping system of a language controls when one type is considered a subtype of another and can be used in its place. 
1. nominal subtyping: types requires explicit instruction to be subtypes of one another $\rightarrow$ objects are intended to interact only with types they are explicitly designed to support
2. structural subtyping: types do not require explicit declaration of a subtype relationship $\rightarrow$ an object is considered a subtype of another type if it provides the required attributes/methods of that type

The above are the primary subtyping paradigms. Python is a mixture of both of these paradigms as it has both nominal and structural subtyping. Often mentioned in relation to the above, duck typing is the runtime equivalent of structural subtyping. That is, duck typing is a form of typing where objects interact based on what they can do, not on their explicitly declared or intended type. A type is effectively defined by its supported behaviour and capabilities rather than by what it was originally built to represent.

**optional defaults**
When `None` is a valid parameter value, annotate the parameter as the intended type combined with `None`. The older equivalent is `typing.Optional[str]`. In the example below, the syntax `str | None` uses the `or` operator `|` to type annotate plural is either `str` or `None` and defaults to `None`. 

```python
#example
def show_count(
    count: int,
    singular: str,
    plural: str | None = None
    ...
```

**ariadic parameters**

Variadic parameters accept a variable number of arguments. Their annotations apply to each collected argument value.

```python
def func(*args: str) -> None: # args is tuple[str, ...]
    ...
def func(**kwargs: Any) -> None: # kwargs is dict[str, Any]
    ...
```

**inputs and returns**
Inputs should be open, outputs should be closed $\rightarrow$ return type annotations should not be `Iterable` but actual defined types i.e. `str`. 

**variance**
Chapter 8 provides an early introduction to variance, which describes how subtype relationships behave when types are used as parameters of generic types such as `Callable`. In Python's static type system, `int` is treated as a subtype of `float`, and `float` as a subtype of `complex` (`int <: float <: complex`). These are static typing relationships and do not imply runtime inheritance; for example, `issubclass(int, float)` is `False`.

For `Callable`, return types are covariant: a callable may return a more specific subtype than the declared return type. Parameter types are contravariant: a callable may accept a more general supertype than the declared parameter type. Therefore, if `update` expects `probe: Callable[[], float]`, a function returning `int` is acceptable because `int <: float`. If it expects `display: Callable[[float], None]`, a function accepting only `int` is not acceptable because `update` may pass any `float`; however, a function accepting `complex` is acceptable because it can also accept a `float`.

Positional-only parameters are marked with `/` and are enforced by Python itself at runtime. PEP 484 also defines a two-leading-underscore convention for positional-only parameter names, such as `__value`, which static type checkers such as Mypy understand and enforce.

```python
from collections.abc import Callable

def update(
    probe: Callable[[], float],
    display: Callable[[float], None]
) -> None:
    temperature = probe()
    display(temperature)


# Covariance: int <: float, so returning int is safe.
def temperature() -> int:
    return 15


# Contravariance: accepting only int is too specific.
def display_wrong(temperature: int) -> None:
    print(temperature)


# Accepting complex is more general and therefore safe.
def display_ok(temperature: complex) -> None:
    print(temperature)


update(temperature, display_ok)       # accepted by type checker
update(temperature, display_wrong)    # type-checking error


# Positional-only parameter
def func(value: int, /) -> None:
    ...


# PEP 484 positional-only convention used by static type checkers
def typed_func(__value: int) -> None:
    ...
```

---
## 9: decorators and closures ✪
> Decorators are callables designed to modulate function behaviour signified with the @ character. Because they modify function behaviour, decorators return functions themselves $\rightarrow$ a decorator may replace a function with another function (the \@ character is syntactic sugar). 
> 
> Decorators are executed immediately before runtime (difference between runtime and import time $\rightarrow$ import time comes before runtime).
```
#pseudocode example

@decorator
def func():
    ...
    
is syntactically equal to

def func():
    ...

func = decorator(func)
```

**decorator**
A decorator takes an original function as input and returns another function, usually called a wrapper, which replaces the original function. After decoration, attributes such as `__name__`, `__doc__`, and `__annotations__` belong to the wrapper function unless they are copied from the original function with `@functools.wraps(func)`. In the above example, `func` becomes whatever `decorator(func)` returns destroying `func` in the process. 

```python
#example
def decorator(func): # func = original function
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs) #

    return wrapper # decorator returns the wrapper

@decorator
def greet(name):
    """Return a greeting."""
    return f"Hello, {name}"
```

**decorator factory**
A decorator is itself a function, so a decorator factory can be called like any other function, e.g. `@decorator(param_a=...)`. A decorator factory prevents the destruction of the original function while allowing for additional parametrization (most imported decorators are decorator factories). The call returns the actual decorator that will receive the function being decorated. 

A decorator factory adds an extra outer function around the general decorator so that options or keyword arguments can be supplied. The outer function receives the decorator parameters and returns the decorator; the middle function, e.g. `def decorator(func):`, receives the function being decorated and returns the wrapper; the inner function contains the actual wrapper logic and returns the result of the wrapped function.

```python
#example decorator factory
def decorator_factory(option=True): # outer: receives decorator parameters

    def decorator(func): # middle: receives function being decorated

        def wrapper(*args, **kwargs): # inner: wrapper logic
            result = func(*args, **kwargs)

            if option:
                print("Option enabled")

            return result

        return wrapper # middle returns wrapper

    return decorator # outer returns decorator

@decorator_factory(option=True)
def greet(name):
    return f"Hello, {name}"
```

**variable scoping / scope**
Determines in what environmental context a variable exists; local, global, or nonlocal. This is important because it dictates how decorators access information within their scope.

**local scope**
This the scope defined for variables defined within a function. Under `def function(a, b): ...` $\rightarrow$ variables `a` and `b` are local and do not exist outside the function's scope.  Variables in this scope are local variables. 

**global scope**
This is the scope for direct assignments outside of functions or classes i.e. `a = 10` $\rightarrow$ `a` is a global variable. Variables in this scope are global variables. 

**closure**
A closure is formed when an inner function references variables from an enclosing function’s scope and the enclosing function returns that inner function. The returned inner function retains access to those free variables even after the enclosing function has finished executing.

**nested/inner function**
Another word for inner function $\rightarrow$ the function within the enclosure of an outer function. 

**free variable** 
A free variable is a variable accessible to a function but defined in its surrounding enclosure. In the example below, `x` is accessed by `inner()` but defined in `outer()`. If a variable is defined globally (in the global score $\rightarrow$ `global()`) it not considered a free variable. A free variable must be defined in an enclosing function's scope. 
```python
#example
def outer():
    x = 10 # x is defined in the outer() enclosure

    def inner():
        return x # x is accessed in the inner enclosure
```

**scope conflicts**
The compiler reads the script before it executes the script. During this process, if a name is assigned anywhere within a function, Python generally treats that name as local to that function unless it is declared `global` or `nonlocal`. If a name is not local, Python searches outward according to the LEGB rule. When this happens, assignment conflicts are given preference to local variables. If a local variable does not exist, python first looks at its enclosing scope, and if not found, then looks at the global scope and then the built-in scope. The LEDB rule,$$\boxed{\text{1.Local} \rightarrow \text{2.Enclosing} \rightarrow \text{3.Global} \rightarrow \text{4.Built-in}}$$
An `UnboundLocalError` occurs when Python has classified a variable as local, but the function attempts to read it before that local variable has been assigned. A variable of an outer function used or available to an inner function, is a free variable of the inner function, not a local variable although it may appear in `locals()`. Therefore, `locals()` may include free variables available to that function, but that does not make them locally defined variables even if they show in `locals()`. 

Global variables are different from free variables in that they will not show in `locals()`. In other words, `locals()` may show captured free variables, but it does not include global variables simply because the function accesses them. The augmented assignment operators += or \*= may also cause UnboundLocalError as they modify a variable.

```python
#example 1 - UnboundLocalError
b = 5

def add(a):
	print(a)
	print(b)
	b = 10
	
add(5)
```
```python
>>> UnboundLocalError
```
```python
#example 2
b = 5

def add(a):
	b = 10
	print(a)
	print(b)

add(5)
```
```python
>>> 5
>>> 10
```
```python
#example 3 - global lookup
b = 5

def add(a):
	print(a)
	print(b)
	
add(5)
```
```python
>>> 5
>>> 5
```
```python
#example 4 - locals() can access outer()
def outer():
    x = 10
    def inner():
        print(locals()) #locals returns {'x': 10}
        return x

    return inner
outer()()
```
```python
>>> {'x': 10}
>>> 10
```
```python
#example 5 - locals() cant access global variable
x = 10
def outer():
    def inner():
        print(locals()) #locals returns {'x': 10}
        return x

    return inner
outer()()
```
```python
>>> {}
>>> 10
```

**global statement**
The `global` statement tells Python that a name inside a function refers to the variable in the global scope, rather than creating or modifying a local variable. 
```python
#example
b = 20

def add(a):
    global b #gets access to b = 20
    print(a)
    print(b)
    b = 10

add(5) #avoids causing UnboundLocalError
print(b)
```

**nonlocal statement**
The `nonlocal` statement tells Python that a name inside a function refers to the free variable in its enclosing scope, rather than creating or modifying a local variable. 
```python
def outer():
    x = 10
    def inner():
        nonlocal x # nonlocal x avoid 
        x+=1
        return x

    return inner
outer()()
```


**inspection, \__code__**
Holds metadata, representing the compiled body of a function, keeps the names of the local and free variables $\rightarrow$ returns a types.CodeType object containing the function's compiled bytecode and associated metadata.

| attribute     | description                                                                      |
| ------------- | -------------------------------------------------------------------------------- |
| `co_code`     | compiled bytecode instructions of the function                                   |
| `co_varnames` | names of local variables including parameters                                    |
| `co_freevars` | names of variables captured from enclosing function scopes                       |
| `co_cellvars` | names of local variables captured by nested functions                            |
| `co_consts`   | constants referenced by the compiled code including nested code objects          |
| `co_names`    | names referenced through global or other name-based lookups including attributes |

```python
#example
def outer():
    x = 10
    y = 20

    def inner():
        return x + y

    return inner

f = outer()
f.__code__.co_freevars, f.__closure__, tuple(cell.cell_contents for cell in f.__closure__)
```
```python
>>> (('x', 'y'), (<cell at 0x107750bb0: int object at 0x1044af608>, <cell at 0x107751180: int object at 0x1044af748>),(10, 20))
```

---
> The rest of this chapter is just decorators from the standard library you can use and import. 

**\@functools.wraps(func)**
Is equivalent in purpose to `functools.update_wrapper(wrapper, func)` and copies the original function’s metadata (name, docstring, annotations, etc.) to the wrapper function. Attributes such as `__name__`, `__doc__`, and `__annotations__` are copied from `func`. `functools.wraps` should generally always be used because it preserves the identity/metadata of `func`; although `func = decorator(func)` rebinds the name to the inner wrapper, that new function now impersonates the original `func`. Note, `@functools.wraps(func)` is placed directly above the inner wrapper function.

```python
#example
import functools

def clock(func):
    @functools.wraps(func)
    def clocked(*args, **kwargs):
        return func(*args, **kwargs)
    return clocked

@clock
def add(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b
```

```python
>>> add.__name__
'add'
>>> add.__doc__
'Add two numbers.'
```

**\@functools.cache**
Saves the results of previous invocations of a compute-intensive function; using a cache this way is also called memoization. Cache keys are generated from the function’s hashable input arguments, and each key maps to its corresponding output (~i.e. `.cache` checks whether the given input was already passed and accesses the stored result instead of computing it again). This is especially useful for recursive functions.

```python
#example
from functools import cache

@cache
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
    
fibonacci(10)
```

```python
>>> 55
```

**\@functools.lru_cache**
Works similarly to `@functools.cache`, but `.lru_cache` caps the number of stored entries at `maxsize`; `.lru_cache(maxsize=None)` removes that limit. `typed=True` determines the distinctness of input types: when `typed=False`, some arguments of different types that compare equal may share a cached entry, while `typed=True` caches them separately by type.

```python
#example
from functools import lru_cache

@lru_cache(maxsize=128, typed=False)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
```

```python
>>> fibonacci(10)
55
```

**\@functools.singledispatch**
Is used to modularize functions by creating a registry of implementations (~`.singledispatch` is useful when you want one general function that behaves differently based on varying input types). It can replace unwieldy `if`/`else` or `match/case` structures and is useful when you want 1. varying behaviour by type, 2. a clean API, 3. third-party extension, or 4. to build libraries or frameworks. `.singledispatch` commonly uses type annotations to signify the registered input type. `@functools.singledispatch` is placed on the default function, often annotated with `object`, while `@htmlize.register` is placed on specialized implementations such as `def _(text: str) -> str:`. The specialized functions may have arbitrary names because the generic function is still called through `htmlize()`. Its main advantage is modular extension: different modules can register specialized implementations for the types they support.

```python
#example
from functools import singledispatch

@singledispatch
def htmlize(obj: object) -> str:
    return f"<pre>{repr(obj)}</pre>"

@htmlize.register
def _(text: str) -> str:
    return f"<p>{text}</p>"

@htmlize.register
def _(n: int) -> str:
    return f"<pre>{n}</pre>"
    
    
htmlize("hello")
htmlize(5)
```

```python
>>> '<p>hello</p>'
>>> '<pre>5</pre>'
```
---
## 10 : design patterns with first-class functions
> Python’s first-class functions can simplify or eliminate traditional object-oriented design patterns, particularly the Strategy and Command patterns.

**strategy Pattern**
Pattern uses interchangeable strategy classes, often enforced through an `ABC` with `@abstractmethod`. In Python, strategies can instead be represented directly as functions. Functions can be stored in lists and called dynamically, discovered automatically from a module using `inspect.getmembers(..., inspect.isfunction)`, or registered explicitly using a registry decorator. This removes much of the class hierarchy and boilerplate required by the traditional pattern.

**command pattern**
Can similarly be simplified because Python functions are callable objects. A command executor can store commands with `self.commands = list(commands)` and implement `__call__` to iterate through them and execute each with `command()`, avoiding separate command classes when they are unnecessary.

---
# III: Classes and Protocols
---
## 11: a pythonic object
> Fundamental behaviour expected from python objects and what makes an object pythonic. The example explored in this chapter is called Vector2d. 

```python
#example
from array import array
import math

class Vector2d:
    __match_args__ = ('x', 'y')

    typecode = 'd'

    def __init__(self, x, y):
        self.__x = float(x) # protected variable names
        self.__y = float(y)

    @property # auto implements descriptor protocol 
    def x(self):
        return self.__x

    @property # auto implements descriptor protocol 
    def y(self):
        return self.__y

    def __iter__(self):
        return (i for i in (self.x, self.y))

    def __repr__(self): # debug print
        class_name = type(self).__name__
        return '{}({!r}, {!r})'.format(class_name, *self)

    def __str__(self): # pretty print
        return str(tuple(self)) # using tuple() requires __iter__ to apply 

    def __bytes__(self): #returns bytes
        return (
            bytes([ord(self.typecode)]) +
            bytes(array(self.typecode, self))
        )

    def __eq__(self, other):
        return tuple(self) == tuple(other)

    def __hash__(self):
        return hash((self.x, self.y))

    def __abs__(self):
        return math.hypot(self.x, self.y)

    def __bool__(self):
        return bool(abs(self))

    def angle(self):
        return math.atan2(self.y, self.x)

    def __format__(self, fmt_spec=''):
        if fmt_spec.endswith('p'):
            fmt_spec = fmt_spec[:-1]
            coords = (abs(self), self.angle())
            outer_fmt = '<{}, {}>'
        else:
            coords = self
            outer_fmt = '({}, {})'

        components = (format(c, fmt_spec) for c in coords)
        return outer_fmt.format(*components)

    @classmethod
    def frombytes(cls, octets):
        typecode = chr(octets[0])
        memv = memoryview(octets[1:]).cast(typecode)
        return cls(*memv)
```

**\__init__**
The basic constructor argument for self. All arguments passed to `__init__` are instances of the class (class instances). They exist effectively in name of the variable. `__init__` is fundamentally an initialization entity which does roughly 
```python
#example
obj = MyClass.__new__(MyClass)
MyClass.__init__(obj)
return obj
```

So 1.`type.__call__ `$\rightarrow$ makes the class callable, 2. `__new__` $\rightarrow$ creates the instance and 3. `__init__` $\rightarrow$ initializes the instance. Importantly, classes are callable because they inherit from `type` which has `__call__` defined already. Defining `__call__` is only necessary if you want custom behaviour. 

**\__iter__**
Makes an object iterable $\rightarrow$ including unpacking "\*". Vector2d uses a generator expression to yield components one after another. 

**\__repr__**
Returns the developer debug representation. In notebooks, `__repr__` returns when you call `repr()` or when `print()` has not `__str__` value. When formatting `__repr__`, i.e. `return '{}({!r},{!r}).format(class_name, *self)` $\rightarrow$ `!r` invokes `__repr__` method for debugging representation, which encloses a value with quotation marks (or related formatting depending on the type) so that you know its a string i.e. $\rightarrow$  `f"{'Adam'!r}" → 'Adam'`  (you can also use `repr` with `__repr__` to return the same thing). 

**\__str__**
Returns the pretty print of an object. Called on `print()`, or `str`. 

**\__bytes__**
Returns the binary information of for the class instance. First consider, `Vector2d` has class attribute, `typecode = 'd'` (the typecode is prefixed). Then,  `ord` returns the  unicode code point for a one-character string and`bytes()` converts this output to binary. Next, `bytes(array(self.typecode, self))` takes the typecode as its first argument and takes the instance itself as the next argument. The result is then converted to binary to get a full binary representation of your object. Note, the object in question is no longer `self`, which itself was a `Vector2d` instance, but instead represents `array.array` object instead now. 

The `array('d', ...)` is necessary because it knows how to encode Python floats as C doubles. Consider, `bytes()`, on a custom class looks for `__bytes__` first and if not found, searches the arguments below where you can see `bytes()` requires integers from 0-255 to be convertible to binary, which is precisely why `__bytes__` is defined in the first place.

```python
#example
bytes(integer)          # that many zero bytes
bytes(iterable_of_ints) # each int must be 0–255
bytes(buffer_object)    # copy raw buffer data
```

**\__eq__**
With this implementation using `tuple`, `Vector2d(3.0, 4.0)` will evaluate `True` to `Vector2d(3.0, 4.0) == (3.0, 4.0)`. The `__eq__` constructor takes two arguments $\rightarrow$ `self` and `other`. 

**\__abs__**
Return `math.hypot(self.x, self.y)` (requires `math` import). Note, the implementation of `__abs__` is mathematical $\rightarrow$ the absolute value of a coordinate is equal to the distance to its origin.  

**\__bool__**
Note, `__bool__` uses `abs`  for cleanliness here. Technically, bool evaluates whether a value is zero (false) or non-zero (True). 

**\__match_args__**
Supports positional argument matching (restructuring: `match/case`) `__match_args__ = ('x', 'y')` (all thats required). See the example below. 
```python
#example
class Point:
    __match_args__ = ('x', 'y')
    
match p:
    case Point(1, 2):
        ...
        
p.x == 1
p.y == 2
```

**\__format__**
Supports `format()` and `obj.__format__(format_spec)`, but must return a string.You can add an optional support term under `format_spec` to return custom formatting. 

**\__hash__**
Return hash ID. You cannot make a class hashable. Just return hash for individual values or attributes. 

**\__slots__**
This is a special attribute (not method) designed for memory optimization. `__slots__` creates an efficient data storage device on class which effectively bypasses the class' `__dict__` (a class may not have `__dict__` and `__slots__` defined simultaneously $\rightarrow$ `AttributeError`- additionally you cannot add attributes to class with `__slots__` arbitrarily). While `__init__` primarily serves an initialization terminal, it indirectly creates a `__call__` object (inherited from `type`) which you can use to pass values stored in `__slots__`. Alternatively, you can just attribute notation to i.e. `class.x`. 

```python
#example
class Vector2d:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y
        
#example
v = Vector2d(3, 4)
v.x = 10      # works
v.y = 20      # works
v.z = 30      # AttributeError
```

**frombytes**
This class method (`@classmethod` $\rightarrow$ receives `cls`, not `self`) will return `Vector2d` instance if it was converted to `bytes` (basically undoes `bytes`). The function, `chr()`, returns a unicode string of one character with ordinal i; 0 <= i <= 0x10ffff such that `typecode = chr(octest[0])` converts first `byte` object  character to `str`.  
```python
#example
xx = Vector2d(5, 10)
xx_bytes = bytes(xx) # returns b'd\x00\x00\x00\x00\x00...'
xx.frombytes(xx_bytes) 
```
```python
>>> Vector2d(5.0, 10.0)
```

---
## 12: special methods for sequences ✪
> Extends multidimensional functionality to Vector2d under Vector while explaining the sequence protocol and how to implement a custom sequence. Some of the dunders are omitted from the example below.
> 
> All sequences are iterables $\neq$ but not all iterables are sequences. A sequence has a defined length $\rightarrow$ `len(sequence)` $\rightarrow$ and is an ordered collection that supports indexing $\rightarrow$ `sequence[:5]`.
> 
> The sequence protocol is composed of two special methods $\rightarrow$ `__len__`, `__getitem__` provide basic sequence support. A sequence is an object with `__len__`, `__getitem__`. 

```python
#example
from array import array
import reprlib
import math
import functools
import operator
import itertools


class Vector:
    typecode = 'd' #keep typecode

    def __init__(self, components):
        self._components = array(self.typecode, components) #privatized
        #components is

    def __iter__(self):
        return iter(self._components)

    def __repr__(self):
        components = reprlib.repr(self._components)
        components = components[components.find('['):-1]
        return f'Vector({components})'

    def __str__(self):
        return str(tuple(self))

    def __bytes__(self):
        return (bytes([ord(self.typecode)]) +
                bytes(self._components))

    def __eq__(self, other):
        return (len(self) == len(other) and
                all(a == b for a, b in zip(self, other)))

    def __hash__(self):
        hashes = (hash(x) for x in self)
        return functools.reduce(operator.xor, hashes, 0)

    def __abs__(self):
        return math.hypot(*self)

    def __bool__(self):
        return bool(abs(self))

    def __len__(self):
        return len(self._components)

    def __getitem__(self, key):
        if isinstance(key, slice):
            cls = type(self)
            return cls(self._components[key])
        index = operator.index(key)
        return self._components[index]

    __match_args__ = ('x', 'y', 'z', 't')

    def __getattr__(self, name):
        cls = type(self)
        try:
            pos = cls.__match_args__.index(name)
        except ValueError:
            pos = -1
        if 0 <= pos < len(self._components):
            return self._components[pos]
        msg = f'{cls.__name__!r} object has no attribute {name!r}'
        raise AttributeError(msg)

    def angle(self, n):
        r = math.hypot(*self[n:])
        a = math.atan2(r, self[n-1])
        if (n == len(self) - 1) and (self[-1] < 0):
            return math.pi * 2 - a
        else:
            return a

    def angles(self):
        return (self.angle(n) for n in range(1, len(self)))

    def __format__(self, fmt_spec=''):
        if fmt_spec.endswith('h'):  # hyperspherical coordinates
            fmt_spec = fmt_spec[:-1]
            coords = itertools.chain([abs(self)],
                                     self.angles())
            outer_fmt = '<{}>'
        else:
            coords = self
            outer_fmt = '({})'
        components = (format(c, fmt_spec) for c in coords)
        return outer_fmt.format(', '.join(components))

    @classmethod
    def frombytes(cls, octets):
        typecode = chr(octets[0])
        memv = memoryview(octets[1:]).cast(typecode)
        return cls(memv)
```

**\__init__**
Components (`self._components`) is privatized. Vector is designed to take a sequence object as opposed to values directly to the constructor i.e. denoted as `*args` $\rightarrow$ instead i.e. `Vector([1, 2, 3]).` 

**\__iter__**
Performs `iter()`, which allows sequential access to each object in `_components`. 

**\__repr__**
The repr here is just means of representing the sequence object $\rightarrow$ `find()` returns the lowest index at which the `'['` first occurs $\rightarrow$`components[...]` encases a slice `[:]` resulting in a sliceable return. The final product is something like $\rightarrow$ if matrix is an array in `range(1, 101)` then `components = reprlib.repr(matrix)` yields `array('d', [1.0, 2.0, 3.0, 4.0, 5.0, ...])`; the values from which starting at `'['` are copied till `-1` (which is `')'` with `matrix[matrix.find('['):-1]` ). `reprlib` is used for representing values that are too long by inserting an ellipsis after a certain amount of values (this is important for `__repr__` methods to not overflow the debug window or console). 

**\__bytes__**
Now, `bytes()` operates directly on `self._components`. 

**\__eq__**
The length of the sequences being compared must be equal $\rightarrow$ `return (len(self) == len(other)` and each item in each sequence must be equal $\rightarrow$ `all(a == b for a, b in zip(self, other)`. 

**\__getitem__**
Along with `__len__`, implements the basic sequence protocol. `key` $\rightarrow$ returns one component. `slice` → returns a new `Vector` while `operator.index(key)` validates non-slice indices.

**\__getattr__**
Fallback for failed attribute lookup.Vector maps x, y, z, t $\rightarrow$ components 0, 1, 2, 3 using `__match_args__`. Raises AttributeError if the requested shortcut is unavailable.

**\__setattr__**
Intercepts attribute assignment. Vector uses it to prevent assignment to `x, y, z, t `and other lowercase single-letter attributes.

**hashability** 
Hashability requires a `__hash__` method and equal behaviour so that if `a == b → hash(a) == hash(b)` is true. There are different ways to hash. A memory efficient example $\rightarrow$  instead of using `hash(tuple())` you can call `hash()` for an item in a sequence i.e. `hash(a) for a in sequence` and compute an aggregate value i.e. an aggregate hash using `operator.xor` $\Rightarrow$ `operator.xor(a, b, /)` is a bitwise operator yielding the equality below. Additionally, the docstring for `functools.reduce` is given below. 
```python
#example
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0
```

```
#docstring functools.reduce
"reduce(function, iterable[, initial]) -> value
Apply a function of two arguments cumulatively to the items of a sequence
or iterable, from left to right, so as to reduce the iterable to a single
value.  For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates
((((1+2)+3)+4)+5).  If initial is present, it is placed before the items
of the iterable in the calculation, and serves as a default when the
iterable is empty."
```

---
## 13: interfaces, protocols and ABCs ✪
> There are two different types of protocols in python: 
> 
> dynamic protocol: outlined in the python interpreter and implemented via dunder special methods. they are dynamic because they work via the duck typing principle that states that a method is defined by its behaviour
> 
> formal protocol:  outlined by the collections.abc module, which formalize protocol definitions i.e. collections.abc.Sequence (usually distinguished by capitalization i.e. `Sequence` vs. `sequence`.) by requiring objects to posses certain methods to be of that class
> 
> summarized: a dynamic protocol defines an object by its behaviour, while a formal protocol defines an object by its formal definition 
> 
> What is an abstract base class ( `collections.abc.`)?  Abstract base class module is a collection of classes that formalize object interfaces. 

**abc**
abc is the general framework for creating abstract base classes (includes things like abstractmethod, abstractproperty, etc.)

**collections.abc**
Used to establish a formal interface for an object that must define certain set of methods i.e. `abc.Container` requires `__contains__` method (includes Iterable, Sequence, Mapping, MutableMapping, etc.). The collections.abc module is a specific set of ABCs for collection interfaces built using the abc framework from above.

**abc.ABCMeta // abc.ABC**
The ABCMeta metaclass (`type`) is the fundamental construct required for a class to be recognized as a metaclass (metaclases are explained later) $\rightarrow$ specify `class class_name(metaclass=ABCMeta):...` after importing $\rightarrow$ `from abc import ABCMeta`. You can also use `abc.ABC` which is a convenient way to get `ABCMeta` through inheritance $\rightarrow$ `class class_name(ABC):` $\rightarrow$ `ABCMeta` provides support for the following machinery: 1) track abstract methods on the class 2) prevent instantiation until those methods are implemented 3) make the class participate properly in `isinstance()` / `issubclass()` checks 4) give access to `ABC` features like virtual subclass registration via `.register()`. 

**abc.abstractmethod // @abstractmethod**
This decorator (`@abstractmethod)`) is applied to specific methods in a superclass which you then must define in a subclass. This allows you to enforce a formal protocol $\rightarrow$ all subclasses of the superclass where this decorator exists on specific methods, must be specified in the subclass. Failure to specify decorated methods from the superclass in a subclass class cause runtime enforceable errors. Additionally, the superclass must also be an abstract base class $\rightarrow$ `class(abc.ABC):...`. 

**type checking**
You can use `isinstance()`in conjunction with `collections.abc`, which provides useful types beyond the built-in classes (`int`, `str`, etc.) which work with `isinstance`, i.e. `collections.abc.MutableMapping` which can test an object against any mutable mapping (which could include `dict`, `OrderedDict`, etc). 

Inheriting from a `collections.abc` does not instantly inherit all special methods. For example, with `collections.abc.MutableMapping` requires you define `'__delitem__', '__getitem__', '__iter__', '__len__', '__setitem__'` to instantiate the class or you will cause error $\rightarrow$ this is effectively a form a forced inheritance.  

**abc.register**
This function is used for virtual subclassing. ABCs can register unrelated classes as virtual subclasses using `SomeABC.register(SomeClass)`. The registered class will then return `True` for `isinstance()` without inheritance

```python
#example
from abc import ABC

class MyABC(ABC):
    pass

MyABC.register(tuple)

assert issubclass(tuple, MyABC)
assert isinstance((), MyABC)
```
```python
>>> True
>>> True
```

**@classmethod**
This built-in decorates a method so it receives the class as its first arguments. Any method on class defaults to `self` $\rightarrow$ it operates on the class instance, not the class. If you wish to operate on the class itself, you must decorate using `@classmethod`, which receives class as its first argument usually denotes as `cls`. 
```python
#example
class class_name:
	@classmethod
	def class_operation(cls, arg1, arg2):...
```

**the numeric tower**
The numeric tower is a concept in the numeric abstract base class `numbers`, which contains a set of abstract base classes for mathematical number types. These numbers are used for runtime classification with `isinstance()` and `issubclass()`.  Use of `numbers` is discouraged for static type checking due to the hierarchical system below (and other irrational behaviour). 

```
#pseudocode example
numbers.Number
└── numbers.Complex
    └── numbers.Real
        └── numbers.Rational
            └── numbers.Integral
```

**static type - numeric types**
Because the above abstract classes are discouraged for static type checking, use the protocol types from `typing` $\rightarrow$ `SupportsInt`, `SupportsFloat`, `SupportsComplex`, `SupportsIndex`, `SupportsRound`, `SupportsAbs` $\Rightarrow$ static numeric protocols types are preferred to `numbers` abstract classes as they test for convertibility and not exact type adherence (particularly because type adherence in numbers is hierarchical). 

**\__subclasshook__**
A special class method used by an abstract base class (`ABC`) to customize the behaviour of `issubclass()`. It allows an ABC such as `YourABC` to decide whether another class such as `SomeClass` should be treated as a virtual subclass, even if `SomeClass` does not explicitly inherit from or register with `YourABC`. In other words, `__subclasshook__` lets an ABC define structural rules for `issubclass()`. For example, it can return `True` when a class provides a required method:

```python
#example
@classmethod
def __subclasshook__(cls, C):
    return any("do_something" in B.__dict__ for B in C.__mro__)
```

You could write equivalent logic in a normal helper function but that function would not integrate with Python's `issubclass()` or `isinstance()` machinery. `__subclasshook__` is what allows that custom structural check to participate directly in ABC subclass checking.

```python
#example
def is_virtual_subclass(C):
    return any("do_something" in B.__dict__ for B in C.__mro__)
```

**typing.Protocol**
Is a static type checking device inherited by a class to enable static structural subtyping in Python $\rightarrow$ Allows you to define an interface by behaviour instead of by inheritance (use when you don't care what the object's actual type is as long as it has these methods or properties).

By defining a base class that inherits `Protocol`, any class with a compatible type signature may register as a supported type (compatibility is determined by the variance of the field i.e. returns $\rightarrow$ contravariant, inputs $\rightarrow$ variant, assignments $\rightarrow$ invariant - chapter 15). 

In the example below, `Rando()` is not type `SupportsClose`, but nevertheless accepted by mypy as it holds the correct method and compatible type signature. 

If you wish to extend a Protocol by subclassing another Protocol use $\rightarrow$  `class class_name(superclass_name, Protocol)`. 

```python
#example
from typing import Protocol
class SupportsClose(Protocol):
    def close(self) -> str : 
        return 'CLOSE'

class Rando: #Rando is not type SupportsClose
    def close(self) -> str: 
        return 'OPEN'

def close_item(a : SupportsClose):
    return 'PASS'
    
close_item(Rando()) #Rando is accepted
```
```python
>>> 'PASS'
```


**@runtime_checkable**
Normally, `Protocol` is for static type checkers only but by applying `@runtime_checkable` $\rightarrow$ makes a `Protocol` compatible with `isinstance()` or `issubclass()` at runtime (static duck typing). The decorator, is primarily for use with `Protocol` usage with `isinstance`. 
```python
#example
from typing import Protocol, runtime_checkable

@runtime_checkable
class SupportsClose(Protocol):
    def close(self) -> None: ...

isinstance(obj, SupportsClose)
```

---
## 14: inheritance 
**inheritance**
An object, (daughter) which inherits the capabilities from another object (mother). A class from which you inherit must be passed to the class constructor call. Inheriting from a class imports all of the methods from that class to your new class. 

```python
#example
class Daughter(Mother):...
```

**overriding functions**
You can override functions from mother class (for more customization) in the daughter class by redefining any of the methods in the mother class in the daughter (using the same method name (`__name__`). 

**super()**
Used to call the implementation of a function up the `__mro__`. It respects Python's method resolution order and is especially important with multiple inheritance. `super()` passes any arguments to the method in the superclass. If no arguments are provided it simply recalls the original method. Note, `super()` does not simply call the parent function $\rightarrow$ it searches up the `__mro__` creating a threaded (see below for threaded search up the `__mro__` with `super()`). 
```python
#example
class Parent:
    def speak(self):
        print("Parent")

class Child(Parent):
    def speak(self): #override
        super().speak() #call inherited implementation
        print("Child")

kid = Child()
kid.speak()
```
```python
>>> Parent
>>> Child
```

**multiple inheritance**
Consider, `super()` works dynamically such that in a multiple-inheritance diagram, if a method from `BrokenClass` calls `super().method_name()` without specifying the superclass, the program will naturally return an `AttributeError` if there is a gap in the `__mro__`.

However, if `super().method_name()` were called from another class, `CombinedClass`, with multiple inheritance, one of which contains `method_name` $\rightarrow$ `CombinedClass(BrokenClass, CorrectClass)`then the method in `BrokenClass()` and the superclass will execute (this works because the `__mro__` is fully established from the subclass to the superclass). 

```python
#example
class BrokenClass:
    def do_nothing(self):
        print('NOTHING')

class CorrectClass():
    def do_something(self): #do_something exists
        print('SOMETHING')

class CombinedCass(BrokenClass, CorrectClass):
    def do_something(self):
        super().do_something() #retrieves do_something
        print('SOMETHING MORE') #augments do_something

x = CombinedCass()
x.do_something() #augmented function
```
```python
>>> SOMETHING
>>> SOMETHING MORE
```

The order of the inheritance chain in the class call matters $\Rightarrow$ `CombinedClass(BrokenClass, Correctclass)` $\neq$ `CombinedClass(Correctclass, BrokenClass)` $\Rightarrow$ the first class is given priority. 

```python
#example
class BrokenClass:
    def do_something(self):
        print('NOTHING')

class CorrectClass():
    def do_something(self): 
        print('SOMETHING')

class CombinedCass(BrokenClass, CorrectClass):
    def do_something(self):
        super().do_something()
        print('SOMETHING MORE')

x = CombinedCass()
x.do_something() #BrokenClass is called
```

A mixin class is a form of inheritance design that uses cooperative multiple inheritance to provide modular functionality or augmentation to a class that uses multiple inheritance between two classes with signature compatibility (complicated explanation). The key in understanding lies in the ability to create new classes with a mixed-functionality derived partially from one class (the mixin class) which augments the behaviour of the other class. 

When calling `super()`, it does not just call the parent method, but instead creates a virtual method-chain up the `__mro__`, such that in the example below, calling `MixinClass()` establishes a threaded relationship between `UpperStrMixin` and `Root` such that calling `super().__str__().upper` $\rightarrow$ refers to $\rightarrow$ `Root.__str__()`.

```python 
#example
class Root:
    def __str__(self):
        return "hello world"

class LeafA(Root):
    pass

class UpperStrMixin:
    def __str__(self):
        return super().__str__().upper()

class MixinClass(UpperStrMixin, LeafA):
    pass

x = MixinClass()

print(x)
print(MixinClass.__mro__)
```

```python
>>> HELLO WORLD
>>> (<class '__main__.MixinClass'>, <class '__main__.UpperStrMixin'>, <class '__main__.LeafA'>, <class '__main__.Root'>, <class 'object'>)
```

```
#pseudocode example
#__mro__ method thread

print(x)
   ↓
MixinClass.__str__
   ↓ MRO
UpperStrMixin.__str__
   ↓ super()
LeafA
   ↓
Root.__str__()
   ↓
"hello world"
   ↓ .upper()
"HELLO WORLD"
```

**\__mro__**
Method resolution order is the order in which class inheritance is compiled. The `__mro__` is always establish with the name of the given class on which it is called first, and then proceeds to describe the order in which classes are inherited in a multiple inheritance structure. Call it on the class of an object, not an instance.
```python
#example
CombinedClass.__mro__
```
```python
>>> (__main__.CombinedClass, __main__.BrokenClass, __main__.CorrectClass, object)
```

**what can be inherited?**
Technically, every object in Python inherits from `object()` although its not specified. You can inherit from built-ins like `list`, `dict`, or `tuple` but it is not advised because overriding certain methods will fail. Instead use `collections` objects that are designed for object extension `UserList`, `UserDict`, `UserString`, etc. 

**concrete class vs. abstract class**
The difference here is somewhat cosmetic as both are technically classes however, the distinction lies in that an abstract class has an enforceable interface via `@abstractmethod` that raises `TypeError` if all the `abstractmethods` are not specified in the subclass whereas a concrete class can be instantiated directly.

---
## 15: more on type hints ✪
> The type systems in python is a gradual typing system designed to satisfy the demands of structural and nominal type systems. Typing is primarily made to enforce data structures so that when code scales, the inputed types are correctly flagged. 

**gradual typing**
A typing system in python, which uses both structural and nominal typing interface. 

**static types**
A static type (i.e.`Iterable[float]`) is distinguished from a concrete type (i.e.`float`) by the fact that concrete types refer to actual classes and (runtime) types:
- `int` is both a class and a type
- `Iterable[float]` is a type but not a real class
The example below makes two points. A type cannot be instantiated and cannot be subclasses. A type can be any static typing object (`int`, `Complex`, `Iterable[float`, `Any`, etc.). 

In this chapter, types are considered static types by default. Types are embedded in python's syntax and encoded in abstract base classes as useable objects. 
```python
#example
class UnionClass(Union[str, float]):... #raises TypeError
...
u = UnionClass #raises TypeError
```

**subtype relationships**
The typing interface is based on subtype relationships that are different from the native subclasses in python and centered on type compatibility and convertibility. 

Normally, `int` has no relationship to `float`, however static type checkers treat `int` as a subclass of `float` because an `int` is convertible to `float` $\rightarrow$ anywhere where you can take a `float` you can also take an `int`. This forms the effective type hierarchy (although this hierarchy is not explicitly listed in any docs as it is implicitly enforced by static type checkers). 

Similar to `object`, the fundamental type is `Any` $\rightarrow$ all types (`float`, `int`, `Complex`, etc.) accept `Any`, but unlike `object`, `Any` superclasses and subclasses every type. That is, `Any` is the subclass and superclass of every type. 

**generic type**
A generic type is a parameterized static type $\rightarrow$ a type annotated by another type i.e. `list[float]`, `dict[str, int]`, `Union[T, S]`, etc. Generally, types are parameterized using square brackets `[]` but may take different inputs. Below are the fundamental building blocks for generic types. 

| type       | signature                    | example                      | explation                                                                   |
| ---------- | ---------------------------- | ---------------------------- | --------------------------------------------------------------------------- |
| `Any`      | `...`                        | `Any`                        | compatible with every type                                                  |
| `Union`    | `Union[T1, T2, ...]`         | `int \| str`                 | types that are subtype of at least one of T1, T2, ..., are subtypes of this |
| `Optional` | `Optional[T]`                | `Optional[int]`              | alias for `Union[T1, None]`                                                 |
| `tuple`    | `tuple[T1, T2, ...]`         | `tuple[int, str]`            | each field corresponds to positional entry                                  |
| `dict`     | `dict[T1,T2]`                | `dict[str,int]`              | T1, T2 correspond to key, value type pair                                   |
| `list`     | `list[T1]`                   | `list[float]`                | list can only take one type `T1`                                            |
| `Callable` | `Callable[[T1, T2, ...], R]` | `Callable[[int, str], bool]` | T1, T2, ..., are input types, R is return type                              |
| `TypeVar`  | `TypeVar(name, ...)`         | `T = TypeVar("T")`           | type variable $\rightarrow$ must be declared twice                          |
| `Generic`  | `Generic[T1, T2, ...]`       | `class Box(Generic[T]): ...` | class generic $\rightarrow$ create custom generic clasw                     |

**generic class**
You can create a generic class by inheriting typing.Generic ($\leq$ 3.11) or use a type variable (3.11+) in the class constructor $\rightarrow$ `class class_name(Generic[T])` $\rightarrow$ `class class_name(T)`. A generic class is a runtime object (class) invoked or typed on class instances

```python
#example
class Square[T: (int, float)]:
    def __init__(self, length: T, width: T) -> None:
        self.length : T = length
        self.width : T = width

    def perimeter(self) -> T:
        return 2*(self.length + self.width)

box : Square[int] = Square(4, 4) #typed class instance 
print(box.perimeter())

```

**type variable / typing.TypeVar**
Represents an unknown type whose identity or relationship must be preserved across multiple positions in a type expression. The general way to construct a type variable. In the example below, `Generic[T]` is called in the class constructor and declared with one or more type variables (`Box(Generic[T])`$\leq$ 3.11).). The type variable is the variable that appears in a generics description $\rightarrow$ `[T]`.

Its important to note, TypeVar is not a direct comparator $\rightarrow$ it does not work by checking if two fields are equal, but instead defaults to closest shared inheritor of the given 

```python
#example
from typing import TypeVar, Generic, reveal_type

T = TypeVar('T')

class Box(Generic[T]):
    def __init__(self, content: T):
        self.content = content

    def get(self) -> T:
        return self.content


box_int = Box[int](5)

reveal_type(box_int.get()) #returns builtins.int
box_int.get()
```
```python
>>> Runtime type is 'int'
>>> 5
```

**variance**
Variance defines how generic types support safe substitution when their type parameters are used in different fields (calls, returns, assignments etc). This matters because generic classes have an implicit type hierarchy and variance determines whether `Generic[Dog]` is related to `Generic[Animal]` if `Dog <: Animal`. 

For any class hierarchy involving types like $A$, and a subtype $C$ such that $C < A$, and a generic class $B[T]$, the relationship between $B[C]$ and $B[A]$ depends on the declared variance of $B$:

1. If B is covariant, and $C < A$, then $B[C] < B[A]$. This is safe because the generic type only produces values i.e., a function that returns $C$ can safely stand in for one that returns $A$.

2. If $B$ is contravariant, and $C < A$, then $B[A] < B[C]$. This is safe because the generic type only consumes values i.e., a function that accepts $A$ can safely stand in for one that accepts $C$.

3. If B is invariant, then $B[C]$ and $B[A]$ are completely unrelated types, even if $C < A$. Substitution is not allowed in either direction. This is necessary when the type is both consumed and produced, since allowing substitution in either direction could break type safety.

Python's built-in generic types like `list`, `dict`, and default set to invariant $\rightarrow$ `list[int]` is not compatible with `list[float]`, even though int and float are both subtypes of `numbers.Number`. The type checker enforces exact type matches for safe behaviour.

The above is important as it determines how much flexibility the static type checker gives to a certain field. In python, function parameter types are contravariant, while return types are covariant when comparing callable types (contra $\approx$ comes in, co $\approx$ comes out). Alternatively, covariant means to vary in accordance (covary) to the given type hierarchy, whereas contravariant means to vary against (contra) the given type hierarchy. 
```python
#example
def f(x: Animal) -> Dog: #Dog is covariant, Animal is contravariant
    ...
```

---
> The rest of this chapter are just some useful typing tools and some other concepts. 

**@overload**
The overload function decorator can be used to instantiate a function/method multiple times to with different type signatures to prevent without using unions or other mechanisms. 
```python
#example
from typing import overload

class bytes:
    ...
    @overload
    def __getitem__(self, i: int) -> int: ...
    @overload
    def __getitem__(self, s: slice) -> bytes: ...

#alternative derivation
#def __getitem__(self, a: int | slice) -> int | bytes: ...
```

**typing.TypedDict()**
Used to type check dictionaries or record types (not good for json or xlm where the loaded vales can take `Any`). Running mypy (terminal $\rightarrow$ `mypy file_name.py`) will flag wrongly typed values. Cannot be enforced with `@runtimecheckable`
```python
#example
class Movie(TypedDict):
	name: str 
	year: int # values must be int
	
br : Movie = {'name': 'Blade Runner',
              'year': '1982'} #will be flagged
```


**runtime issues with \__annotations__**
When calling `class_name.__annotations__` it yields inconsistent results depending on the context $\rightarrow$ 1. calling`__annotations__` normally, will yield the real type objects `{'x': int}` $\rightarrow$ 2. calling`__annotations__` with `from __future__ import annotations` this will yield a string `{'x': 'int'} — string` $\rightarrow$ 3. forward references are strings always i.e. `{'x': 'MyClass'}` (forward references are classes typed with a type whose logic is preceded by its formal definition i.e. `demo_class: -> demo_class`).

---
## 16: operator overloading
> Is about extending operator functionality (+, -, /, ...) to custom classes. Basic terminology $\rightarrow$ 'operator' $\rightarrow$ mathematical terms $(+,-,\div,\text{etc.})$, 'operand' $\rightarrow$ the values on which the sign/operator acts, 'unary operator' $\rightarrow$ an operator which acts on a single operand i.e. $+4$, 'infix operator' $\rightarrow$ an operator which acts between two operands i.e. $4 + 5$, $20 - 10$. 
> 
> Many of the special methods in this chapter (and previous for that matter) are directly inherited from object. See [[#object]] in the appendix. Defining these special methods overwrites their behaviour. 

**basic arithmetic operators**
Do no redefine any of the below as it just leads to confusion i.e. `__add__` should add, and `__sub__` should subtract. 

| Dunder Method  | Operator   | Usage/Description      | Example                                                          |
| -------------- | ---------- | ---------------------- | ---------------------------------------------------------------- |
| `__add__`      | `+`        | Addition               | `def __add__(self, other): return self.x + other.x`              |
| `__sub__`      | `-`        | Subtraction            | `def __sub__(self, other): return self.x - other.x`              |
| `__mul__`      | `*`        | Multiplication         | `def __mul__(self, other): return self.x * other.x`              |
| `__truediv__`  | `/`        | True division          | `def __truediv__(self, other): return self.x / other.x`          |
| `__floordiv__` | `//`       | Floor division         | `def __floordiv__(self, other): return self.x // other.x`        |
| `__mod__`      | `%`        | Modulo                 | `def __mod__(self, other): return self.x % other.x`              |
| `__pow__`      | `**`       | Exponentiation         | `def __pow__(self, other): return self.x ** other.x`             |
| `__divmod__`   | `divmod()` | Quotient and remainder | `def __divmod__(self, other): return divmod(self.x, other.x)`    |
| `__matmul__`   | `@`        | Matrix multiplication  | `def __matmul__(self, other): return self.matrix @ other.matrix` |
| `__and__`      | `&`        | Bitwise AND            | `def __and__(self, other): return self.x & other.x`              |
| `__or__`       | `\|`       | Bitwise OR             | `def __or__(self, other): return self.x \| other.x`              |
| `__xor__`      | `^`        | Bitwise XOR            | `def __xor__(self, other): return self.x ^ other.x`              |

**reverse method**
For any given statement, class(value) + value, we may have the reverse, value + class(value) i.e. $\rightarrow$ `Vector([1, 2]) + (1, 2)` $\iff$ `(1 + 2) + Vector([1, 2])`. 

In such a scenario the basic special methods are insufficient because adding class(value) + value is different than value + class(value) $\rightarrow$ should the output be type(class) or tuple? To support these reflections/reversals, Python has a set of reverse operators that have rs in front of them i.e. `__add__` becomes $\rightarrow$ `__radd__`. 

| Dunder Method   | Operator   | Usage/Description                | Example                                                      |
| --------------- | ---------- | -------------------------------- | ------------------------------------------------------------ |
| `__radd__`      | `+`        | Reflected addition               | `def __radd__(self, other): return other + self.x`           |
| `__rsub__`      | `-`        | Reflected subtraction            | `def __rsub__(self, other): return other - self.x`           |
| `__rmul__`      | `*`        | Reflected multiplication         | `def __rmul__(self, other): return other * self.x`           |
| `__rmatmul__`   | `@`        | Reflected matrix multiplication  | `def __rmatmul__(self, other): return other @ self.matrix`   |
| `__rtruediv__`  | `/`        | Reflected true division          | `def __rtruediv__(self, other): return other / self.x`       |
| `__rfloordiv__` | `//`       | Reflected floor division         | `def __rfloordiv__(self, other): return other // self.x`     |
| `__rmod__`      | `%`        | Reflected modulo                 | `def __rmod__(self, other): return other % self.x`           |
| `__rdivmod__`   | `divmod()` | Reflected quotient and remainder | `def __rdivmod__(self, other): return divmod(other, self.x)` |
| `__rpow__`      | `**`       | Reflected exponentiation         | `def __rpow__(self, other): return other ** self.x`          |
| `__rlshift__`   | `<<`       | Reflected left shift             | `def __rlshift__(self, other): return other << self.x`       |
| `__rrshift__`   | `>>`       | Reflected right shift            | `def __rrshift__(self, other): return other >> self.x`       |
| `__rand__`      | `&`        | Reflected bitwise AND            | `def __rand__(self, other): return other & self.x`           |
| `__rxor__`      | `^`        | Reflected bitwise XOR            | `def __rxor__(self, other): return other ^ self.x`           |
| `__ror__`       | `\|`       | Reflected bitwise OR             | `def __ror__(self, other): return other \| self.x`           |

**riche comparisons**
Is what python calls the ability to customize the behaviour of the equality operators below. 

| Method   | Operator | Meaning                  | Example                                             |
| -------- | -------- | ------------------------ | --------------------------------------------------- |
| `__eq__` | `==`     | Equal to                 | `def __eq__(self, other): return self.x == other.x` |
| `__ne__` | `!=`     | Not equal to             | `def __ne__(self, other): return self.x != other.x` |
| `__lt__` | `<`      | Less than                | `def __lt__(self, other): return self.x < other.x`  |
| `__le__` | `<=`     | Less than or equal to    | `def __le__(self, other): return self.x <= other.x` |
| `__gt__` | `>`      | Greater than             | `def __gt__(self, other): return self.x > other.x`  |
| `__ge__` | `>=`     | Greater than or equal to | `def __ge__(self, other): return self.x >= other.x` |
**NotImplemented**
The NotImplemented special value is for when a class does not know how to perform an operation with another operand type $\rightarrow$ this tells python to reverse its operations if `a` is a class instance$\rightarrow$ `a.__eq__(b)` $\rightarrow$ `NotImplemented` $\rightarrow$ `b.__eq__(a)` reverses the operation. 
```python
#example
def __add__(self, other):
    if isinstance(other, DemoClass):
        return DemoClass(self.x + other.x)

    return NotImplemented
```

**in-place operators (augmented assignment)**
Have their own pre-suffix similar to the reverse operators. Unlike the traditional operators (reversed or not), the in-place operators need to return the same object, which means they must return self. 

The in-place operator is syntactic sugar for an binary operator with an assignment. Also, the in-place operator reduces ambiguity: `a += b` is `a = a + b` $\iff$ `a + b` if `a` and `b` are different types... is the final result class `a` or class `b`?

All in-place special methods should NOT extend to an immutable classes (obvious but worth mentioning). 

| Method          | Operator | Meaning                 | Example                                                           |
| --------------- | -------- | ----------------------- | ----------------------------------------------------------------- |
| `__iadd__`      | `+=`     | In-place addition       | `def __iadd__(self, other): self.x += other.x; return self`       |
| `__isub__`      | `-=`     | In-place subtraction    | `def __isub__(self, other): self.x -= other.x; return self`       |
| `__imul__`      | `*=`     | In-place multiplication | `def __imul__(self, other): self.x *= other.x; return self`       |
| `__itruediv__`  | `/=`     | In-place true division  | `def __itruediv__(self, other): self.x /= other.x; return self`   |
| `__ifloordiv__` | `//=`    | In-place floor division | `def __ifloordiv__(self, other): self.x //= other.x; return self` |
| `__imod__`      | `%=`     | In-place modulo         | `def __imod__(self, other): self.x %= other.x; return self`       |
| `__ipow__`      | `**=`    | In-place exponentiation | `def __ipow__(self, other): self.x **= other.x; return self`      |
| `__iand__`      | `&=`     | In-place bitwise AND    | `def __iand__(self, other): self.x &= other.x; return self`       |
| `__ior__`       | `\|=`    | In-place bitwise OR     | `def __ior__(self, other): self.x \|= other.x; return self`       |
| `__ixor__`      | `^=`     | In-place bitwise XOR    | `def __ixor__(self, other): self.x ^= other.x; return self`       |
| `__irshift__`   | `>>=`    | In-place right shift    | `def __irshift__(self, other): self.x >>= other.x; return self`   |
| `__ilshift__`   | `<<=`    | In-place left shift     | `def __ilshift__(self, other): self.x <<= other.x; return self`   |

---
# IV: Control Flow
---
## 17: iterators, generators, classic coroutines ✪

**iterator**
Produces items one at a time. Must implement `__iter__` and  `__next()__`. Tracks its position in an index an yields values one after another $\rightarrow$ is stateful $\approx$ tracks its state.   

Use if you want to iterate over a custom class, generate values lazily (instead of storing them), control the order of traversal, pass large spaces of values, traverse things like trees, graphs, databases, results, files etc. 

An iterator is the direct machinery on which iteration functions $\rightarrow$ `y = iter(x)` returns an iterator $\rightarrow$ `next(y)` is how each value is yielded. 

**iterable**
Collection of objects which you can loop which return an iterator: list, str, tuple, dict. An iterable returns. May implement either `__iter__` or `__getitem__`. 

**iterator vs. iterable**
The difference is small but important $\rightarrow$ an iterable returns an iterator $\rightarrow$ an iterator is the stateful iteration mechanism. 

| term     | requirements                                        |
| -------- | --------------------------------------------------- |
| iterable | may implement either: `__iter__()` OR `__getitem__` |
| iterator | must implement both: `__iter__` AND `__next__`      |

The primary reason for an iterator $\rightarrow$  allows independent iteration of the same structure. 

```python
#example
x = [10, 20, 30] # list is iterable

a = iter(x) # return independent iterator to a
b = iter(x) # return independent iterator to b

print(next(a))
print(next(a))
print(next(b))
```
```python
>>> 10
>>> 20
>>> 10
```

An iterable returns an independent iterator using `__iter__` with its own independent `__next__` method $\rightarrow$ an iterator has the `__next__` method present on the actual. 

The need for an iterator is apparent $\Leftrightarrow$  without one, an object needs a global index across all its instances. However, given the that an iterable returns an iterator (`iter()` - see below), the primary reason for an iterator is when you need special state control of index (i.e. counting downwards, counting by special intervals, etc.). You often don’t need a custom iterator class at all. 

In the example below, the iterable inherits default `__next__` behaviour (not customizable), while the iterator class has custom `__next__` behaviour. It is important to note, the iterable class does `iter(self.items)`, while the iterator class does `return self` $\Rightarrow$ the reason for this is that in a custom iterator we return the iterator itself (`self`), which then calls `next(self)` recursively until we exhaust the index. Additionally, an iterable may use `__getitem__` to iterate values if not `__iter__` exists. 

```python
# example
class MakeIterable:
	def __init__(self, items):
		self.items = items
		
	def __iter__(self):
		return iter(self.items) # return iter(self)
		
class MakeIterator: 
	def __init__(self, items):
		self.items = items
		self._index = 0
		
	def __iter__(self): 
		return self # return self
		
	def __next__(self): #custom behaviour
		if self._index >= len(self.items):
			raise StopIteration # stop iterating
	
		item = self.items[self._index] #retrieve item
		self._index += 2 # update index in twos
		return item
		
numbers = list(range(11)) # [0, 1, 2,..., 10]
a = MakeIterable(numbers) 
b = MakeIterator(numbers)
print([i for i in a]) # default __next__
print([i for i in b]) # every second value
```
```python
>>> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> [0, 2, 4, 6, 8, 10]
```

**iter()**
Built-in function that obtains an iterator from an object. 1. The object just has `__iter__()` $\Longrightarrow$ `iter(obj)` calls `obj.__iter__()` and returns the iterator it provides. 2. The object has `__iter__()` and `__next__` $\Longrightarrow$ returns `self` and the returned iterator implements `__next__()`. 3. The object just has `__getitem__()` $\Longrightarrow$ `iter()` creates a legacy sequence iterator that requests `obj[0], obj[1], obj[2]`, etc. until `IndexError`

**generator object**
The generator expression ([[#2 Array of Sequences ✪]]) returns a generator object. A generator object is a type of iterator $\rightarrow$ implements the iterator protocol. 

Fundamentally, it is a suspended, stateful computation that remembers its execution state between iterations . Each call to `next()` resumes execution, computes until the next value is produced, yields that value, and suspends again. The fundamental behaviour of generator object is a resumable computation with preserved execution state. The `yield` statement is syntactically integral to generator object $\rightarrow$ it is what suspends computation, receive, and produce values. 

 **classic coroutines**
A type of generator object that is primarily used to both produce and consume values. The term 'classic coroutine' comes from Fluent Python and used to categorically describe generator objects which receive, perform some processing, and return a new value. These generators use the following operators: `send()`, `throw()`, `close()`, `next()`. 

```python 
#example
def coroavg():
    total = 0.0
    count = 0

    while True:
        value = yield
        total += value
        count += 1
        print(total / count)


coro = coroavg()
next(coro)       # prime coroutine

coro.send(10) # 10.0
coro.send(20) # 15.0
coro.send(30) # 20.0
```

**generator function**
Any function that contains a `yield` becomes a generator function $\rightarrow$ pauses a function's execution $\rightarrow$ sends a value back to the caller $\rightarrow$ saves its internal state $\rightarrow$ sends a value back to the caller. 

**yield**
Covered above but is worth mentioning $\rightarrow$ `yield` is the statement from which all values are suspended, consumed, and produced. Again, the `yield` statement turns a function into a fun

**yield from**
Lets one generator temporarily hand over iteration/control to another iterable or generator (syntactic sugar). 
```python
#example
def gen(): # manual generator function
    for x in [1, 2, 3]:
        yield x

def gen(): # automated generator function
    yield from [1, 2, 3] 
```
---
## 18: with, match, else, scopes ✪
**context manager**
Automates the lifecycle of an object within an environment $\rightarrow$ the environment is any object with resources that require controlled access and release i.e. files. The context manager allows you to automatically trigger custom enter and exit processes. 

**with**
Statement invokes context manager protocol by returning a with block (just a line return). When you call the with statement you enter the with block (py3.10 now supports $\rightarrow$ `with (file1 as fp1, file2 as fp2,...)` for multiple files). The value following the 

```python
# example
with ContextManager as CM: 
	...
	
with open(file) as fp:  #open(file) is bound to fp
	data = fp.read() 
	...
```

**context manager protocol**
The with statement creates the with block (context), while `__enter__` and `__exit__`, are called upon entering and leaving the `with` block. 

**\__enter__, \__exit\__**
The special methods called upon entering and exiting the with block respectively. When you exit the `with` block, python calls `manager.__exit__(exc_type, exc_value, traceback)`. 

```python
#example
class ContextManagerClass:
	def __enter__(self):
		return self
		
	def __exit__(self, exc_type, exc_value, traceback)-> bool | None:
		 print("EXIT CONTEXT")

        if exc_type is not None:
            print("Exception:", exc_type.__name__)
            print("Message:", exc_value)
        
        return False # do not suppress exceptions
        
with ContextManager as CM:
	print('INSIDE CONTEXT')

```
```python
>>> ENTER CONTEXT
>>> INSIDE CONTEXT
>>> EXIT CONTEXT
```

The `__exit__` is not an ordinary method. The main job of `__exit__` is to release/clean up resources, inspect any error/exception occurred, and decide whether that it should be kept. 
 
 The return value of `__exit__` is bool (true/false), which determines whether an error (if any) should be suppressed or propagated. In this context, an error refers to any error 
coming from within the with block i.e. dividing $\rightarrow n \div 0$. When falsy, the error is suppressed, and when truthy, the error is propagated. Once any error inside the with block occurs, it is fed parameters below in the following manner:
```python
#example
ContextMangerClass.__exit__(
    ZeroDivisionError,
    ZeroDivisionError("division by zero"),
    <traceback object>
)
```

- `exc_type`: the class of the exception raised (e.g., `ZeroDivisionError`); `None` if no exception occurred.
-  `exc_value`: the actual exception instance that was raised; typically holds the message or arguments passed when raising the exception and can be inspected via .args.
- `traceback`: a traceback object representing the call stack at the point where the exception was raised (useful for advanced debugging or logging)
- the parameters allow the context manger to catch exceptions, log exceptions, or re-raise them. These params must be present in the `__exit__()` constructor for `__exit__` to actually work with the `with` statement although technically any param name values can be passed to `__exit__` i.e. `__exit__(self, one, two, three)`. the omission of any param causes the protocol to break and return `TypeError`

**contextlib.contextmanager/@contextmanager**
Reduces boilerplate code $\rightarrow$ having to construct a whole class i.e. `__enter__`, `__exit__` methods
(Fluent Python, lis.py). 

**files**
The `open()` function is very often used in conjunction with a context manager (`with`) $\rightarrow$ and returns a TextIOWrapper (from `io` - standard library) object which has options like `read` on it for retrieving string information from a file (type: `_io.TextIOWrapper` in text mode). When the `with` block ends, the file is closed automatically (even if an exception happens). 
```python
#example
path = '/Users/adamabgao/Desktop/py/FluentPython/random_text.txt'
with open(path, 'r', encoding='utf-8') as p:
    data = p.read()
    print(data)
```

**OR vs. '|'** 
The pipe operator, `|` was introduced for structural pattern matching (match/case) in PEP 634 but can be used to substitute `or` where `or` expression is expected. 
```python
#example
x = 'quote', 10.0
match x:
    case int(x) | float(x): #takes int or float
        print('FOUND')
    case 'def' | 'quote', float(x): #right side is tuple(str,float(x))
	    print('DOUBLE FOUND') #this was found

class TypeExample:
    def add(a, b) -> int | float: #takes int or float
        return sum([a, b])

ten = TypeExample.add(5, 5)
ten
```
```python
>>> DOUBLE FOUND
>>> 10
```

---
> the else block is covered in this chapter because it also a type of block

**else**
Else block is generally used with if/else statements however can also be used in other syntaxes. 1. You can use an else statement in a for-loop $\rightarrow$ the else statement executes when the for loop ends. 2. You can use an else statement in a while-loop $\rightarrow$ the else statement executes when the while-loop exists because the condition becomes falsy. 3. You can use the else statement in a try statement $\rightarrow$ the else will run only if no exception is raised in the try block $\rightarrow$ if the except block runs, the else statement will skip (be careful to use return statement appropriately considering that return causes a total block exit $\rightarrow$ meaning it will skip the else statement).

```python
#for-loop example
for i in range(1,3): #for loop
    print(i, end=',')
else:
    print('3')

#while-loop example
i = 0
while i < 2:
    i += 1
    print(i, end=',') #seperate with comma
else:
    print('3')

#try example
try:
    1/0
except ZeroDivisionError:
    print('1,2,',end=':(')
else:
    print('3')
```
```python
>>> 1,2,3
>>> 1,2,3
>>> 1,2,:(
```
---
> the rest of this chapter deals with closures and scopes beyond chapter 9

**nested scope**
A scope is said to be nested if it is within another environment. When python searches for variables it searches environments according to the LEGB rule. 


$$
\textbf{LEGB Scope Resolution}
$$

$$
\enclose{circle}{
  \qquad \text{Built-in} \qquad
  \enclose{circle}{
    \qquad \text{Global} \qquad
    \enclose{circle}{
      \qquad \text{Enclosing} \qquad
      \enclose{circle}{
        \qquad \text{Local} \qquad
      }
    }
  }
}
$$
$$
\begin{gathered}
&\boxed{\text{Local}}\\
&\downarrow\\
&\boxed{\text{Enclosing}}\\
&\downarrow\\
&\boxed{\text{Global}}\\
&\downarrow\\
&\boxed{\text{Built-in}}\\
\end{gathered}
$$

```python
#example
msg = 'Global Variable'

def func(): # global example
	print(msg)

func()

def outer(): # enclosing example
	msg = 'Enclosing'
	def inner():
		print(msg)
		
	inner()

func(), outer()
```
```python
>>> Global Variable
>>> Enclosing
```

**lexical scoping**
The lexical scope is the place of the scope where a variable is assigned (local, enclosing, etc.). The lexical scope is resolved according the LEGB. Python uses lexical scoping $\rightarrow$ variables are defined by where they are defined in the source code and not where or how the function is called (dynamic scoping). 

**\__closure__**
The enclosing scope is the function scope surrounding another function (an enclosing scope must be a function). An enclosure is a function (literal). 

`__closure__` is a special attribute that captures all the free variables referenced in the inner function $\rightarrow$ the free variables must be referenced in the inner function in order for them to be captured by `__closure__`. The enclosing variable cannot be renamed in the inner function or it will just become a local variable of the inner function). You can access more information using `__code__` special attribute listed in chapter 9. 
```python
#example
msg = 'Global Variable'

def outer():
    msg = 'Enclosing' #free variable
    m = 3.1415 #free variable
    def inner():
        print(msg, m) #both freevars are referenced
    return inner #return inner function

f = outer()
f.__closure__
```
```python
>>> (<cell at 0x1108a38b0: float object at 0x110db0890>,
 <cell at 0x1108a10c0: str object at 0x110e92d30>)
```

---
## 19: concurrency models
**concurrency**
Concurrency has to do with executing multiple actions at once. Can be parallel actions (things that occur in unison) or things that simply occur coincidentally. 

**execution unit**
Object that executes code concurrently. There are three different kids of processing units: processes, threads, coroutines

**performance profiling**
Before explaining the different type of execution units in concurrent programming, we explain performance profiling (performance analysis), which asks what kinds of work a program can do. 

Because the purpose of concurrency is to optimize resources for performance maximization, we must know what the general resource types are and how they are best utilized by different concurrency models.

1. **CPU-bound**: These operations use the processor’s arithmetic-logic units (ALUs), on-chip caches, and registers, typically executing in nanoseconds. During this time, your code is actively running on the CPU, performing tasks such as crunching numbers, parsing, and rendering.

2. **IO-bound**: These operations involve resources outside the CPU, such as SSDs, disks, main memory, networks, DSPs, cloud APIs, and database servers, typically taking microseconds to milliseconds. During this time, your code is mostly waiting, with the thread often sleeping until the device or remote service completes the operation.

**concurrency models**
**Processes**: Each Python interpreter instance typically runs in its own process, with independent memory, file descriptors, and operating system resources. Concurrency is managed by the OS scheduler, which schedules processes independently. This enables true parallelism across multiple CPU cores, as separate interpreter processes have their own GIL and memory address spaces. Because processes do not directly share ordinary Python state, communication requires OS-mediated mechanisms such as pipes, sockets, or shared memory. Consequently, inter-process communication can be relatively expensive due to serialization, data transfer, and synchronization overhead. 
Analogy: A process is like an independent workspace or office. 

**Threads**: A thread is an execution unit within a process, which initially starts with a single thread called the main thread. Additional threads can be created to execute tasks concurrently while sharing the process’s memory and live Python objects. In standard CPython builds, the Global Interpreter Lock (GIL) prevents multiple threads from executing Python bytecode simultaneously, although the OS scheduler can switch between threads and allow their execution to overlap during waiting periods. Consequently, threads generally cannot achieve true parallelism for CPU-bound Python code, but they can for certain operations that release the GIL. Threads communicate through shared memory and coordinate access to resources using synchronization primitives such as locks and semaphores to prevent race conditions. Threads have lower overhead than processes and are particularly effective for I/O-bound workloads, where execution can switch to other threads while one waits for an I/O operation to complete. 
Analogy: A thread is like an independent worker within that office who executes a task. A worker shares office resources with other workers in the office. 

**Coroutines**: A coroutine is a lightweight execution unit managed by an event loop, typically defined using `async def` and suspended or resumed using `await` (or `yield` in generator-based coroutines). Coroutines generally execute concurrently within a single thread using `asyncio`, without requiring additional threads or processes. Concurrency is achieved through cooperative multitasking, where coroutines voluntarily suspend execution at `await` points, allowing the event loop to schedule other tasks. Coroutines coordinate and communicate using `asyncio` primitives such as `asyncio.Queue`, `asyncio.Event`, and `asyncio.Lock`, while `asyncio.Task` schedules coroutine execution. Because each suspended coroutine retains only its execution state, including local variables and references to its suspended call frames, rather than requiring a dedicated OS thread stack, coroutines have significantly lower memory overhead than threads. This makes them particularly efficient for managing large numbers of concurrent I/O-bound operations, although they do not provide true CPU parallelism within a single event-loop thread.
Analogy: A task that a worker can temporarily set aside while waiting for something, allowing them to work on another task.

**python execution model**
In Python, calling a regular function defined with `def` immediately executes its body, and its return value is subsequently bound to the assignment target. For example, `x = some_function(10)` executes the function before assigning its returned value to `x`, even if that value is `None`. This illustrates a fundamental principle of Python’s execution model: the right-hand expression is evaluated before assignment occurs. Consequently, any side effects produced during function execution, such as printing, modifying objects, or changing external state, occur regardless of whether the returned value is used.


**Global Interpreter Lock (GIL)**
Global Interpreter Lock (GIL): A lock on the CPython interpreter that prevents multiple threads from executing Python bytecode simultaneously. Concurrent threads are interleaved (run sequentially, switching back and forth, or paused and resumed).

Origin: The GIL originated in early CPython during the 1990s, when single-core computing dominated. Multiple threads share the same memory space, and simultaneous access to mutable resources can cause contention and data corruption. Like in structured databases, locks are needed to synchronize access. Rather than configuring individual locks for every object, the GIL was introduced as a global lock to protect shared interpreter state and memory management.

Bypass: True CPU parallelism can be achieved using `multiprocessing`, which creates separate Python processes with independent interpreters and GILs. Native libraries that release the GIL and free-threaded CPython builds also enable parallel execution.

Release: By default, CPython uses a thread-switch interval of approximately 5 milliseconds, allowing other threads opportunities to acquire the GIL. This is not a guaranteed release every 5 milliseconds.

Efficiency release: Many Python libraries release the GIL during I/O or native computations, allowing other threads to execute and improving concurrency.

Race conditions: Occur when two or more threads access shared mutable resources without sufficient synchronization, potentially causing inconsistent results or data corruption. The GIL does not eliminate race conditions.

**what is threaded concurrency?**
Threaded concurrency means high-speed thread switching taking advantage of the GIL release switch

**parent and child thread**
The default python environment is a single thread called the main thread or the parent thread. Processes that run concurrently are referred to as the child thread.

**threading.Thread**
Allows you to create a thread that will function concurrently with the main thread (spinner example FP, chapter 19 needs to execute at the same time as the primality check). The use case needs to be determined, however threads are useful for multiple concurrent actions where waiting is required. 

**multiprocessing**
The primary library used for multiple core processes. 
```python
#example
from multiprocessing import SimpleQueue, Process, cpu_count
from multiprocessing import queues
```

**THEORY: Classic vs Native Coroutines: What is a coroutine? What is a Thread? What is a process?**
It is worth considering coroutines and threads from the standpoint of concurrency. Both are methods of concurrency, meaning they enable multiple tasks to make progress over overlapping periods. Specifically, they use different operational methods to manage concurrent execution.

In general, the answer to "what is a coroutine?" is "a specific function that enables concurrent execution." More specifically, it is a function that supports pausing and resuming execution using `def/yield` (classic coroutines) or `async/await` (native coroutines). Classic coroutines use the generator protocol, whereas native coroutines typically employ an event loop to schedule and resume execution, making them lightweight and efficient for task switching.

Next, consider the three concurrency methods: processes, threads, and coroutines. There is an intuitive hierarchy to their structure. A process is an independent running program, typically containing its own Python interpreter instance, such as when executing a `.py` file. A thread is an execution unit within a process, subject to GIL restrictions and concurrent interleaving in standard CPython. A coroutine is a function based execution mechanism that can suspend and resume, typically operating within a single thread. Classic coroutines are generator based, and generators are a type of iterator.

**threaded primality checker**
This is simply executed without `threading.Thread` instances as there is no use (no real need for switching). 

**multicore primality checker**
The primality checking logic is implemented in `is_prime()`, as described in Chapter 19 of Fluent Python. This function is wrapped in another function, `check()`, which measures execution time and returns a `PrimeResult(n, prime, elapsed)` object.

The general execution structure is `Logic → check(n) → PrimeResult(n, prime, elapsed) → Worker (JobQueue in, ResultQueue out) → start_jobs()`, which distributes work across CPU cores.

The implementation follows a `Process + Queue + Sentinel` pattern using `multiprocessing.Process` and `SimpleQueue`. Two queues are established: `JobQueue` for incoming tasks and `ResultQueue` for completed results. Each worker executes a loop (`while n := jobs.get():`) to retrieve and process jobs. Sentinel values, such as `0`, signal workers to exit, while a custom `report()` function aggregates and displays the results.

---
## 20: concurrent executors
**flags.py**
Flags is a basic program that downloads flag images using `concurrent.futures.ThreadPoolExecutor`. It appends country codes (`cc`) to a base URL to demonstrate the effectiveness of thread-based concurrency for HTTP I/O-bound tasks. This program represents a form of directed web scraping, more generally described as automated HTTP requests, and will therefore be categorized under that heading below.

**HTTP Requests: General Overview**
1. Target Definition: URL (scheme, domain, path), optional query parameters
2. HTTP Method Selection: `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, `OPTIONS`
3. Header Preparations (Optional): Not currently relevant
4. Payload Attachment (Optional): Not currently relevant
5. Request Execution: DNS Resolution: Not relevant for this example
6. Response Handling: Receive status line, read response body
7. Post-Processing: Return/consume data


**1. flags.py: Sequential downloads**

This version downloads flags sequentially without catching request errors. The save_flag function writes the image bytes to a file in DEST_DIR and returns None. It does not create the directory; the main function does that using Path.mkdir().

The get_flag function constructs a URL from the country code and retrieves the image using httpx.get(). Calling resp.raise_for_status() raises an exception for an unsuccessful HTTP status. The function then returns resp.content, which contains the image’s raw bytes.

The download_many function iterates through the sorted country codes, retrieves and saves each flag, and prints each completed code. Setting flush=True flushes the output immediately.

The main function accepts a downloader callable for reuse across implementations. It creates the destination directory, starts a timer, calls the downloader with POP20_CC, and prints the download count and elapsed time.

```python
#example
import time
from pathlib import Path
from typing import Callable

import httpx

POP20_CC = ('CN IN US ID BR PK NG BD RU JP '
            'MX PH VN ET EG DE IR TR CD FR').split()

BASE_URL = 'https://www.fluentpython.com/data/flags'
DEST_DIR = Path('downloaded')


def save_flag(img: bytes, filename: str) -> None:
    (DEST_DIR / filename).write_bytes(img)


def get_flag(cc: str) -> bytes:
    url = f'{BASE_URL}/{cc}/{cc}.gif'.lower()
    resp = httpx.get(url, timeout=6.1,
                    follow_redirects=True)
    resp.raise_for_status()
    return resp.content


def download_many(cc_list: list[str]) -> int:
    for cc in sorted(cc_list):
        image = get_flag(cc)
        save_flag(image, f'{cc}.gif')
        print(cc, end=' ', flush=True)
    return len(cc_list)


def main(downloader: Callable[[list[str]], int]) -> None:
    DEST_DIR.mkdir(exist_ok=True)
    t0 = time.perf_counter()
    count = downloader(POP20_CC)
    elapsed = time.perf_counter() - t0
    print(f'\n{count} downloads in {elapsed:.2f}s')


if __name__ == '__main__':
    main(download_many)
```

**2. flags_threadpool.py: Concurrent downloads with executor.map()**

This version adds threaded concurrency using ThreadPoolExecutor and executor.map(). It reuses the save_flag, get_flag, and main functions from flags.py. It does not add 404 handling or return download statuses.

The download_one function contains the work previously performed inside the sequential loop. It retrieves and saves one flag, prints its country code, and returns that code. Each call runs as a task on a worker thread, and a worker can handle multiple tasks over time.

The download_many function maps download_one over the sorted country codes. Downloads run concurrently, although the result iterator yields values in input order. Exiting the executor’s context waits for its tasks to finish. Converting the results to a list consumes the iterator, and its length gives the download count.

```python
#example
from concurrent import futures

from flags import save_flag, get_flag, main


def download_one(cc: str):
    image = get_flag(cc)
    save_flag(image, f'{cc}.gif')
    print(cc, end=' ', flush=True)
    return cc


def download_many(cc_list: list[str]) -> int:
    with futures.ThreadPoolExecutor() as executor:
        res = executor.map(download_one, sorted(cc_list))

    return len(list(res))


if __name__ == '__main__':
    main(download_many)
```

**3. flags2_sequential.py: Sequential downloads with error handling**

This version adds HTTP and request error handling, a tqdm progress bar, and an optional verbose mode. It imports the save_flag and main functions and the DownloadStatus enum from flags2_common. Unlike the earlier version, the get_flag function receives the base URL as an argument.

The download_one function wraps get_flag with error handling for HTTP 404 responses. A missing flag produces DownloadStatus.NOT_FOUND, while a successfully downloaded and saved flag produces DownloadStatus.OK. Other HTTP status exceptions are raised again, while other exceptions propagate to the caller. Verbose mode prints the country code and its outcome.

The download_many function iterates through the country codes sequentially. When verbose mode is off, it wraps the iterable with tqdm to display progress. It catches other HTTP status errors and request errors, records them as DownloadStatus.ERROR, and counts each outcome using Counter. The concurrency argument is unused but keeps the function’s interface compatible with the concurrent implementations.

The following combines Examples 20-14 and 20-15, with the entry point included.

```python
#example
from collections import Counter
from http import HTTPStatus

import httpx
import tqdm  # type: ignore

from flags2_common import main, save_flag, DownloadStatus

DEFAULT_CONCUR_REQ = 1
MAX_CONCUR_REQ = 1


def get_flag(base_url: str, cc: str) -> bytes:
    url = f'{base_url}/{cc}/{cc}.gif'.lower()
    resp = httpx.get(url, timeout=3.1, follow_redirects=True)
    resp.raise_for_status()
    return resp.content


def download_one(
    cc: str,
    base_url: str,
    verbose: bool = False,
) -> DownloadStatus:
    try:
        image = get_flag(base_url, cc)
    except httpx.HTTPStatusError as exc:
        res = exc.response
        if res.status_code == HTTPStatus.NOT_FOUND:
            status = DownloadStatus.NOT_FOUND
            msg = f'not found: {res.url}'
        else:
            raise
    else:
        save_flag(image, f'{cc}.gif')
        status = DownloadStatus.OK
        msg = 'OK'

    if verbose:
        print(cc, msg)

    return status


def download_many(
    cc_list: list[str],
    base_url: str,
    verbose: bool,
    _unused_concur_req: int,
) -> Counter[DownloadStatus]:
    counter: Counter[DownloadStatus] = Counter()
    cc_iter = sorted(cc_list)
    if not verbose:
        cc_iter = tqdm.tqdm(cc_iter)

    for cc in cc_iter:
        try:
            status = download_one(cc, base_url, verbose)
        except httpx.HTTPStatusError as exc:
            error_msg = 'HTTP error {resp.status_code} - {resp.reason_phrase}'
            error_msg = error_msg.format(resp=exc.response)
        except httpx.RequestError as exc:
            error_msg = f'{exc} {type(exc)}'.strip()
        except KeyboardInterrupt:
            break
        else:
            error_msg = ''

        if error_msg:
            status = DownloadStatus.ERROR
        counter[status] += 1
        if verbose and error_msg:
            print(f'{cc} error: {error_msg}')

    return counter


if __name__ == '__main__':
    main(download_many, DEFAULT_CONCUR_REQ, MAX_CONCUR_REQ)
```

**4. flags2_threadpool.py: Concurrent downloads with error handling**

This version combines threaded concurrency with error handling, a progress bar, and optional verbose output. It reuses the download_one function from flags2_sequential, including its retrieval, saving, and 404 handling.

The download_many function creates a ThreadPoolExecutor with concur_req worker threads. It submits each download separately using executor.submit() and stores a mapping from each returned future to its country code.

The as_completed function yields futures as they finish, allowing results to be processed in completion order. When verbose mode is off, tqdm wraps this iterator to display progress.

Calling future.result() retrieves the download status or raises the exception produced by the worker. The function catches HTTP and request errors, counts each outcome, and uses the future mapping to identify failed country codes in verbose mode. The shared main function receives the downloader and the default and maximum concurrency settings.

```python
#example
from collections import Counter
from concurrent.futures import ThreadPoolExecutor, as_completed

import httpx
import tqdm  # type: ignore

from flags2_common import main, DownloadStatus
from flags2_sequential import download_one

DEFAULT_CONCUR_REQ = 30
MAX_CONCUR_REQ = 1000


def download_many(
    cc_list: list[str],
    base_url: str,
    verbose: bool,
    concur_req: int,
) -> Counter[DownloadStatus]:
    counter: Counter[DownloadStatus] = Counter()

    with ThreadPoolExecutor(max_workers=concur_req) as executor:
        to_do_map = {}
        for cc in sorted(cc_list):
            future = executor.submit(download_one, cc,
                                     base_url, verbose)
            to_do_map[future] = cc

        done_iter = as_completed(to_do_map)
        if not verbose:
            done_iter = tqdm.tqdm(done_iter, total=len(cc_list))

        for future in done_iter:
            try:
                status = future.result()
            except httpx.HTTPStatusError as exc:
                error_msg = 'HTTP error {resp.status_code} - {resp.reason_phrase}'
                error_msg = error_msg.format(resp=exc.response)
            except httpx.RequestError as exc:
                error_msg = f'{exc} {type(exc)}'.strip()
            except KeyboardInterrupt:
                break
            else:
                error_msg = ''

            if error_msg:
                status = DownloadStatus.ERROR
            counter[status] += 1
            if verbose and error_msg:
                cc = to_do_map[future]
                print(f'{cc} error: {error_msg}')

    return counter


if __name__ == '__main__':
    main(download_many, DEFAULT_CONCUR_REQ, MAX_CONCUR_REQ)
```

Source: Fluent Python, second edition, Chapter 20, Examples 20-2, 20-3, and 20-14 through 20-16. The flags2 examples require the book’s separate flags2_common.py module. The external packages used are httpx and tqdm.

---
## 21: asynchronous programming
**What is asynchronous programming?**
Asynchronous programming is a concurrency approach that allows tasks to make progress while other tasks wait, typically for I/O. An event loop schedules tasks and resumes them when they can continue.

Python’s standard library provides asyncio, but the async and await keywords are language features that also work with other asynchronous libraries.

**async**
The async keyword is used with def, for, and with to define coroutine functions, asynchronous iteration, and asynchronous context management. It does not automatically make operations nonblocking or schedule them concurrently.

**native coroutine functions**
A function defined with async def is a native coroutine function, provided it does not contain yield. Its body can use await to wait for awaitable objects.

Calling a coroutine function creates a coroutine object without executing its body. The coroutine must be awaited or scheduled to run.

**asynchronous iteration**
An async for loop consumes an asynchronous iterable. Its protocol uses `__aiter__()` to obtain an asynchronous iterator and `__anext__()` to obtain an awaitable for the next item.

Asynchronous iteration allows waiting between items, but it does not automatically run operations concurrently or produce results in completion order. The iterable determines the order.

**asynchronous context management**
An async with statement manages resources whose setup or cleanup may require awaiting. Its protocol uses `__aenter__()` and `__aexit__()`, which return awaitables.

Common uses include HTTP clients, database connections, transactions, locks, and semaphores.

**anatomy of an asynchronous program**
A coroutine object represents an execution that can be started, suspended, and resumed. The event loop schedules tasks and callbacks, monitors I/O, and resumes work when awaited operations become ready.

A Task wraps a coroutine and schedules its execution. Calling asyncio.create_task() creates a task on the running event loop.

A Future represents an eventual result or exception. Awaiting a pending future suspends the current coroutine until the future completes.

**coroutine functions vs. coroutine objects**
Given a definition such as async def func(), the name func refers to the coroutine function. Calling func() returns a coroutine object.

The returned coroutine object is not the function’s final return value and is not itself a code object. It contains execution state and references the underlying code through its cr_code attribute.

Assigning func() to a variable does not start execution. Awaiting that coroutine runs it, and the await expression produces its return value once it completes.
```python
#example
import asyncio


async def func() -> int:
    return 42


async def main() -> None:
    coro = func()
    result = await coro
    print(result)


if __name__ == '__main__':
    asyncio.run(main())
```

**awaitables and suspension**
The await keyword identifies a point where a coroutine may suspend while waiting for an operation. Suspension is not guaranteed: an awaitable that completes immediately may allow execution to continue without yielding control.

Common awaitables include native coroutine objects, asyncio Tasks, and asyncio Futures. Custom objects can also be awaitable by implementing __await__().

Suspending a coroutine does not inherently release the GIL. Coroutine scheduling and GIL management are separate mechanisms.

**basic asyncio**
For a standalone program, asyncio.run(main()) manages the event loop, runs the main coroutine, and closes the loop afterward.

Inside a running coroutine, asyncio.get_running_loop() returns the active event loop. This provides access to methods such as loop.getaddrinfo() and loop.run_in_executor().

Manual loop creation and management are available for specialized integrations but are generally unnecessary for ordinary application code. In environments such as Jupyter that already run an event loop, use await main() directly.

The asyncio library includes networking and synchronization tools. Higher level protocols such as HTTP commonly use another library, such as HTTPX, which integrates with the event loop.

**running multiple coroutines**
Awaiting coroutine calls one after another executes those operations sequentially. To make them run concurrently, schedule them as tasks or pass them to a coordinating function such as asyncio.gather().

The gather function schedules coroutine arguments concurrently and returns their results in argument order once they all complete successfully. The as_completed function allows results to be processed in completion order.

```python
#example
import asyncio


async def work(value: int, delay: float) -> int:
    await asyncio.sleep(delay)
    return value


async def main() -> None:
    results = await asyncio.gather(
        work(1, 0.3),
        work(2, 0.1),
        work(3, 0.2),
    )
    print(results)  # [1, 2, 3]


if __name__ == '__main__':
    asyncio.run(main())
```

**benefits of coroutines**
Coroutines generally require less memory and scheduling overhead than operating system threads. They are especially useful for handling many I/O operations concurrently.

Threads are also useful for I/O, particularly when working with blocking libraries. In standard CPython builds with the GIL enabled, threads generally do not execute CPU intensive Python code in parallel. Processes are a common choice for that work, while native extensions that release the GIL may also benefit from threads.

**blogdom.py: DNS probing**
This program uses short Python keywords to construct domain names such as for.dev and checks whether they resolve through DNS.

DNS probing does not send an HTTP request or retrieve webpage content. Successful resolution indicates that address information is available; it does not establish that a website works or that the domain is available for registration.

The probe coroutine obtains the running event loop and awaits loop.getaddrinfo(). It returns the domain and True when resolution succeeds, or the domain and False when socket.gaierror occurs. Domain names are passed without an HTTP prefix.

The main coroutine constructs the domain names and creates a coroutine object for each probe. The as_completed function runs the probes concurrently and allows their results to be displayed in completion order.
```python
#example
import asyncio
import socket
from keyword import kwlist

MAX_KEYWORD_LEN = 4


async def probe(domain: str) -> tuple[str, bool]:
    loop = asyncio.get_running_loop()
    try:
        await loop.getaddrinfo(domain, None)
    except socket.gaierror:
        return (domain, False)
    return (domain, True)


async def main() -> None:
    names = (kw for kw in kwlist if len(kw) <= MAX_KEYWORD_LEN)
    domains = (f'{name}.dev'.lower() for name in names)
    coros = [probe(domain) for domain in domains]

    for coro in asyncio.as_completed(coros):
        domain, found = await coro
        mark = '+' if found else ' '
        print(f'{mark} {domain}')


if __name__ == '__main__':
    asyncio.run(main())
```

**flags_asyncio.py: Asynchronous flag downloads**
This version performs the flag downloads asynchronously using HTTPX’s AsyncClient class. It reuses BASE_URL, save_flag, and main from flags.py.

The get_flag coroutine awaits an HTTP request and returns the response’s image bytes. In this example, the response body has already been loaded by the awaited request, so `resp.read()` is synchronous. Unlike the earlier sequential version, this listing does not call raise_for_status().

The download_one coroutine retrieves and saves one flag, prints its country code, and returns that code. Saving remains synchronous in this introductory example.

The supervisor coroutine manages a shared HTTP client with async with. It builds a list of download coroutines and passes them to asyncio.gather(), which runs them concurrently. The function returns the number of results.

The download_many function remains synchronous so it can be passed to the existing main function. It uses asyncio.run() to execute supervisor and return its result.
```python
#example
import asyncio

from httpx import AsyncClient

from flags import BASE_URL, save_flag, main


async def download_one(client: AsyncClient, cc: str):
    image = await get_flag(client, cc)
    save_flag(image, f'{cc}.gif')
    print(cc, end=' ', flush=True)
    return cc


async def get_flag(client: AsyncClient, cc: str) -> bytes:
    url = f'{BASE_URL}/{cc}/{cc}.gif'.lower()
    resp = await client.get(
        url,
        timeout=6.1,
        follow_redirects=True,
    )
    return resp.read()


def download_many(cc_list: list[str]) -> int:
    return asyncio.run(supervisor(cc_list))


async def supervisor(cc_list: list[str]) -> int:
    async with AsyncClient() as client:
        to_do = [
            download_one(client, cc)
            for cc in sorted(cc_list)
        ]
        res = await asyncio.gather(*to_do)

    return len(res)


if __name__ == '__main__':
    main(download_many)
```

**asynchronous context managers**
An asynchronous context manager follows the same general structure as a synchronous context manager, but its entry and exit operations can suspend execution.

The async with statement awaits `__aenter__()` before entering the block and awaits `__aexit__()` when leaving it, including when an exception occurs. Whether the exception is suppressed depends on the context manager.

For HTTPX, this manages the client’s lifetime and closes its connections when the block ends. Individual requests still require explicit await expressions.
```python
#example
import asyncio

from httpx import AsyncClient


async def main() -> None:
    async with AsyncClient() as client:
        response = await client.get('https://www.python.org')
        response.raise_for_status()
        print(response.status_code)


if __name__ == '__main__':
    asyncio.run(main())
```

**blocking operations inside asynchronous code**
A blocking operation executed on the event loop’s thread prevents other tasks on that loop from progressing until it returns. Declaring a function with async def does not change this behaviour.

Asynchronous programs can still contain ordinary synchronous code. The goal is to avoid blocking the loop for significant periods, particularly during I/O or expensive computation.

Blocking I/O can be delegated to a thread. CPU intensive Python work can be delegated to a process pool when parallel execution is needed.

**delegating work with run_in_executor()**
The event loop’s run_in_executor() method submits a synchronous callable to an executor and returns an awaitable future.

Passing None selects the default executor, which is normally a ThreadPoolExecutor created when needed. An explicit executor can be supplied instead.

Inside a coroutine, obtain the loop using asyncio.get_running_loop(). Additional positional arguments passed after the callable are forwarded to that callable.
```python
#example
import asyncio
from pathlib import Path


def blocking_io() -> str:
    return Path('data.txt').read_text()


async def main() -> None:
    loop = asyncio.get_running_loop()
    result = await loop.run_in_executor(None, blocking_io)
    print(result)


if __name__ == '__main__':
    asyncio.run(main())
```

**delegating work with asyncio.to_thread()**
The to_thread function provides a simpler way to run a synchronous callable in a separate thread. It accepts positional and keyword arguments for that callable.

Calling it returns a coroutine, which must be awaited or scheduled. It can be passed to asyncio.gather() alongside other awaitables.
```python
#example
import asyncio
from pathlib import Path


def blocking_io() -> str:
    return Path('data.txt').read_text()


async def main() -> None:
    result, _ = await asyncio.gather(
        asyncio.to_thread(blocking_io),
        asyncio.sleep(1),
    )
    print(result)


if __name__ == '__main__':
    asyncio.run(main())
```

**semaphores**
A semaphore controls how many tasks may enter a protected section concurrently. It maintains a counter representing available permits.

Acquiring a permit decrements the counter. When no permits remain, further acquisition attempts wait until another task releases one. Releasing a permit increments the counter.

An asyncio.Semaphore can be used with async with to acquire a permit on entry and release it on exit. All participating tasks must share the same semaphore for the limit to apply.
```python
#example
import asyncio


async def limited_work(
    value: int,
    semaphore: asyncio.Semaphore,
) -> int:
    async with semaphore:
        await asyncio.sleep(1)
        return value


async def main() -> None:
    semaphore = asyncio.Semaphore(2)
    results = await asyncio.gather(
        *(limited_work(value, semaphore) for value in range(5))
    )
    print(results)


if __name__ == '__main__':
    asyncio.run(main())
```

**asynchronous generators**
An asynchronous generator function is defined with async def and contains yield. Calling it returns an asynchronous generator object, which can be consumed with async for.

The generator can await operations between yielded values. A yield expression supplies a value to the consumer and suspends the generator until the next item is requested.

Using yield await waits for an awaitable and then yields its result. It does not automatically schedule other operations concurrently or determine completion order.
```python
#example
import asyncio


async def values():
    for value in range(3):
        await asyncio.sleep(1)
        yield value


async def main() -> None:
    async for value in values():
        print(value)


if __name__ == '__main__':
    asyncio.run(main())
```

**asynchronous iteration protocol**

An asynchronous iterable implements `__aiter__()`, which returns an asynchronous iterator. That iterator implements `__anext__()`, which returns an awaitable for the next value.

When no values remain, the next-item operation raises StopAsyncIteration. An async for loop handles this exception automatically.

Asynchronous generators implement this protocol automatically. Other examples include asynchronous data streams and database cursors that support asynchronous iteration.

An object is asynchronously iterable because it implements the asynchronous iteration protocol, not because its implementation contains multiple await expressions.

Calling an asynchronous generator function produces an asynchronously iterable object. Calling an ordinary coroutine function produces a coroutine object, which is awaitable but is not automatically asynchronously iterable.

Ordinary lists, dictionaries, and sets use synchronous iteration. Their values being awaitable does not make the containers asynchronously iterable.

**asynchronous comprehensions and generator expressions**
Asynchronous comprehensions provide compact syntax for consuming asynchronous iterables. They can build lists, sets, or dictionaries, and may include filtering conditions.

An asynchronous generator expression produces values lazily as its consumer requests them. These constructs use the asynchronous iteration protocol; they do not automatically introduce concurrent execution.

```python
#example
import asyncio


async def values():
    for value in range(5):
        await asyncio.sleep(0.1)
        yield value


async def main() -> None:
    numbers = [value async for value in values()]

    even_numbers = [
        value
        async for value in values()
        if value % 2 == 0
    ]

    squares = (value ** 2 async for value in values())

    print(numbers)
    print(even_numbers)

    async for square in squares:
        print(square)


if __name__ == '__main__':
    asyncio.run(main())
```

**sequential execution versus completion order**
An asynchronous generator that awaits each operation inside a normal for loop starts those operations one at a time. Each result becomes available before the next operation begins.

If tasks have already been scheduled concurrently, awaiting them one at a time in a chosen order does not stop their concurrent execution. It only determines the order in which the consumer receives their results.

The gather function runs its awaitables concurrently and returns a complete result list in input order.

The as_completed function allows results to be consumed in completion order. The book uses a normal for loop over its output, awaiting each yielded object to obtain the next completed result.

The async for statement does not determine scheduling or result order. It requests successive values from an asynchronous iterator, whose implementation determines how they are produced.

```python
#example
import asyncio


async def work(value: int, delay: float) -> int:
    await asyncio.sleep(delay)
    return value


async def sequential_results():
    for value, delay in [(1, 0.3), (2, 0.1), (3, 0.2)]:
        yield await work(value, delay)


async def completion_order_results():
    coros = [
        work(1, 0.3),
        work(2, 0.1),
        work(3, 0.2),
    ]
    for coro in asyncio.as_completed(coros):
        yield await coro


async def main() -> None:
    async for result in sequential_results():
        print(result)  # 1, then 2, then 3

    async for result in completion_order_results():
        print(result)  # 2, then 3, then 1


if __name__ == '__main__':
    asyncio.run(main())
```

---
# V: Metaprogramming

---
## 22 : dynamic attributes and properties ✪

**\__getattr__**
Only invoked when the usual attribute look fails to retrieve a value (not to be confused with `__getattribute__`). Tell the class what to do when the attribute lookup does not work.

```python
#signature
__getattr__(self, name):
```
- `name` : the attribute which does not exist

**getattr()**
Built-in function for attribute retrieval. 
```python
#signature
getattr(obj, name, default):
```
- `name` : is a string for the actual 
- `default` : is returned if no attribute is found

**\__new__**
Objects themselves are not created by `__init__`. This special method initializes an existing instance, typically by binding parameter values to instance attributes through `self`.

The special method `__new__` is the actual constructor, responsible for creating and returning a new class instance. It is inherited from `object` unless overridden and normally does not need to be called explicitly.

In CPython, the underlying implementation of `object` is written in C, not Cython. Its instance creation can loosely be understood as allocating a new object in memory. Once `__new__` returns an instance of the requested class, python invokes `__init__` to initialize it.

The `__new__` special method is an implicitly static method $\rightarrow$ meaning it receives `cls` rather than `self` (`@staticmethod` decorator is not required). Its primary application is customizing instance creation, particularly when subclassing immutable types such as tuple, str, or int. 

```python
#signature
def __new__(cls, *args, **kwargs):
    ...
```

- `cls` : references the class whose instance is being created
- `*args`, `**kwargs` : accept positional and keyword arguments supplied during instantiation. These may be used to customize object creation
- `super().__new__(cls, ...)` : delegates instance creation to the superclass (the accepted arguments depend on that superclass $\rightarrow$ in particular, `object.__new__()` does not accept arbitrary initialization arguments)

**property()**
Built-in function used to create a read-only attribute (dynamic attribute access). This function (decorator) is used when you want a function to be accessed like an attribute instance on a class. You call this on a method with the same name as an instance attribute. You can use property() in two ways. 

The first way is to use a decorator on the function you wish to use decorate for dynamic attribute access. In order for the getter to work, the setter will need to have set a class attribute for the getter to have something to retrieve (del method too). In the following example, if the del statement runs, $\rightarrow$ it will cause AttributeLookupError (the property was destroyed).

```python
#example
class DecoPropClass:
    @property
    def my_attr(self):
        return self._my_attr

    @my_attr.setter
    def my_attr(self, value):
        self._my_attr = value

    @my_attr.deleter
    def my_attr(self):
        del self._my_attr
                             
x = DecoPropClass()
x.my_attr = 'PROP'
x.my_attr
#del x.my_attr
x.my_attr
```
```python
>>> 'PROP'
```

The second way is to create a property is to create individual getter, setter and deleter functions assign each function to the property() constructor manually. This method requires that the property is a class attribute, that it is defined after its selected methods, and that it uses a separate name to avoid infinite recursion. 

```python
#example
class ManualPropClass:
    def getr(self):
        return self._my_attr #protected value

    def setr(self, value):
        self._my_attr = value + '!'
        
    def delr(self):
        del self._my_attr

    my_attr = property(fget=getr,
                        fset=setr,
                        fdel=delr)
                
x = ManualPropClass()
x.my_attr = 'PROP'
x.my_attr
x.my_attr
```
```python
>>> 'PROP!'
```

The signature for property() is given below. 
```python
#signature
property(fget=None, fset=None, fdel=None, doc=None)
```
- `fget` : getter function
- `fset` : setter function
- `fdel` : deleter function $\rightarrow$ functions with `del` statement 
- `doc` : doc string $\rightarrow$ add an optional docstring

**\__dict__**
The `__dict__` is a mapping of an object's local namespace. Almost every user-defined instance has a `__dict__` by default (comprehensive $\rightarrow$ modules, classes, user-defined class instances, functions). Generally, built-in type instances (list, string, etc.), methods, and `__slots__` instances do not have a `__dict__`. 

The `__dict__` of a class and class instances are different. The class dict holds attributes stored directly on the class (not inherited attributes). The instance dict holds attributes stored directly on that particular instance (not all class metadata resides in `__dict__` $\rightarrow$ i.e.`__bases__` `__mro__` are accessed separately). 

**bunch argument (bunch idiom)**
Trick to instantly upload key-value pairs as attributes to class namespace (`__dict__`). 

```python
#example
class Animals:
    def __init__(self, **kwargs):
        self.__dict__.update(kwargs)

animal_dict = {
    'cat': 'bombay',
    'dog': 'shih tzu',
    'bird': 'parrot'
}

animals = Animals(**animal_dict)

print(animals.cat)     
print(animals.dog)      
print(animals.bird)      

print(animals.__dict__)
```
```python
>>> {'cat': 'bombay', 'dog': 'shih tzu', 'bird': 'parrot'}
```

**cacheing with @property and @functools.cache**
Storing properties in a cache is useful as properties can be costly to compute as a read-only attributes. If you stack property + cache decorators, the values will be cached in a global unified cache as visible in cache_info(). If you stack @property + @cache, computed values exist at the function level and each instance will cache and retrieve form the same global cache. There are no direct memory implications of stacking these decorators, but it means instance values wont hold isolated caches and will search through a larger global cache to retrieve values. 

In the example below, the cache is not stacked with property $\rightarrow$ the setter uses a separate function with cache as the cache must decorate the function (callable) running computation (which is why property must property must proceed cache when being stacked). 

```python
#example
import math
import time
from functools import cache

class Calc:
    @cache
    def comp(self, value):
        time.time
        result = sum(
            math.sin(n)**2 + math.cos(n)**2
            for n in range(value)
        )
        return result

    @property
    def my_value(self):
        return self._my_value

    @my_value.setter
    def my_value(self, value):
        self._my_value = self.comp(value)

x = Calc()
y = Calc()
%time x.my_value = 5_000_000
%time x.my_value = 5_000_000 #second time is faster
%time y.my_value = 5_000_000
%time y.my_value = 5_000_000 #second time is faster
print(f'xcache: {x.comp.cache_info()}')
x.comp.cache_clear() #cache cleared
print(f'ycache: {y.comp.cache_info()}') #x and y share cache
x.my_value, y.my_value
```
```python
>>> CPU times: user 1.56 s, sys: 5.28 ms, total: 1.57 s
>>> Wall time: 1.57 s
>>> CPU times: user 3 μs, sys: 0 ns, total: 3 μs
>>> Wall time: 5.96 μs 
>>> CPU times: user 1.55 s, sys: 2.33 ms, total: 1.55 s
>>> Wall time: 1.55 s
>>> CPU times: user 3 μs, sys: 1 μs, total: 4 μs
>>> Wall time: 5.01 μs
>>> xcache: CacheInfo(hits=2, misses=2, maxsize=None, currsize=2)
>>> ycache: CacheInfo(hits=0, misses=0, maxsize=None, currsize=0)
>>> (5000000.0, 5000000.0)
```

**@functools.cached_property**
You can use the dedicated cached_property decorator from functools, which creates an isolated instance-level cache and stores its value in `instance.__dict__`. When values are changed under property, they are simply updated in the instance's `__dict__`.
```python
#example
import math
from functools import cached_property

class PropClass:
    def __init__(self, value):
        self.value = value
    
    @cached_property
    def compute(self):
        return sum(math.sin(val) + math.cos(val) for val in range(self.value))

x = PropClass(50_000_000)
%time x.compute
%time x.compute
x.__dict__ #value is cached here
```
```python
>>> CPU times: user 8.77 s, sys: 7.52 ms, total: 8.78 s
>>> Wall time: 8.79 s
>>> CPU times: user 2 μs, sys: 0 ns, total: 2 μs
>>> Wall time: 4.05 μs
>>> 0.9596258066204428
>>> {'value': 50000000, 'compute': 0.9596258066204428}
```

**property factories (luciano ramalho)**
Describes a function that dynamically returns a property object using an attribute name. The property factory makes creating properties much easier than manually adding getters and setters to each property. 

In the example below, quantity is the property factory. The property factory returns a property() loaded with custom getter and setter functions. The storage_name will ultimately be the property name on the class. The properties, weight and price are bound to the property() object and are first in attribute lookup protocol. The init special method is nullified and only there to demonstrate that the only point of an init in such an instance is to have values initialized from the constructor call. 

```python
#example
def quantity(storage_name):
    def qty_getter(instance):
        return instance.__dict__[storage_name]

    def qty_setter(instance, value):
        if value > 0:
            instance.__dict__[storage_name] = value
        else:
            raise ValueError('value must be > 0')

    return property(qty_getter, qty_setter)

class LineItem:
    weight = quantity('weight')
    price = quantity('price')
	'''
    def __init__(self, description, weight, price):
        self.description = description
        self.weight = weight
        self.price = price
	'''
    def subtotal(self):
        return self.weight * self.price
```


**essential attributes for functions and Attribute Handling**

| Attribute   | Type              | Description                                                                                                                                                                | Example                       |
| ----------- | ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| `__class__` | Special Attribute | References an object's class, usually equivalent to `type(obj)`. Special methods such as `__getattr__` are looked up on the object's class.                                | `obj.__class__`               |
| `__dict__`  | Special Attribute | Dictionary storing an object's writable instance attributes. Allows arbitrary attributes to be dynamically added when supported. Not all Python objects have a `__dict__`. | `obj.__dict__`                |
| `__slots__` | Special Attribute | Declares permitted instance attributes and can reduce memory usage by avoiding an instance `__dict__`. A `__dict__` may still exist if inherited or explicitly included.   | `__slots__ = ('name', 'age')` |

**built-In functions for attribute handling**

|Function/Method|Type|Description|Example|
|---|---|---|---|
|`dir([object])`|Built-in Function|Returns a human-friendly list of available attributes. Can be customized using `__dir__()`.|`dir(obj)`|
|`getattr(object, name[, default])`|Built-in Function|Retrieves a named attribute dynamically. Raises `AttributeError` if missing unless a default is supplied.|`getattr(obj, 'name', None)`|
|`hasattr(object, name)`|Built-in Function|Returns `True` if the named attribute exists and can be retrieved, otherwise `False`.|`hasattr(obj, 'name')`|
|`setattr(object, name, value)`|Built-in Function|Assigns a value to a named attribute, creating or overwriting it if permitted.|`setattr(obj, 'age', 25)`|
|`vars([object])`|Built-in Function|Returns an object's `__dict__`. Without arguments, returns the local namespace, equivalent to `locals()`.|`vars(obj)`|
|`__delattr__(self, name)`|Special Method|Invoked when deleting an attribute using `del`. May invoke a property's deleter.|`del obj.name`|
|`__dir__(self)`|Special Method|Invoked by `dir(obj)` to customize the list of available attribute names.|`dir(obj)`|
|`__getattr__(self, name)`|Special Method|Called when normal attribute lookup fails. Acts as a fallback after `__getattribute__`.|`obj.missing_attr`|
|`__getattribute__(self, name)`|Special Method|Intercepts every normal instance attribute lookup. Custom implementations should generally delegate to `super().__getattribute__(name)` to avoid recursion.|`obj.name`|
|`__setattr__(self, name, value)`|Special Method|Intercepts attribute assignments, including those made through `setattr()`.|`obj.name = 'Adam'`|

---
## 23 : attribute descriptors ✪
**descriptor**
Descriptors are used by classes to manage their attributes $\rightarrow$ used for custom validation logic i.e. preventing an attribute value from equalling zero, value concatenation, etc.

Descriptors are implemented as class attributes on a class using class attribute syntax (i.e. simple variable assignment) as they are accessible to all instances. But, even though descriptors are class-level attributes, they behave as instance attributes in that they take parameters which adhere to the specified validation logic in the descriptor class. 

**attribute descriptor**
Is a special kind of descriptor that is called to manage the attributes of another class which it manages. A property is an attribute descriptor with built-in features that make creating an attribute descriptor easier whereas an attribute descriptor gives full control of the descriptor class.

**descriptor protocol**
Implements `__get__`, `__set__`, or `__del__`. The example below shows the default implementation with `__set_name__` (which works behind the scenes by retrieving the name directly from the populated namespace during execution), which automatically forwards the  attribute descriptors name (`weight`, `price`) to the descriptor. Without `__set_name__`, the attribute descriptor`Positive` needs the `__init__` method and `LineItem` needs to pass the attribute descriptor name to the descriptor. 

When you call the descriptor from the class and `insance=None` $\rightarrow$ it will return the managed attribute. Instead, you can add if `instance = None: return self`, which will return the actual descriptor object i.e. `<__main__.Quantity object at 0x1096dcaa0>`. The following show the different passed arguments from the instance, `obj.descriptor_obj` and the class, `cls.descriptor_obj`. 

```
#pseudocode example

#receives:
dscrptr.__get__(self=dscrptr_obj, instance=obj, owner=type(obj))

#receives:
dscrptr.__get__(self=dscrptr_obj, instance=None, owner=Class)
```

In the example below, in the descriptor class you should not save to self $\rightarrow$ if you save to self  (`self.__dict__[self.storage_attribute] = value`) the attribute will save to the descriptor class instance. In other words, self will refer to descriptor instances in the managed class. This means all instances will have access to the same `__dict__` and continue to overwrite one another. You must save to the instance (`instance.__dict__[self.storage_name] = value`) so that each value saves to an isolated address.

```python
#example
class Positive:
    def __set_name__(self, owner, name):
        self.storage_name = f"_{name}"
    '''    
	def __init__(self, name):
		self.storage_name = name
	'''
    def __get__(self, instance, owner):
        if instance is None:
            return self #return cls if called from cls
        return instance.__dict__[self.storage_name]

    def __set__(self, instance, value):
        if value <= 0:
            raise ValueError("Value must be positive")
        instance.__dict__[self.storage_name] = value

    def __delete__(self, instance):
        del instance.__dict__[self.storage_name]

class LineItem:
    weight = Positive() #__set_name__ forwards name
    price = Positive() #__init__ forwards Positive('price') 
    
    def __init__(self, weight, price):
        self.weight = weight
        self.price = price

    def subtotal(self):
        return self.weight * self.price

item = LineItem(10, 5)

print(item.weight)
item.weight = 20
print(item.subtotal())

del item.weight
```

**types of descriptors**
There are three core types of descriptors. They are all prioritized differently in the lookup pipeline. See the table below. 

| Type                                | Required methods                                 | Instance attribute precedence       | Examples                                       |
| ----------------------------------- | ------------------------------------------------ | ----------------------------------- | ---------------------------------------------- |
| Data descriptor (overriding)        | `__get__` and `__set__` and/or `__delete__`      | Descriptor takes precedence         | `property`, custom validating descriptors      |
| Non-data descriptor (nonoverriding) | `__get__` only                                   | Instance attribute takes precedence | Regular methods, `staticmethod`, `classmethod` |
| Write-only data descriptor          | `__set__` and/or `__delete__`, without `__get__` | Controls assignment/deletion        | Custom write-only descriptors                  |


**descriptors pre-Py 3.6**
Why was the original syntax `item = Descriptor('item')` required? In the old pattern it was necessary to tell the descriptor object what its instance name was in the managed class i.e. `attr = Descriptor('attr')` so that `__get__` and `__set__` could reference the storage attribute name, `self.storage`. The 'getting' and 'setting' was done entirely manually. The assignment to `attr` in the managed class effectively shadows.

No `__get__` method with `__set_name__`.Although you can add a `__get__` method to your descriptor class, with `__set_name__` it is not needed because once the name is saved in the managed instance `__dict__`, regular attribute lookup falls back to the instances `__dict__` after it checks for data descriptors first (see 'The Lookup Pipeline' above). 

Note, the lookup order does not change with `__set_name__` meaning that you can omit `__get__` in both the new and the old pattern by exploiting the attribute lookup protocol. However, this only works if the storage attribute name is the same as the descriptor instance name i.e. for `self.storage = name` and `cls.attr` where `name == attr` and `name` is the storage name internally in the descriptor class, and `attr` is the descriptor instance name

**attribute lookup protocol**
When you call an attribute on a class the default methods used to search for the attribute are `__getattribute__`, `__setattr__`, and `__getattr__`. 

The methods `__getattribute__` and `__setattr__` are the default methods for basic attribute getting and setting. Anytime you get an attribute (`instance.attr`) or set an attribute (`instance.attr = x`), through dot notation, these methods get called. The method `__getattr__` is a fallback method when `__getattribute__` cannot find an attribute and returns AttributeError. 

The use of `self.attr` automatically invokes the attribute lookup protocol calling getters and setters respectively, which prevents the use of `self.attr` in their description. To avoid this problem, which causes RecursionError, use `super()` to return the parent method. 

```python
#example
class AttrLookup:
    def __init__(self, value):
        self.value = value

    def __getattribute__(self, name):
        return super().__getattribute__(name)
    
    def __setattr__(self, name, value):
        super().__setattr__(name, value + 5)
    
    def __getatr__(self):
        print('LOOKUP FAILED')

x = AttrLookup(5)
x.value
x.value = 10
x.value
```
```python
>>> 15
```

**the lookup pipeline** 
Attribute lookup follows a specific order implemented inside. The order depends on whether a data descriptor is present on the class.  
$$
\begin{gathered}
\boxed{\text{1. data descriptors}}\\ 
\downarrow \\
\boxed{\text{2. instance dictionar}}\\
\downarrow \\
\boxed{\text{3. non-data descriptors}}\\
\downarrow \\
\boxed{\text{4. other class attributes}}\\
\downarrow \\
\boxed{\text{5. getattr fallbacl}}\\
\end{gathered}

$$


1. Data descriptor check (class + MRO): If the class attribute defines `__set__` or `__delete__`, it is returned before the instance dictionary is consulted.*  

2. Instance dictionary (`obj.__dict__`): If no data descriptor is found, Python next checks the instance for the attribute.
 
3. Non-data descriptors (class + MRO): If the class attribute defines only `__get__`, it is used unless the instance already had the attribute.
 
4. Other class attributes (class + MRO)
Plain attributes and methods are returned next.  
 
5. `__getattr__` fallback*  
Used only when all previous steps fail.  

⚠ Data descriptors override instance attributes; non-data descriptors do not.  
⚠ MRO is always searched after the instance dictionary, except in the data-descriptor case.

**methods are non-overriding descriptors**
All methods implicitly have `__get__` on them as part of the descriptor protocol in python and are invoked using `__get__`.

Because methods have a `__get__`, they are non-overriding descriptors which means that they are shadowed by instance attributes with the same name.

When we call a method from the instance, its returned as a bound method `<bound method Class.mthd of <__main__.Class object at ....>>` but when its called from the class it returns a function `<function Class.mthd at 0x104981d00>`. 

```
#pseucode example

obj.__class__.method.__get__(obj, obj.__class__)

functions + automatic binding (via __get__)
```

Effectively, a method is not a method, but a non-data descriptor, which is a function that uses `__get__` as a binding to `__self__`, as `(obj, function)`. A method returns two objects: 1. the original function object `method.__func__` and 2. the instance `method.__self__`.

Effectively, a method is a function defined on a class that, when accessed through an instance, becomes a bound method by means of the function's built-in `__get__` descriptor and returns the instance, `__self__` and the function `__func__`. 

In the example below, `self.method` shadows the class method and therefore causes TypeError.

```python
#lookup pipeline
class MethodOverride:
    def __init__(self, method):
        self.method = method

    def method(self):
        print('METHOD')

x = MethodOverride(5)
x.method()
```
```python
>>> TypeError
```

**\__call__ is the part of the method that actually performs the code**
The special method `__call__` is responsible for executing a method's code, while `__get__` is responsible for binding the function to an instance. When accessing a method, Python uses the descriptor protocol, conceptually represented by `obj.__class__.f.__get__(obj, obj.__class__)`, to produce a bound method. The bound method can then be executed using `bound.__call__(*args, **kwargs)`, passing the actual arguments to the function. Thus, the complete process can be represented as `obj.__class__.f.__get__(obj, obj.__class__).__call__(*args, **kwargs)`, where `__get__` performs the binding and `__call__` performs the execution.

---
> The following table is added for the sake of clarity on the terms used in this section.

| word                | definition                                                                                                                                                                                                                          |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| descriptor class    | the descriptor itself, a class containing either `__set__`, `__get__`, or `__del__`                                                                                                                                                 |
| managed class       | the class whose attributes are managed by descriptors.                                                                                                                                                                              |
| descriptor instance | an instance of the descriptor class called in the body of the managed class i.e. `weight = Quantity('weight')`, where `Quantity()` is an instance of the descriptor class called as a  regular class instance i.e. `x = Class(...)` |
| managed instance    | an instance of the managed class                                                                                                                                                                                                    |
| storage attribute   | the storage attribute is the location where the descriptor data is returned i.e. `instance.__dict__`                                                                                                                                |
| managed attribute   | the managed attribute is the is the public interface that uses the descriptor instance behind the scenes                                                                                                                            |

---
## 24 : class metaprogramming

**\__qualname__**
Returns the name of a class or function, including its enclosing classes or functions, relative to the module's top level (i.e. the pathname of an object as displayed from its top root). Note, `__qualname__` does not call `__mro__`, it just returns the root class trace. 
```python
class KlassA:
    pass

class KlassB:
    class KlassC:
        pass

print(KlassA.__qualname__) 
print(KlassB.__qualname__)
print(KlassB.KlassC.__qualname__)
```
```python
>>> KlassA
>>> KlassB
>>> KlassB.KlassC
```

**\__subclasses__**
Returns subclasses for each object in format `[<class '__main__.Klass4'>]`. The is `__repr__` not accessed unless overwritten. 

**\__init_subclass__**
Special method when used on a base class receives its subclasses as objects to operate on. `__init_subclass__` receives its subclass objects after they are created as those objects must exist in memory to be operated.  

```python
#example
class Base:
    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        print(f"New subclass created: {cls.__name__}")
        cls.category = "Child of Base"

class KlassA(Base):
    pass

class KlassB(Base):
    pass

print(KlassA.category)
```
```python
>>> New subclass created: KlassA
>>> New subclass created: KlassB
>>> Child of Base
```

**namespace**
A namespace is the internal C mapping of names to their objects. Python maintains several namespace: global namespace (names defined at the module level), local namespace (names defined within a function), class namespace (names defined in a class body), instance namespace (attributes belonging to an individual instance), built-in namespace (all the built-in functions, data structures, etc.). The namespace simply controls in what context names reference objects. 

When Python executes a class definition, it creates a temporary namespace dictionary to collect the attributes and methods defined inside the class.

Once the class finishes executing, python passes that dictionary to the metaclass (`type`), which creates the actual class object. The metaclass then creates the class and copies the namespace entries into its internal dictionary. 

The class dictionary is a`mappingproxy`, which is a is a read-only view of the its internal namespace dictionary (it is not a separate copy of the dictionary $\rightarrow$ it provides direct access to the original dictionary without allowing direct modification). However, an class instance will return a mutable python dictionary. Python protects classes from direct dictionary modification whereas instances do not generally require this restriction.

**\__main__**
The `__main__` object is an actual python module object, and an instance of the built-in module class `types.ModuleType`. When Python executes a script, it needs a module object to serve as the namespace in which the script code executes. 

How this works is python creates a module object (`types.ModuleType`) to hold the script global variables, functions, and classes and then sets the module `__name__` attribute to `'__main__'`, identifying it as the program's entry point (the entry point is where the python program begins running). 

Finally, python stores the module object in its module cache under the key `'__main__'`, allowing it to be accessed using `sys.modules["__main__"]`.

The `__main__` term does not exist in its own global namespace $\rightarrow$ it does not exist in the currently opened module. Every module has a set of special attributes native to the module itself: `['__annotations__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__']`).

The statement, `if __name__ == '__main__'`  enacts automatic execution of the `__main__` module $\rightarrow$ the currently open (active) execution module. 

```python
#example
import sys

print(__name__)
print(sys.modules["__main__"] is sys.modules[__name__])

if __name__ == "__main__":
    print("Executed directly")
```

**import time vs. runtime time**
Import time is the time that after python executes the import statement. Runtime is the time during the execution of your program. Top level code is all the code that is not in a function, not in a method, and not in a if `__name__ = '__main__'` statement $\rightarrow$ includes: module-body statements, class bodies, imports, assignments, expressions, top-level loops, top-level conditionals, top-level try/except blocks, decorator execution, function object creation, class creation, metaclass protocol + set_name, main block.

**import time**
When you `import`, python executes a certain set of actions, which are considered occurring at import time. The execution sequence, called the execution model, executes the top-level code from an imported module first. 

The code from the `__main__` module is also executed according to the execution model albeit after the `import`. Although functions and methods are not considered top-level code, a function call is (`func(a)`) will occur at import time if stated globally in a module. Therefore, although functions are not import time, they can occur at import time. 

**runtime**
Runtime executes all the code in the `__main__` module according to pythons execution module. The execution model is covered in depth below after other concepts are clarified.

---
**metaclasses, type, \__new__**
A metaclass is a class that creates a type of class. In python, all classes are instances of a metaclass, and all instances of are instances of a class. 

The default metaclass is `type`, which is an instance of itself (`type`) subclasses `object`. Consequently, any metaclass you create is type `type` and a subclass of `object`. Instances are distinct from classes, as they are instances of classes (and subclasses of `object`). 

To create a metaclass object you inherit `type` $\rightarrow$ i.e. `MetaClass(type)`. A metaclass object is of type `type` and used to create new classes, of `type(cls) = MetaClass` $\rightarrow$ in other words, a metaclass creates classes $\rightarrow$ classes create instances. 

$$
\begin{gathered}
\boxed{\text{type}}\\
\Downarrow\\
\boxed{\text{metaclass}}\\
\Downarrow\\
\boxed{\text{class}}\\
\downarrow\\
\boxed{\text{instance}}
\end{gathered}
$$

Effectively, `__new__` is the special method responsible for creating and returning an object, which may be an instance or a class object. It generally executes before `__init__` where `__init__` applies. Importantly, defining `__new__` in a metaclass does not apply to the `type` constructor but defines the `__new__` implementation for the instantiated class. 

To create class of type metaclass you must specify `metaclass=MetaClass` parameter in the class constructor. 

In the example below, assigning `cls = type.__new__()` is syntactically equal to `class cls(metaclass=Meta): pass`. Generally, `type.__new__` is the default constructor in python unless you use a different metaclass (see signature  of `__new__` below). The execution flow is something like the example below. 

```python
#example
class Meta(type):
    pass

cls = type.__new__(Meta, "MyClass", (), {})

print(type(cls))
print(type(type(cls)))
print(type(type(type(cls))))
print(issubclass(type(cls), object))
```
```python
>>> <class '__main__.Meta'>
>>> <class 'type'>
>>> <class 'type'>
>>> True
```

The `__new__` statement is responsible for creating an object 
Below are examples of when metaclasses might be used. For most application, metaclasses are not needed. 

| application           | example                                      | purpose                                                                     |
| --------------------- | -------------------------------------------- | --------------------------------------------------------------------------- |
| database orm          | django models, sqlalchemy declarative models | interpret class attributes as database columns and construct model metadata |
| abstract base classes | `abc.ABCMeta`                                | prevent instantiation when abstract methods remain unimplemented            |
| validation frameworks | custom model systems                         | validate class definitions when they are created                            |
| plugin systems        | automatic class registries                   | register subclasses automatically when defined                              |
| api frameworks        | declarative schemas                          | collect and process field definitions                                       |
**type()**
Refers to the function object, `type()` not the `type` object. Type can be used as a function to return the type of an object (`type(obj)`$\rightarrow$ `<class 'type'>`). 

```python
#signature
type(name, bases, namespace, /, **kwds) -> type #class construction mode
```

**\__new__**
This special method is used to operate on a class or instance before it is received. Each respective version is shown below. Note, the `type.__new__` constructor cannot be modified or customized $\rightarrow$ exists only for creating new metaclasses. 

```python
#signature
type.__new__(meta_cls, name, bases, namespace, /, **kwargs) -> cls
```
- `metacls`: this is the functional equivalent of `cls` but for metaclasses
- `name` : is the name of the class created by the metaclass i.e. `class Klass(metaclass=Meta)`
- `bases` : is a tuple of the the class being created i.e. the `Klass.__mro__`

```python
#signature
object.__new__(cls, *args, **kwargs)
```

- `cls` : the class whose instance is being created. Unlike `self`, which references an existing instance, `cls` references the class itself
- `*args` : accepts arbitrary positional arguments
- `**kwargs` : accepts arbitrary keyword arguments

**object.__new__() with __slots__**
Implementing `object.__new__()` with `__slots__ = ('a', 'b', 'c',...)` is a common design pattern as it allows the `__slots__` attributes defined per-instance while restricting an `__init__` which make the given class an immutable that cant change with added memory benefits. 

**\__prepare__**
Metaclass method responsible for preparing the namespace (usually a dictionary) in which a new class's attributes and methods will be defined. Returns a mapping into which the namespace arguments are populated and later copied into the class `__dict__`. Unlike new, which creates an object, `__prepare__` establishes the namespace before the class body executes. 
```python
#signature
__prepare__(mcs, name, bases, **kwargs)
```
- `mcs` : the metaclass responsible for creating the class (equivalent to `cls` in a class method).
- `name` : the name of the class being created, as a string.
- `bases` : a tuple containing the class's base classes.
- `**kwargs` : additional keyword arguments supplied in the class definition.

**execution model** 
The execution model can be split into two scopes: module execution model and class creation model. 

**module execution model** 
The module execution model is as follows: Execute top-level statements sequentially in source order:
$$
\begin{gathered}
\boxed{\text{imports}}\\
\Downarrow\\
\boxed{\text{assignments}}\\
\Downarrow\\
\boxed{\text{expressions (including print)}}\\

\end{gathered}
$$
When a class statement is encountered the following execution occurs in order:
$$
\begin{gathered}
\boxed{\text{run class creation protocol}}\\
\Downarrow\\
\boxed{\text{assignments}}\\
\Downarrow\\
\boxed{\text{expressions (including print)}}\\
\Downarrow\\
\end{gathered}
$$
Python executes top-level statements sequentially in source order, processing imports, assignments, and expressions (including `print`) as they are encountered. When a `class` statement is reached, Python invokes the class creation protocol, immediately executes the class body in its prepared namespace, and binds the resulting class object to the specified name.

**class creation model** 
$$
\begin{gathered}
\boxed{\text{parse and evaluate class header}}\\
\Downarrow\\
\boxed{\text{call metaclass.\_\_prepare\_\_}}\\
\Downarrow\\
\boxed{\text{execute class body in namespace}}\\
\Downarrow\\
\boxed{\text{call Descriptor.\_\_init\_\_ (during body execution)}}\\
\Downarrow\\
\boxed{\text{call metaclass.\_\_new\_\_}}\\
\Downarrow\\
\boxed{\text{call Descriptor.\_\_set\_name\_\_}}\\
\Downarrow\\
\boxed{\text{call base.\_\_init\_subclass\_\_}}\\
\Downarrow\\
\boxed{\text{call metaclass.\_\_init\_\_}}\\
\Downarrow\\
\boxed{\text{apply class decorators}}\\
\Downarrow\\
\boxed{\text{bind final class object to name}}
\end{gathered}
$$

Python begins class creation by parsing and evaluating the class header, including its base classes, metaclass, and keyword arguments. It then calls `metaclass.__prepare__(name, bases, **kwargs)` to obtain the namespace in which the class body will execute. By default, `__prepare__` returns a standard dictionary `{}` $\rightarrow$ python executes the class body sequentially, populating this namespace through `__setitem__` as assignments occur. If descriptors are instantiated during execution, their `__init__` methods are called at that point.

Once the class body finishes executing, python calls `metaclass.__new__(meta, name, bases, namespace, **kwargs)` to construct the class object. During the default `type.__new__` process, python invokes `Descriptor.__set_name__(cls, attr_name)` for applicable descriptors, followed by the parent class's `__init_subclass__(**kwargs)` hook. After `__new__` returns, python calls `metaclass.__init__(cls, name, bases, namespace, **kwargs)` to initialize the newly created class. Finally, any class decorators are applied sequentially, and the resulting object is bound to the class name.

---
# VI: Appendix

---
## object methods 
**object**
[`__new__`, `__repr__`, `__hash__`, `__str__`, `__getattribute__`, `__setattr__`, `__delattr__`, `__lt__`, `__le__`, `__eq__`, `__ne__`, `__gt__`, `__ge__`, `__init__`, `__reduce_ex__`, `__reduce__`, `__getstate__`, `__subclasshook__`, `__init_subclass__`, `__format__`, `__sizeof__`, `__dir__`, `__class__`, `__doc__`]

**list**
[`__new__`, `__repr__`, `__hash__`, `__getattribute__`, `__lt__`, `__le__`, `__eq__`, `__ne__`, `__gt__`, `__ge__`, `__iter__`, `__init__`, `__len__`, `__getitem__`, `__setitem__`, `__delitem__`, `__add__`, `__mul__`, `__rmul__`, `__contains__`, `__iadd__`, `__imul__`, `__reversed__`, `__sizeof__`, `clear`, `copy`, `append`, `insert`, `extend`, `pop`, `remove`, `index`, `count`, `reverse`, `sort`, `__class_getitem__`, `__doc__`]

**tuple**
[`__new__`, `__repr__`, `__hash__`, `__getattribute__`, `__lt__`, `__le__`, `__eq__`, `__ne__`, `__gt__`, `__ge__`, `__iter__`, `__len__`, `__getitem__`, `__add__`, `__mul__`, `__rmul__`, `__contains__`, `__getnewargs__`, `index`, `count`, `__class_getitem__`, `__doc__`]

**dict**
[`__new__`, `__repr__`, `__hash__`, `__getattribute__`, `__lt__`, `__le__`, `__eq__`, `__ne__`, `__gt__`, `__ge__`, `__iter__`, `__init__`, `__or__`, `__ror__`, `__ior__`, `__len__`, `__getitem__`, `__setitem__`, `__delitem__`, `__contains__`, `__sizeof__`, `get`, `setdefault`, `pop`, `popitem`, `keys`, `items`, `values`, `update`, `fromkeys`, `clear`, `copy`, `__reversed__`, `__class_getitem__`, `__doc__`]

**array.array**
[`__new__`, `__repr__`, `__getattribute__`, `__lt__`, `__le__`, `__eq__`, `__ne__`, `__gt__`, `__ge__`, `__iter__`, `__buffer__`, `__release_buffer__`, `__len__`, `__getitem__`, `__setitem__`, `__delitem__`, `__add__`, `__mul__`, `__rmul__`, `__contains__`, `__iadd__`, `__imul__`, `append`, `buffer_info`, `byteswap`, `__copy__`, `count`, `__deepcopy__`, `extend`, `fromfile`, `fromlist`, `frombytes`, `fromunicode`, `index`, `insert`, `pop`, `__reduce_ex__`, `remove`, `reverse`, `tofile`, `tolist`, `tobytes`, `tounicode`, `__sizeof__`, `__class_getitem__`, `typecode`, `itemsize`, `__doc__`, `__hash__`, `__module__`]

---
## built-in functions

| Built-in Function | What it does                            |
| ----------------- | --------------------------------------- |
| `abs()`           | Returns absolute value.                 |
| `aiter()`         | Returns an async iterator.              |
| `all()`           | Checks if all values are truthy.        |
| `anext()`         | Gets next async item.                   |
| `any()`           | Checks if any value is truthy.          |
| `ascii()`         | Returns ASCII-safe representation.      |
| `bin()`           | Converts integer to binary string.      |
| `bool()`          | Converts value to Boolean.              |
| `breakpoint()`    | Starts the debugger.                    |
| `bytearray()`     | Creates mutable bytes.                  |
| `bytes()`         | Creates immutable bytes.                |
| `callable()`      | Checks if object is callable.           |
| `chr()`           | Converts code point to character.       |
| `classmethod()`   | Creates a class method.                 |
| `compile()`       | Compiles source code.                   |
| `complex()`       | Creates a complex number.               |
| `delattr()`       | Deletes an attribute.                   |
| `dict()`          | Creates a dictionary.                   |
| `dir()`           | Lists object attributes.                |
| `divmod()`        | Returns quotient and remainder.         |
| `enumerate()`     | Adds indexes to an iterable.            |
| `eval()`          | Evaluates an expression.                |
| `exec()`          | Executes Python code.                   |
| `filter()`        | Filters an iterable.                    |
| `float()`         | Converts value to float.                |
| `format()`        | Formats a value.                        |
| `frozenset()`     | Creates an immutable set.               |
| `getattr()`       | Gets an attribute.                      |
| `globals()`       | Returns global namespace.               |
| `hasattr()`       | Checks for an attribute.                |
| `hash()`          | Returns hash value.                     |
| `help()`          | Displays documentation.                 |
| `hex()`           | Converts integer to hexadecimal string. |
| `id()`            | Returns object identity.                |
| `input()`         | Reads user input.                       |
| `int()`           | Converts value to integer.              |
| `isinstance()`    | Checks an object's type.                |
| `issubclass()`    | Checks subclass relationship.           |
| `iter()`          | Returns an iterator.                    |
| `len()`           | Returns number of items.                |
| `list()`          | Creates a list.                         |
| `locals()`        | Returns local namespace.                |
| `map()`           | Applies a function to items.            |
| `max()`           | Returns largest item.                   |
| `memoryview()`    | Creates a memory view.                  |
| `min()`           | Returns smallest item.                  |
| `next()`          | Gets next iterator item.                |
| `object()`        | Creates a base object.                  |
| `oct()`           | Converts integer to octal string.       |
| `open()`          | Opens a file.                           |
| `ord()`           | Returns character code point.           |
| `pow()`           | Raises value to a power.                |
| `print()`         | Prints output.                          |
| `property()`      | Creates a managed attribute.            |
| `range()`         | Creates an integer sequence.            |
| `repr()`          | Returns developer representation.       |
| `reversed()`      | Iterates in reverse.                    |
| `round()`         | Rounds a number.                        |
| `set()`           | Creates a set.                          |
| `setattr()`       | Sets an attribute.                      |
| `slice()`         | Creates a slice object.                 |
| `sorted()`        | Returns a sorted list.                  |
| `staticmethod()`  | Creates a static method.                |
| `str()`           | Converts value to string.               |
| `sum()`           | Adds iterable values.                   |
| `super()`         | Accesses parent-class behavior.         |
| `tuple()`         | Creates a tuple.                        |
| `type()`          | Returns or creates a type.              |
| `vars()`          | Returns an object's attributes.         |
| `zip()`           | Combines iterables by position.         |
| `__import__()`    | Imports a module.                       |

