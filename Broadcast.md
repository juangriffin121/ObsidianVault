The term broadcasting describes how NumPy treats arrays with different shapes during arithmetic operations. Subject to certain constraints, the smaller array is “broadcast” across the larger array so that they have compatible shapes. Broadcasting provides a means of vectorizing array operations so that looping occurs in C instead of Python. It does this without making needless copies of data and usually leads to efficient algorithm implementations. There are, however, cases where broadcasting is a bad idea because it leads to inefficient use of memory that slows computation.

```python
x.shape: (100,)
5.shape: ()
y = 5*x
y.shape: (100,)
```

```python
M.shape: (5,3)
v.shape: (5,1)
N = M+v
N.shape: (5,3)
```

