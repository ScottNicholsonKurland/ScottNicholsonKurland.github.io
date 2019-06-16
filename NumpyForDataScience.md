
Data science is OCEMN. Numpy - Python's linear algebra library - can help with obtaining, cleaning, and exploring data, leaving modeling and interpretation to other libraries. Genfromtxt is more flexible and powerful than loadtxt, so we'll explore its use in obtaining data. Preliminaries: conda install numpy, or if you're a bad person, pip install numpy. Import numpy as np. There are several - many - functions to create numpy arrays to play with and and many more to play with them, but for the purposes of data science our first step is to obtain data. Playground functions, therefore, are in the appendix.

Consider Project Euler's tenth problem, summation of primes. In Project Euler itself it is easy to brute force it, but it is more nuanced in HackerRank, where there might be tens of thousands of test cases; speed and elegance are important.


The sum of the primes below N:

Find the sum of all the primes not greater than N. The first line contains the number of test cases T. The next T lines will contain Ns: https://www.hackerrank.com/contests/projecteuler/challenges/euler010/submissions/code/1311394025


```python
#vanilla Python

#s=Sieve of Eratosthenes

s = [False, True] * 500000

#i=iterator

for i in range (3,1000,2):
    if s[i]:
        for m in range (i**2,1000000,i*2):
            s[m]=False

#sop=list of sum of primes

sop=[0,0,2]
sum = 2
for i in range(3,1000000):
    if s[i]:
        sum+=i
    sop.append(sum)
```


```python
#NumPython




```


```python
# Print the number of `my_array`'s dimensions
print(my_array.ndim)

# Print the number of `my_array`'s elements
print(my_array.size)

# Print information about `my_array`'s memory layout
print(my_array.flags)

# Print the length of one array element in bytes
print(my_array.itemsize)

# Print the total consumed bytes by `my_array`'s elements
print(my_array.nbytes)
```


```python
import numpy as np
import sys

py_arr = [1,2,3,4,5,6]
numpy_arr = np.array([1,2,3,4,5,6])

sizeof_py_arr = sys.getsizeof(1) * len(py_arr)           # Size = 168
sizeof_numpy_arr = numpy_arr.itemsize * numpy_arr.size   # Size = 48

py_arr = [1.1, 1.2,1.3424242, 1.436654646546, 454353.1232131, 32434353.324324, 1234.2321]
numpy_arr = np.array([1.1, 1.2,1.3424242, 1.436654646546, 454353.1232131, 32434353.324324, 1234.2321])

sizeof_py_arr = sys.getsizeof(12345.232343) * len(py_arr)           # Size  = 196
sizeof_numpy_arr = numpy_arr.itemsize * numpy_arr.size   # Size = 56

```

The above examples, it turns out, are a whole string lies starting with the 8 byte cost of a 24 byte int is the pointer and the actual number and its memory cost is cleverly hidden elsewhere. In short, numpy is much smaller. Exactitude is a rabbit hole that sys.getsizeof() and its recursive big brother deep_getsizeof do not get to the bottom of; things like the list of small integers from -5 to 256 that python keeps as a matter of course; free, sort of. Fixed overhead, anyway. CPython manages small objects (less than 256 bytes) in special pools on 8-byte boundaries. There are pools for 1-8 bytes, 9-16 bytes... 249-256 bytes. When an object of size 9 is allocated, it is allocated from the 16-byte pool for objects 9-16 bytes in size. So, even though it contains only 9 bytes of data, it will cost 16 bytes of memory. If you allocate a million objects of size 9, you actually use 16 megabytes not 9. This overhead - 800% for 1 byte data - can hurt.


```python
import time
import numpy as np

size = 1000

def vanilla():
    t1 = time.time()
    X = range(size)
    Y = range(size)
    Z = [X[i] + Y[i] for i in range(len(X)) ]
    return time.time() - t1

def numpy_version():
    t1 = time.time()
    X = np.arange(size)
    Y = np.arange(size)
    Z = X + Y
    return time.time() - t1


t1 = vanilla()
t2 = numpy_version()
print(t1, t2)
print("Numpy is in this example " + str(t1/t2) + " faster!")
```
