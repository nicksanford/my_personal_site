+++
title = "TIL - Makefile target prerequisites are concurrent"
date = 2023-01-29
[taxonomies]
+++
Today I learned that when make evaluates Makefile target prerequisites, it assumes that the order of dependency resolution doesn't matter.

As a result Makefile target prerequisites are assumed to be concurrent.

Lets say you have a Makefile like this:

```Makefile
do: build run

build:
	echo "building"
	sleep 2
	echo "built"

run:
	echo "running"
```

and you run
```bash
make do
```

`do` will execute `build` and `run`. 

On many systems they will appear to execute sequentially:

```bash
$ make do
echo "building"
building
sleep 2
... 2 seconds later ...
echo "built"
built
echo "running"
running
```

This is because by default `make` only executes one job with a single core.

However GNU make is also able to run a given number of parallel jobs, which will then allow the concurrent parts of the Makefile to execute in parallel:
```bash
$ make --help | grep jobs
  -j [N], --jobs[=N]          Allow N jobs at once; infinite jobs with no arg.
# assuming your computer has more than one logical core
$ make -j 2 do
echo "building"
echo "running"
... run executes before build completes ...
running
building
sleep 2
echo "built"
built
```

This is important to keep in mind when creating a Makefile to ensure one is not defining a Makefile target which requires its dependencies to be evaluated in a specific order.
