#Numpy

## 创建np.array的方法

### np.zeros
10个0

```
np.zeros(10)
```

### np.ones
3行5列共15个1

```
np.ones(shape=(3,5))
```

###np.full
4行3列共12个666

```
np.full(shape=(4,3),fill_value=666,dtypes=float)
```

**numpy.full (shape, fill_value, dtype=None, order='C')**

Return a new array of given shape and type, filled with fill_value.

Parameters:	

- shape : int or sequence of ints
- Shape of the new array, e.g., (2, 3) or 2.
- fill_value : scalar Fill value.
- dtype : data-type, optional The desired data-type for the array The default, None, means np.array(fill_value).dtype.

### np.linspace
**numpy.linspace(start, stop, num=50, endpoint=True,retstep=False, dtype=None, axis=0)**

Return evenly spaced numbers over a specified interval.

Returns num evenly spaced samples, calculated over the interval [start, stop].

The endpoint of the interval can optionally be excluded.

Parameters:

- start : array_like,The starting value of the sequence.
- stop : array_like,The end value of the sequence, unless endpoint is set to False. In that case, the sequence consists of all but the last of num + 1 evenly spaced samples, so that stop is excluded. Note that the step size changes when endpoint is False.
- num : int, optional,Number of samples to generate. Default is 50. Must be non-negative.
- endpoint : bool, optional,If True, stop is the last sample. Otherwise, it is not included. Default is True.
- retstep : bool, optional. If True, return (samples, step), where step is the spacing between samples.
- dtype : dtype, optional.The type of the output array. If dtype is not given, infer the data type from the other input arguments.

```
>>> np.linspace(2.0, 3.0, num=5)
array([2.  , 2.25, 2.5 , 2.75, 3.  ])
>>> np.linspace(2.0, 3.0, num=5, endpoint=False)
array([2. ,  2.2,  2.4,  2.6,  2.8])
>>> np.linspace(2.0, 3.0, num=5, retstep=True)
(array([2.  ,  2.25,  2.5 ,  2.75,  3.  ]), 0.25)
```

### np.arange()
**numpy.arange([start, ]stop, [step, ]dtype=None)**

Return evenly spaced values within a given interval.

Values are generated within the half-open interval [start, stop) (in other words, the interval including start but excluding stop). For integer arguments the function is equivalent to the Python built-in range function, but returns an ndarray rather than a list.

When using a non-integer step, such as 0.1, the results will often not be consistent. It is better to use numpy.linspace for these cases.

```
>>> np.arange(3)
array([0, 1, 2])
>>> np.arange(3.0)
array([ 0.,  1.,  2.])
>>> np.arange(3,7)
array([3, 4, 5, 6])
>>> np.arange(3,7,2)
array([3, 5])
```

## Random sampling (numpy.random)

[link to np.random](https://www.numpy.org/devdocs/reference/routines.random.html?highlight=random#module-numpy.random)
