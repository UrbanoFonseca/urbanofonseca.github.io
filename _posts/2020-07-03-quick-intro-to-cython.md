---
layout: post
title: "Quick Intro to Cython"
author: "Francisco Fonseca"
categories: tutorial
tags: [tutorial]
image: horseshoe-1.jpg
---

# Quick Intro to Cython
>A tourist guide approach to learn Cython in a easy, understandable and practical way.

## Why
Python is a beautifully simple, newbie-friendly programming language. Nonetheless, such simplicity often comees with a cost: speed. Python is fast to write but slow to execute, specially if you go into heavier computation stuff, such as algorithms. To speed up, you can extend it with Cython. 

## What
**Cython** is a Python compiler that allows to write C extensions. It can compile normal Python code on top of static type declarations to speed up the execution.

## Example
We will write a meaningless loop function (iterate and sum for nothing in special really) to showcase the potential of statically type variables with C for known routines.

The first code file will be the Python standard `loop.py`:
```python
# loop.py, cloop.pyx
import time

def loop_and_add():
    t1 = time.time()
    j = 0
    for i in range(30_000_000):
        if i % 2 == 0 and i % 15 == 0:
            j += i
    t2 = time.time()
    return j, round(t2-t1, 4)
```
The first test will be to have the same code compiled with Cython. To do that, we duplicate the same file into an  `cloop.pyx` Cython file and create a `cloop_setup.py` that will handle the cynthonization.

```python
# cloop_setup.py
import distutils.core
import Cython.Build

distutils.core.setup(
    ext_modules = Cython.Build.cythonize("cloop.pyx")
)
```

After that, run the following in the terminal to generate the compiled code.

```bash
$ python cloop_setup.py build_ext --inplace
```

We now have two importable modules into our python, the original `loop` and the Cythonized `cloop`. Let's try those out.

```python
import loop
import cloop

for i in range(5):
    print(loop.loop_and_add())

#(14999985000000, 2.7513)
#(14999985000000, 2.6421)
#(14999985000000, 2.5611)
#(14999985000000, 2.5821)
#(14999985000000, 2.5908)

for i in range(5):
    print(cloop.loop_and_add())

#(14999985000000, 1.2493)
#(14999985000000, 1.3441)
#(14999985000000, 1.3525)
#(14999985000000, 1.3364)
#(14999985000000, 1.3344)    
```

As you can see, by running cython over the already existing Python code (even without any changes) speeds up this process to close to a half of the time. We can even go further and statically define the types of the variables within the cython `.pyx` file as a new optimized loop file `oloop.pyx`.

```python
# oloop.pyx
import time

def loop_and_add():
    # Declare the variable types.
    cdef int i, N
    cdef long j
    
    t1 = time.time()
    j = 0
    N = 30_000_000
    for i in range(N):
        if i % 2 == 0 and i % 15 == 0:
            j += i
    t2 = time.time()
    return j, round(t2-t1, 4)
```

We can test that using the same logic as before.

```python
import oloop

for i in range(5):
    print(oloop.loop_and_add())

#(14999985000000, 0.042)
#(14999985000000, 0.0424)
#(14999985000000, 0.0426)
#(14999985000000, 0.0433)
#(14999985000000, 0.0478)
```
The computation time is now reduced to approximately zero.

The full comparison is repeated now with 100M entries over 5 runs and we achieve the expected results: full Python is much slower, Cythonize raw Python code helps but statically typing the variables is much faster than before.

```python
import loop
import cloop
import oloop

for i in range(5):
    print(loop.loop_and_add())

#(166666683333330, 8.9182)
#(166666683333330, 9.5814)
#(166666683333330, 9.6526)
#(166666683333330, 9.3617)
#(166666683333330, 9.0275)


for i in range(5):
    print(cloop.loop_and_add())

#(166666683333330, 4.492)
#(166666683333330, 4.5549)
#(166666683333330, 4.4763)
#(166666683333330, 4.4895)
#(166666683333330, 4.4776)


for i in range(5):
    print(oloop.loop_and_add())

#(166666683333330, 0.1468)
#(166666683333330, 0.1436)
#(166666683333330, 0.1483)
#(166666683333330, 0.1488)
#(166666683333330, 0.1497)
```

## In a Nutshell
> Get the python code. Define the types and function calls inside C whenever possible. Setup the Cythonizer with distutils. Import the optimized module.
Additionally, Cython requires you to have C compiler _(protip: use VSCode + WSL on Windows)_.

## Tips and Tricks
### Performance Report
Cython has a nice, built-in functionality that allows to provide hints for code blocks where the execution will take longer based on code execution calls. To test this, you can simply run `cython -a mycythonfile.pyx` and check the generated html.

### Variable Types
Cython can use `ìnt`, `float`, `long`, `char`, `struct`, `bint`.

### Function Declarations
Functions defined with `def` are visible to other Python code. `cdef` are only visible to Cython/C and execute much faster, so use them if those functions are only called within the Cython module. `cpdef` allows for compatibility with both Python and C, in a way that C can access the declared function at full speed but with some overhead (more code generated and more call overhead).