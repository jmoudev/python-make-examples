# foobar

Basic **make** examples using the Python print statement.

## foo

Make .PHONY target foo, printing foo:

```bash
$ make foo
foo
```

## bar

Make .PHONY target bar and therefore foo, printing foo then bar:

```bash
$ make bar
foo
bar
```
