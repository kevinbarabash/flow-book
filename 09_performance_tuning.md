# Chapter 9: Performance Tuning

Flow has some great tools for diagnosing performance issues.

## Logs

When flow starts up it prints the location of its log file. This log file
includes lots of different things including log messages for any files taking a
long time to merge.

## Profiling

`flow check --profile` outputs detailed profiling information including a list
of cycles in your code as well as timing data showing how long flow spent in
each phase.

In order reduce time spent in parsing, it is important to ignore as many paths
as possible. Adding `node_modules` to the `[ignore]` section of your .flowconfig
file and then using negation patterns to make sure only certain modules are
included can really help. The official docs don't mention negation patterns even
though support was added for them in
[6acd727](https://github.com/facebook/flow/commit/6acd727e4493af59d12e79b714de83b023321638).

```
[ignore]
<PROJECT_ROOT>/node_modules
!<PROJECT_ROOT>/node_modules/aphrodite
!<PROJECT_ROOT>/node_modules/react-dom
!<PROJECT_ROOT>/node_modules/redux
```

## Cycles

The cycles listed by `flow check --profile` can be visualized by running
`flow cycle --strip-root path/to/file/in/cycle.js`. This will output a directed
graph of the cycle in .dot format.
