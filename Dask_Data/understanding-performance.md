# Understanding Performance

The first step in making computations run quickly is to understand the
costs involved. In Python we often rely on tools like the [CProfile
module](https://docs.python.org/3/library/profile.html), [%%prun IPython
magic](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-prun),
[VMProf](https://vmprof.readthedocs.io/en/latest/), or
[snakeviz](https://jiffyclub.github.io/snakeviz/) to understand the
costs associated with our code. However, few of these tools work well on
multi-threaded or multi-process code, and fewer still on computations
distributed among many machines. We also have new costs like data
transfer, serialization, task scheduling overhead, and more that we may
not be accustomed to tracking.

Fortunately, the Dask schedulers come with diagnostics to help you
understand the performance characteristics of your computations. By
using these diagnostics and with some thought, we can often identify the
slow parts of troublesome computations.

The
`single-machine and distributed schedulers <scheduling>`{.interpreted-text
role="doc"} come with *different* diagnostic tools. These tools are
deeply integrated into each scheduler, so a tool designed for one will
not transfer over to the other.

These pages provide four options for profiling parallel code:

1.  `Visualize task graphs <graphviz>`{.interpreted-text role="doc"}
2.  `Single threaded scheduler and a normal Python profiler <single-threaded-scheduler>`{.interpreted-text
    role="ref"}
3.  `Diagnostics for the single-machine scheduler <diagnostics-local>`{.interpreted-text
    role="doc"}
4.  `Dask distributed dashboard <diagnostics-distributed>`{.interpreted-text
    role="doc"}
