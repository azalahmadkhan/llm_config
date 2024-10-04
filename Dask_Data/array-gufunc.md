# Generalized Ufuncs

EXPERIMENTAL FEATURE added to Version 0.18.0 and above - see
`disclaimer<disclaimer>`{.interpreted-text role="ref"}.

[NumPy](https://www.numpy.org) provides the concept of [generalized
ufuncs](https://docs.scipy.org/doc/numpy/reference/c-api.generalized-ufuncs.html).
Generalized ufuncs are functions that distinguish the various dimensions
of passed arrays in the two classes loop dimensions and core dimensions.
To accomplish this, a
[signature](https://docs.scipy.org/doc/numpy/reference/c-api.generalized-ufuncs.html#details-of-signature)
is specified for NumPy generalized ufuncs.

[Dask](https://dask.org/) integrates interoperability with NumPy\'s
generalized ufuncs by adhering to respective [ufunc
protocol](https://docs.scipy.org/doc/numpy/reference/arrays.classes.html#numpy.class.__array_ufunc__),
and provides a wrapper to make a Python function a generalized ufunc.

## Usage

### NumPy generalized ufunc

::: note
::: title
Note
:::

[NumPy](https://www.numpy.org) generalized ufuncs are currently (v1.14.3
and below) stored in inside `np.linalg._umath_linalg` and might change
in the future.
:::

``` python
import dask.array as da
import numpy as np

x = da.random.normal(size=(3, 10, 10), chunks=(2, 10, 10))

w, v = np.linalg._umath_linalg.eig(x, output_dtypes=(float, float))
```

### Wrap own Python function

`gufunc` can be used to make a Python function behave like a generalized
ufunc:

``` python
x = da.random.normal(size=(10, 5), chunks=(2, 5))

def foo(x):
    return np.mean(x, axis=-1)

gufoo = da.gufunc(foo, signature="(i)->()", output_dtypes=float, vectorize=True)

y = gufoo(x)
```

Instead of `gufunc`, also the `as_gufunc` decorator can be used for
convenience:

``` python
x = da.random.normal(size=(10, 5), chunks=(2, 5))

@da.as_gufunc(signature="(i)->()", output_dtypes=float, vectorize=True)
def gufoo(x):
    return np.mean(x, axis=-1)

y = gufoo(x)
```

## Disclaimer

This experimental generalized ufunc integration is not complete:

-   `gufunc` does not create a true generalized ufunc to be used with
    other input arrays besides Dask. I.e., at the moment, `gufunc` casts
    all input arguments to `dask.array.Array`
-   Inferring `output_dtypes` automatically is not implemented yet

## API

::: currentmodule
dask.array.gufunc
:::

::: autosummary
apply_gufunc as_gufunc gufunc
:::
