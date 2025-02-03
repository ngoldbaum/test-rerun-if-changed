# Sample project to demonstrate `rerun-if-changed` problem

To trigger the issue, run the following test script in a Python environment that has `nox` installed:

```bash
$ nox
$ cargo build
$ rm -r .nox
$ nox
$ cargo build
$ touch .nox/test/bin/python
$ cargo build
```

You should see something like the following output:

```bash
$ nox
nox > Running session test
nox > Creating virtual environment (virtualenv) using python3.13 in .nox/test
nox > python -m pip install pytest
nox > Session test was successful.

$ cargo build
   Compiling test-rerun-if-changed v0.1.0 (/Users/goldbaum/Documents/test-rerun-if-changed)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.30s

$ rm -r .nox

$ nox
nox > Running session test
nox > Creating virtual environment (virtualenv) using python3.13 in .nox/test
nox > python -m pip install pytest
nox > Session test was successful.

$ cargo build
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s

$ touch .nox/test/bin/python

$ cargo build
   Compiling test-rerun-if-changed v0.1.0 (/Users/goldbaum/Documents/test-rerun-if-changed)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.23s
```

Note how the build script was *not* retriggered for the second `cargo build`
invocation, but it was retriggered after the third invocation.

I would think that deleting and recreating a file (producing a file with the
same content but a different `mtime`) would trigger a rebuild.