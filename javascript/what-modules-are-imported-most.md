To figure out all Javascript dependencies in a codebase, use `dependency-cruiser` as follows:

```sh
depcruise --output-type metrics --exclude "^node_modules" src/ > deps-metrics.txt
```

This will create a table with the number of "Afferent couplings" and "Efferent couplings":

- Afferent couplings (Ca) The number of modules outside this folder that depend on this folder ("coming in")
- Efferent couplings (Ce) The number of modules this folder depends on outside the current folder ("going out")

See here for more https://github.com/sverweij/dependency-cruiser/blob/HEAD/doc/cli.md#metrics---generate-a-report-with-stability-metrics-for-each-folder

It seems that "afferent" and "efferent" actually refer to modules outside of this **file**, so you can use this to see the number of incoming and outgoing imports for any Javascript module. This is very useful to figure out what modules are depended on the most to determine where to add more tests.