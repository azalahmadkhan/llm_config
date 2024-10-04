# Dask

*Dask is a flexible library for parallel computing in Python.*

Dask is composed of two parts:

1.  **Dynamic task scheduling** optimized for computation. This is
    similar to *Airflow, Luigi, Celery, or Make*, but optimized for
    interactive computational workloads.
2.  **\"Big Data\" collections** like parallel arrays, dataframes, and
    lists that extend common interfaces like *NumPy, Pandas, or Python
    iterators* to larger-than-memory or distributed environments. These
    parallel collections run on top of dynamic task schedulers.

Dask emphasizes the following virtues:

-   **Familiar**: Provides parallelized NumPy array and Pandas DataFrame
    objects
-   **Flexible**: Provides a task scheduling interface for more custom
    workloads and integration with other projects.
-   **Native**: Enables distributed computing in pure Python with access
    to the PyData stack.
-   **Fast**: Operates with low overhead, low latency, and minimal
    serialization necessary for fast numerical algorithms
-   **Scales up**: Runs resiliently on clusters with 1000s of cores
-   **Scales down**: Trivial to set up and run on a laptop in a single
    process
-   **Responsive**: Designed with interactive computing in mind, it
    provides rapid feedback and diagnostics to aid humans

![Dask collections and schedulers](images/collections-schedulers.png){.align-center
width="80.0%"}

