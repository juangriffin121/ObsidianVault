Inspired from [[Einstein Summation Notation]] einsum is a function in [[tensorflow]] numpy and pytorch.

$w_i = \sum \limits_j M_{ij}v_j$
For a matrix vector multiplication
```python
M.shape: (4,3)
v.shape: (3,)
w = np.einsum('ij,j->i',M,v)
w.shape: (4,)
```

$K_{ik} = \sum \limits_j M_{ij}N_{jk}$
For a matrix matrix multiplication
```python
M.shape: (4,3)
N.shape: (3,5)
K = np.einsum('ij,jk->ik',M,N)
K.shape: (4,)
```

$W_{ji} = M_{ij}$
For matrix transpose
```python
M.shape: (4,3)
W = np.einsum('ij->ji',M)
W.shape: (3,4)
```

$R_{nij} = \sum \limits_j T_{nij}S_{njk}$
Batch matrix multiplication
```python
T.shape: (10,4,3)
S.shape: (10,3,5)
R = np.einsum('nij,njk->nik',T,S)
R.shape: () 
```

For bigger tensors we can use elipses (...) to write code that is agnostic to the number of indices in the input use an ellipsis. The ellipsis is a placeholder for "whatever other indices fit here".
```python
s.shape: (11, 7, 5, 3)
t.shape: (11, 7, 3, 2)
e =  tf.einsum('...ij,...jk->...ik', s, t)
e.shape: (11, 7, 5, 2)
```

Einsum **will** [[Broadcast]] over axes covered by the ellipsis.
```python
s.shape: (11, 1, 5, 3)
t.shape: (1, 7, 3, 2)
e =  tf.einsum('...ij,...jk->...ik', s, t)
e.shape: (11, 7, 5, 2)
```

$M_{il} = \sum \limits_j \sum \limits_k T_{ijk}S_{jkl}$
Multiple indices can be summed over
```python
T.shape: (4,3,6)
S.shape: (3,6,7)
M = np.einsum('ijk,jkl->il',T,S)
M.shape: (4,7) 
```
