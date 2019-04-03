---
title: "External Utilities"
layout: single
classes: wide
lang: en
lang-ref: buildcell-manual
sidebar:
  nav: "docs"
---

AIRSS calls a number of external packages to perform specific tasks related to calculation and analysis. This page provides an overview of these packages.

spglib
------

An excellent library for finding and handling crystal symmetries, written in C by Atsushi Togo.

http://atztogo.github.io/spglib/

> **Note:** This package is fetched and installed automatically.

cellsymm
--------

This is a front-end to spglib, written by Michael Rutter.

http://www.tcm.phy.cam.ac.uk/sw/check2xsf/cellsym.tgz

> **Note:** This package is fetched and installed automatically. It will be replaced by `c2x` in due course.

SYMMOL
------

This Fortran code symmetrises a group of atoms. It can be downloaded [here](https://www.mtg.msm.cam.ac.uk/files/symmol.zip).

1. T. Pilati and A. Forni, [J. Appl. Cryst. **31**, 503–504 (1998)](https://doi.org/10.1107/S0021889898002180)
2. T. Pilati and A. Forni, [J. Appl. Cryst. **33**, 417 (2000)](https://doi.org/10.1107/S0021889800001801)

Before compilation, a patch is applied to `symmol.f`.

> **Note:** This package is fetched and installed automatically.

Castep
------

A high performance plane wave pseudopotential total energy code. It is written and maintained by the [Castep Developers Group](http://www.castep.org/). The executable should be given the name `castep`. In other words, copy the default `castep.mpi`
or `castep.serial` that is created after compiling Castep, and rename it to `castep`. This file should be placed in your path.

OPTADOS
-------

Calculates high quality theoretical DOS, Projected-DOS, Joint-DOS, Optics and core-loss spectroscopy.

http://www.tcm.phy.cam.ac.uk/~ajm255/optados/index.html

Gulp (optional)
---------------

Structure prediction may be performed using a variety of empirical force fields, as implemented in Julian Gale's powerful Gulp code.

http://gulp.curtin.edu.au/gulp/

LAMMPS (optional, not currently recommended)
--------------------------------------------

Structural optimisation is not currently sufficiently stable in this MD focussed code.

http://lammps.sandia.gov/

pspot
-----

This is a directory containing the default Castep `xx_00PBE.usp(cc)` pseudopotentials. In more recent versions of Castep the QC5 set of high-throughput potentials provide an alternative. These can be used for general searching, but tailored OTFG potentials are recommended for accurate results, and/or very high pressures. It is assumed that pspot is in your home directory. If not, set `PSPOT_DIR` appropriately. 

qhull
-----

Calculates the convex hulls.

http://www.qhull.org/

hull (optional)
---------------

Ken Clarkson's convex hull code.

http://www.netlib.org/voronoi/hull.html

antiprism (optional)
---------

http://www.antiprism.com/files/antiprism-0.24.1.tar.gz

R/Rscript
---------

The statistical package R is used to visualise ternary convex hulls. The `ternary.r` scripts can be executed using `Rscript`. The `ggtern` packaged is required.

https://cran.r-project.org/
http://www.ggtern.com/

xmgrace
-------

http://plasma-gate.weizmann.ac.il/Grace/

Xmgrace/Grace is useful for visualising results. `.agr` scripts are generated to facilitate this.

cif2cell
--------

This handy python utility can convert from cif files to a variety of electronic structure codes, including Castep.

http://cif2cell.sourceforge.net/
http://www.sciencedirect.com/science/article/pii/S0010465511000336