See the [dask.distributed documentation (separate
website)](https://distributed.dask.org/en/latest/) for more technical
information on Dask\'s distributed scheduler.

## Familiar user interface

**Dask DataFrame** mimics Pandas -
`documentation <dataframe>`{.interpreted-text role="doc"}

``` python
import pandas as pd                     import dask.dataframe as dd
df = pd.read_csv('2015-01-01.csv')      df = dd.read_csv('2015-*-*.csv')
df.groupby(df.user_id).value.mean()     df.groupby(df.user_id).value.mean().compute()
```

**Dask Array** mimics NumPy - `documentation <array>`{.interpreted-text
role="doc"}

``` python
import numpy as np                       import dask.array as da
f = h5py.File('myfile.hdf5')             f = h5py.File('myfile.hdf5')
x = np.array(f['/small-data'])           x = da.from_array(f['/big-data'],
                                                           chunks=(1000, 1000))
x - x.mean(axis=1)                       x - x.mean(axis=1).compute()
```

**Dask Bag** mimics iterators, Toolz, and PySpark -
`documentation <bag>`{.interpreted-text role="doc"}

``` python
import dask.bag as db
b = db.read_text('2015-*-*.json.gz').map(json.loads)
b.pluck('name').frequencies().topk(10, lambda pair: pair[1]).compute()
```

**Dask Delayed** mimics for loops and wraps custom code -
`documentation <delayed>`{.interpreted-text role="doc"}

``` python
from dask import delayed
L = []
for fn in filenames:                  # Use for loops to build up computation
    data = delayed(load)(fn)          # Delay execution of function
    L.append(delayed(process)(data))  # Build connections between variables

result = delayed(summarize)(L)
result.compute()
```

The **concurrent.futures** interface provides general submission of
custom tasks: - `documentation <futures>`{.interpreted-text role="doc"}

``` python
from dask.distributed import Client
client = Client('scheduler:port')

futures = []
for fn in filenames:
    future = client.submit(load, fn)
    futures.append(future)

summary = client.submit(summarize, futures)
summary.result()
```

## Scales from laptops to clusters

Dask is convenient on a laptop. It
`installs <install>`{.interpreted-text role="doc"} trivially with
`conda` or `pip` and extends the size of convenient datasets from \"fits
in memory\" to \"fits on disk\".

Dask can scale to a cluster of 100s of machines. It is resilient,
elastic, data local, and low latency. For more information, see the
documentation about the [distributed
scheduler](https://distributed.dask.org/en/latest/).

This ease of transition between single-machine to moderate cluster
enables users to both start simple and grow when necessary.

## Complex Algorithms

Dask represents parallel computations with
`task graphs<graphs>`{.interpreted-text role="doc"}. These directed
acyclic graphs may have arbitrary structure, which enables both
developers and users the freedom to build sophisticated algorithms and
to handle messy situations not easily managed by the
`map/filter/groupby` paradigm common in most data engineering
frameworks.

We originally needed this complexity to build complex algorithms for
n-dimensional arrays but have found it to be equally valuable when
dealing with messy situations in everyday problems.

## Index

**Getting Started**

-   `install`{.interpreted-text role="doc"}
-   `setup`{.interpreted-text role="doc"}
-   `use-cases`{.interpreted-text role="doc"}
-   `support`{.interpreted-text role="doc"}
-   `why`{.interpreted-text role="doc"}

::: {.toctree maxdepth="1" hidden="" caption="Getting Started"}
install.rst setup.rst use-cases.rst support.rst why.rst
:::

**Collections**

Dask collections are the main interaction point for users. They look
like NumPy and Pandas but generate dask graphs internally. If you are a
dask *user* then you should start here.

-   `array`{.interpreted-text role="doc"}
-   `bag`{.interpreted-text role="doc"}
-   `dataframe`{.interpreted-text role="doc"}
-   `delayed`{.interpreted-text role="doc"}
-   `futures`{.interpreted-text role="doc"}

::: {.toctree maxdepth="1" hidden="" caption="User Interface"}
user-interfaces.rst array.rst bag.rst dataframe.rst delayed.rst
futures.rst Machine Learning \<<https://ml.dask.org>\> api.rst
:::

**Scheduling**

Schedulers execute task graphs. Dask currently has two main schedulers:
one for local processing using threads or processes; and one for
distributed memory clusters.

-   `scheduling`{.interpreted-text role="doc"}
-   `distributed`{.interpreted-text role="doc"}

::: {.toctree maxdepth="1" hidden="" caption="Scheduling"}
scheduling.rst distributed.rst
:::

**Diagnosing Performance**

Parallel code can be tricky to debug and profile. Dask provides several
tools to help make debugging and profiling graph execution easier.

-   `understanding-performance`{.interpreted-text role="doc"}
-   `graphviz`{.interpreted-text role="doc"}
-   `diagnostics-local`{.interpreted-text role="doc"}
-   `diagnostics-distributed`{.interpreted-text role="doc"}
-   `debugging`{.interpreted-text role="doc"}

::: {.toctree maxdepth="1" hidden="" caption="Diagnostics"}
understanding-performance.rst graphviz.rst diagnostics-local.rst
diagnostics-distributed.rst debugging.rst
:::

**Graph Internals**

Internally, Dask encodes algorithms in a simple format involving Python
dicts, tuples, and functions. This graph format can be used in isolation
from the dask collections. Working directly with dask graphs is rare,
unless you intend to develop new modules with Dask. Even then,
`dask.delayed <delayed>`{.interpreted-text role="doc"} is often a better
choice. If you are a *core developer*, then you should start here.

-   `graphs`{.interpreted-text role="doc"}
-   `spec`{.interpreted-text role="doc"}
-   `custom-graphs`{.interpreted-text role="doc"}
-   `optimize`{.interpreted-text role="doc"}
-   `custom-collections`{.interpreted-text role="doc"}
-   `high-level-graphs`{.interpreted-text role="doc"}

::: {.toctree maxdepth="1" hidden="" caption="Graphs"}
graphs.rst spec.rst custom-graphs.rst optimize.rst
custom-collections.rst high-level-graphs.rst
:::

**Help & reference**

-   `develop`{.interpreted-text role="doc"}
-   `changelog`{.interpreted-text role="doc"}
-   `configuration`{.interpreted-text role="doc"}
-   `presentations`{.interpreted-text role="doc"}
-   `cheatsheet`{.interpreted-text role="doc"}
-   `spark`{.interpreted-text role="doc"}
-   `caching`{.interpreted-text role="doc"}
-   `bytes`{.interpreted-text role="doc"}
-   `remote-data-services`{.interpreted-text role="doc"}
-   `cite`{.interpreted-text role="doc"}
-   `funding`{.interpreted-text role="doc"}
-   `logos`{.interpreted-text role="doc"}

::: {.toctree maxdepth="1" hidden="" caption="Help & reference"}
develop.rst changelog.rst configuration.rst presentations.rst
cheatsheet.rst spark.rst caching.rst bytes.rst remote-data-services.rst
cite.rst funding.rst logos.rst
:::

Dask is supported by [Anaconda Inc](https://www.anaconda.com) and
develops under the BSD 3-clause license.
