# Computation and scaling

Draft of computation and scaling on multicore processors and related issues/tooling.

- [Pandas](https://pandas.pydata.org/) - fast, powerful, flexible and easy to use open source data analysis and manipulation tool
  - [Modern Pandas - Part 1](https://tomaugspurger.github.io/modern-1-intro) - Intro to modern Pandas
- [NumPy](https://numpy.org/) - fundamental package for scientific computing with Python
- [SciPy](https://scipy.org/) - ecosystem of open-source software for mathematics, science, and engineering
- [PyMC](https://docs.pymc.io/) - probabilistic programming library for Python
- [Swifter](https://github.com/jmcarpenter2/swifter) - efficiently applies any function to a pandas dataframe or series in the fastest available manner
- [Dask - Scalable analytics in Python](https://docs.dask.org/en/latest/install.html) - open source library for parallel computing

## Biology / Genomics

- [PyRanges](https://github.com/biocore-ntnu/pyranges) - GenomicRanges and genomic Rle-objects for Python.
- [Epic2](https://github.com/biocore-ntnu/epic2) - epic2 is an ultraperformant reimplementation of SICER. It focuses on speed, low memory overhead and ease of use.

## On avoiding oversubscription
- [Thread-pool Controls](https://github.com/joblib/threadpoolctl) - Python helpers to limit the number of threads used in native libraries that handle their own internal threadpool (BLAS and OpenMP implementations)
- [Multiple OpenMP runtimes](https://github.com/joblib/threadpoolctl/blob/master/multiple_openmp.md) - Issues having multiple OpenMP runtimes and proposed approaches
- [Sharedmem](https://github.com/rainwoodman/sharedmem) ([documentation](https://rainwoodman.github.io/sharedmem/)) - Easier parallel programming on shared memory computers 
- [Static Multi-Processing](https://github.com/IntelPython/smp) - SMP module allows to set static affinity mask for each process inside process pool to limit total number of threads running in application.

Most of the tools for effective computation already come with a lot of optimizations for multicore systems. When optimizing for performance we have to take previous optimizations in consideration to avoid opposite effects. In the case of oversubcription for example when running multitude of python processes we may consider limiting number of available cores for optimization layer (specific library - e.g. blas).

```
from threadpoolctl import threadpool_info, threadpool_limits
from pprint import pprint
import numpy as np

pprint(threadpool_info())

with threadpool_limits(limits=1, user_api='blas'):
    # In this block, calls to blas implementation (like openblas or MKL)
    # will be limited to use only one thread. They can thus be used jointly
    # with thread-parallelism.
    a = np.random.randn(1000, 1000)
    a_squared = a @ a
```

Limiting threads for common optimization libraries:

```
try:
    # Don't place numpy import before this block
    import os
    os.environ["OMP_NUM_THREADS"] = "1"  # OMP_NUM_THREADS: openmp
    os.environ["OPENBLAS_NUM_THREADS"] = "1"  # OPENBLAS_NUM_THREADS: openblas
    os.environ["MKL_THREADING_LAYER"] = "sequential"  # MKL_THREADING_LAYER: mkl (instead of MKL_THREADING_LAYER=1)
    os.environ["VECLIB_MAXIMUM_THREADS"] = "1"  # VECLIB_MAXIMUM_THREADS: accelerate
    os.environ["NUMEXPR_NUM_THREADS"] = "1"  # NUMEXPR_NUM_THREADS: numexpr
    os.environ["MXNET_CPU_WORKER_NTHREADS"] = "1"  # MXNET_CPU_WORKER_NTHREADS: mxnet
except Exception:
    pass
import numpy as np

print(np.show_config())
```


```
try:
    import mkl
    mkl.set_num_threads(1)
except:
    pass
```

## Resources
- [Understanding parallel performance](https://www.drdobbs.com/parallel/understanding-parallel-performance/211800538)
- [MKL Developer guide for calling Intel MKL functions](https://software.intel.com/en-us/mkl-linux-developer-guide-calling-intel-mkl-functions-from-multi-threaded-applications)
- [SciPy - Guide to parallel programming in Python](https://scipy-cookbook.readthedocs.io/items/ParallelProgramming.html)
- [Speed-up numpy with Intel's Math Kernel Library (MKL) (2019.10)](https://jwalton.info/Python-MKL-openBLAS/)
- [Boosting Numpy with BLAS](https://markus-beuckelmann.de/blog/boosting-numpy-blas.html)
