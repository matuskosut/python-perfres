# Computation and scaling

Draft of computation and scaling on multicore processors and related issues/tooling.

## On avoiding oversubscription
- [Thread-pool Controls](https://github.com/joblib/threadpoolctl) - Python helpers to limit the number of threads used in native libraries that handle their own internal threadpool (BLAS and OpenMP implementations)
- [Sharedmem](https://github.com/rainwoodman/sharedmem) ([documentation](https://rainwoodman.github.io/sharedmem/)) - Easier parallel programming on shared memory computers 

```
from threadpoolctl import threadpool_info
from pprint import pprint
import numpy

pprint(threadpool_info())
```

Limiting threads for common optimization libraries:

```
try:
    # Don't place numpy import before this block
    import os
    os.environ["OMP_NUM_THREADS"] = "1"  # OMP_NUM_THREADS: openmp
    os.environ["OPENBLAS_NUM_THREADS"] = "1"  # OPENBLAS_NUM_THREADS: openblas
    os.environ["MKL_NUM_THREADS"] = "1"  # MKL_NUM_THREADS: mkl
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
