# Computation and scaling

Draft of computation and scaling on multicore processors and related issues/tooling.

## On avoiding oversubscription
- [Thread-pool Controls](https://github.com/joblib/threadpoolctl) - Python helpers to limit the number of threads used in native libraries that handle their own internal threadpool (BLAS and OpenMP implementations)
- [Multiple OpenMP runtimes](https://github.com/joblib/threadpoolctl/blob/master/multiple_openmp.md) - Issues having multiple OpenMP runtimes and proposed approaches
- [Sharedmem](https://github.com/rainwoodman/sharedmem) ([documentation](https://rainwoodman.github.io/sharedmem/)) - Easier parallel programming on shared memory computers 
- [Static Multi-Processing](https://github.com/IntelPython/smp) - SMP module allows to set static affinity mask for each process inside process pool to limit total number of threads running in application.

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
