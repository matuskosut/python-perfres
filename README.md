# python-perfres (Performance profiling resources for Python)

Where to start and what not to miss when interested in profiling python apps. Target is to manage profiling web apps based on python frameworks (e.g. flask) right in production, but also many use-cases of basic python profiling methods are listed in order to better understand the possible drawbacks of tracing production apps.

## Basic python profiling tools:

- [line_profiler](https://github.com/rkern/line_profiler) - Line-by-line profiling.
- [memory_profiler](https://github.com/fabianp/memory_profiler) - Monitor Memory usage of Python code.
- [profiling](https://github.com/what-studio/profiling) - An interactive Python profiler.
- [vprof](https://github.com/nvdv/vprof) - Visual Python profiler.

## Memory leaks use-cases:
- [Tracing python memory leaks](http://tech.labs.oliverwyman.com/blog/2008/11/14/tracing-python-memory-leaks/) #PDB
- [Hunting memory leaks in Python](http://mg.pov.lt/blog/hunting-python-memleaks.html) #PDB


## Production profiling use-cases and tools:
- [nylas-perftools](https://github.com/nylas/nylas-perftools "nylas-perftools") one of the best profiling tools I tried, with very little CPU overhead (they litteraly say its negligible). Read more about their use-case: https://www.nylas.com/blog/performance/ #Flask

