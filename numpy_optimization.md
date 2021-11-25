### Optimizing Numpy
#datascience 
```
pip install numba
```

Enables faster #numpy computation by compiling code prior to execution ( #python ).
Basic usage is:

```python
@njit
def func_that_uses_numpy():
	results = 0
	for i in range(1000):
		results += np.exp(i)
	return results
```

@njit is the basic decorator that optimizes functions that work with numpy and scipy. Special functions from scipy require some overload functions. 
TODO: agregar ejemplo y librerias de la notebook lossFunction_plot.

@njit(parallel=True) allows to parallelize between cores the calculations. It requires calling prange() function from numba. Haven't actually used it.

<iframe src="https://www.youtube.com/embed/x58W9A2lnQc"></iframe>

