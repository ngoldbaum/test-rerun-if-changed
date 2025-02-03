# Sample project to demonstrate `rerun-if-changed` problem

To trigger the issue, run the following test script:

```bash
$ mkdir -p .nox/test/bin
$ ln -s /bin/sh .nox/test/bin/python
$ cargo build
$ rm -r .nox
$ mkdir -p .nox/test/bin
$ ln -s /bin/sh .nox/test/bin/python
$ cargo build
```

You should see something like the following output:

```bash
$ mkdir -p .nox/test/bin

$ ln -s /bin/sh .nox/test/bin/python

$ cargo build
   Compiling test-rerun-if-changed v0.1.0 (/Users/goldbaum/Documents/test-rerun-if-changed)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.71s

$ rm -r .nox

$ mkdir -p .nox/test/bin

$ ln -s /bin/sh .nox/test/bin/python

$ cargo build
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
```

Note how the build script was *not* retriggered for the second `cargo build`
invocation.

I would think that deleting and recreating a file (producing a file with the
same content but a different `mtime`) would trigger a rebuild.