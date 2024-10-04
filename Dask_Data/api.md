# API

Dask APIs generally follow from upstream APIs:

-   The `Dask Array API <array-api>`{.interpreted-text role="doc"}
    follows the NumPy API
-   The `Dask DataFrame API <dataframe-api>`{.interpreted-text
    role="doc"} follows the Pandas API
-   The [Dask-ML API](https://ml.dask.org/modules/api.html) follows the
    Scikit-Learn API and other related machine learning libraries
-   The `Dask Bag API <bag-api>`{.interpreted-text role="doc"} follows
    the map/filter/groupby/reduce API common in PySpark, PyToolz, and
    the Python standard library
-   The `Dask Delayed API <delayed-api>`{.interpreted-text role="doc"}
    wraps general Python code
-   The `Real-time Futures API <futures>`{.interpreted-text role="doc"}
    follows the
    [concurrent.futures](https://docs.python.org/3/library/concurrent.futures.html)
    API from the standard library.

Additionally, Dask has its own functions to start computations, persist
data in memory, check progress, and so forth that complement the APIs
above. These more general Dask functions are described below:

::: currentmodule
dask
:::

::: autosummary
compute is_dask_collection optimize persist visualize
:::

These functions work with any scheduler. More advanced operations are
available when using the newer scheduler and starting a
`dask.distributed.Client`{.interpreted-text role="obj"} (which, despite
its name, runs nicely on a single machine). This API provides the
ability to submit, cancel, and track work asynchronously, and includes
many functions for complex inter-task workflows. These are not necessary
for normal operation, but can be useful for real-time or advanced
operation.

This more advanced API is available in the [Dask distributed
documentation](https://distributed.dask.org/en/latest/api.html)

::: autofunction
compute
:::

::: autofunction
is_dask_collection
:::

::: autofunction
optimize
:::

::: autofunction
persist
:::

::: autofunction
visualize
:::
