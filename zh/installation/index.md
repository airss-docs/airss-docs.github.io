---
title: "AIRSS Installation"
layout: single
classes: wide
lang: zh
lang-ref: installation
sidebar:
  nav: "docs"
---

Extract `airss-x.x.x.tgz` and then navigate into the `airss-x.x.x/` directory:

```console
$ tar -xvf airss-0.9.1.tgz

./._airss-0.9.1
airss-0.9.1/
airss-0.9.1/._bin
airss-0.9.1/bin/
...
airss-0.9.1/bin/castep2res
airss-0.9.1/bin/._crud.pl
airss-0.9.1/bin/crud.pl

$ cd airss-0.9.1
```

Execute the following compound command to perform a default installation:

```console
$ make ; make install ; make neat
```

The executables will be placed in `airss-0.9.1/bin`, which you should add to your path. To verify that the installation ran as expected, run the following command:

```console
$ make check
```

The output will tell you whether the essential, recommended, and optional components are installed and accessible. It will also attempt to run a select number of calculations from the examples.

> **Note:** It is strongly recommended that `gcc` and `gfortran` version 5 and above are used to build the AIRSS
utilities. Other compiler families (such as `ifort`) are not supported.

Troubleshooting
===============
