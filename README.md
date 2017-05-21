# Performance profiling resources for Python

Where to start and what not to miss when interested in profiling python apps. Target is to manage profiling web apps based on python frameworks (e.g. flask) right in production, but also many use-cases of basic python profiling methods are listed in order to better understand the possible drawbacks of tracing production apps.

## Basic python profiling tools:

- [line_profiler](https://github.com/rkern/line_profiler) - Line-by-line profiling.
- [memory_profiler](https://github.com/fabianp/memory_profiler) - Monitor Memory usage of Python code.
- [memprof](https://github.com/jmdana/memprof) - Memory profiler for Python
- [profiling](https://github.com/what-studio/profiling) - An interactive Python profiler.
- [vprof](https://github.com/nvdv/vprof) - Visual Python profiler.

## Performance analysis:
- [A guide to analyzing Python performance](https://www.huyng.com/posts/python-performance-analysis) - Very informative article written by SE at Flickr


## Memory leaks use-cases:
- [Tracing python memory leaks](http://tech.labs.oliverwyman.com/blog/2008/11/14/tracing-python-memory-leaks/) #PDB
- [Hunting memory leaks in Python](http://mg.pov.lt/blog/hunting-python-memleaks.html) #PDB
- [Python object graphs](http://mg.pov.lt/blog/python-object-graphs.html) #GC
- [Diagnosing Memory "Leaks" in Python](http://chase-seibert.github.io/blog/2013/08/03/diagnosing-memory-leaks-python.html) #Objgraph #Heapy #GDB

## Production profiling use-cases and tools:
- [nylas-perftools](https://github.com/nylas/nylas-perftools "nylas-perftools") - one of the best profiling toolsets for python that I tried so far, with very little CPU overhead (they litteraly say its negligible). It was actually designed for profiling in production, when its developers were in need to find a new ways to optimizate their wast Nylas platform and it also features [flame graphs](http://www.brendangregg.com/flamegraphs.html) introduced by Brendan Gregg, these are well known as a very good technique for visualisation of stack profiles. Read more about their use-case: https://www.nylas.com/blog/performance/ #Flask
- [Profiling a Werkzeug (flask) app](http://www.alexandrejoseph.com/blog/2015-12-17-profiling-werkzeug-flask-app.html) #Flask
- [Deploying and Monitoring Python Web Apps with uWSGI](https://www.engagespark.com/blog/deploying-monitoring-python-web-apps-uwsgi/) - there is a way to look at a current stack trace using very simple setting in uwsgi app config, and the affect to performance of the application itself seems to be minimal. This may be interesting for those running multiple workers (separate stats socket is used for every worker). Other debugging examples and tools are stated in the article too. #Tracebacker

