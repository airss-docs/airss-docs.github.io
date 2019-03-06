
Introduction
============

Ab initio Random Structure Searching (AIRSS) is a very simple, yet powerful and highly parallel, approach to structure prediction. The concept was introduced in 2006[^1] and its philosophy more extensively discussed in 2011.[^2]

Random structures—or more precisely, random "sensible" structures—are generated and then relaxed to nearby local energy minima. Particular success has been found using density functional theory (DFT) for the energies, hence the focus on "ab initio" random structure searching. The sensible random structures are constructed so that they have reasonable densities, and atomic separations. Additionally they may embody crystallographic, chemical or prior experimental/computational knowledge. Beyond these explicit constraints the emphasis is on a broad, uniform, sampling of structure space.

AIRSS has been used in a number of landmark studies in structure prediction, from the structure of SiH₄ under pressure to providing the theoretical structures which are used to understand dense hydrogen (and anticipating the mixed Phase IV),[^3] incommensurate phases in aluminium under terapascal pressures,[^4] and ionic phases of ammonia.[^5]

The approach naturally extends to the prediction of clusters/molecules, defects in solids,[^6] interfaces and surfaces (which can be thought of as interfaces with vacuum).[^7]

### Licence and citation

The AIRSS package is released under the [GPL 2.0 licence](https://www.gnu.org/licenses/gpl-2.0.html). See the `LICENCE` file for more details. You are not required to, but you might consider citing the following references in any work that makes use of the AIRSS package:

1. C.J. Pickard and R.J. Needs, [Phys. Rev. Lett., **97**, 045504 (2006)](https://doi.org/10.1103/PhysRevLett.97.045504)  
2. C.J. Pickard and R.J. Needs, [J. Phys.: Condens. Matter, **23**, 053201 (2011)](https://doi.org/10.1088/0953-8984/23/5/053201)  

Quick installation
==================

Execute the following to perform a default installation:

```console
$ make ; make install ; make neat
```

The executables will be placed in `airss/bin`, which you should add to your path.

The installation can be tested:

```console
$ make check
```

> **Note:** It is strongly recommended that gcc and gfortran version 5 and above are used to build the airss
utilities. Other compiler families (such as ifort) are not supported.

Detailed guides
===============

##[Custom installation](/installation)
Head to this page if the quick installation doesn't work.

##[External packages](/packages)
The AIRSS package makes use of a number of external utilities. Some of them are available from package managers, while others should be built locally. This guide provides details on how to make sure everything you need is available.

##[AIRSS examples](/examples)
The AIRSS package is documented through a growing list of worked examples. Chapter 1 of the examples uses the included pair potential code (`pp3`), and does not require any DFT package to be installed. Chapter 2 uses the Castep first principles total energy code.

##[Glossary](/glossary)
Use this 

Castep and AIRSS
================

The AIRSS package is tightly integrated with the Castep first principles total energy code. However, it is relatively straightforward to modify the scripts to use alternative codes to obtain the core functionality. xxx_relax scripts for vasp, pp3, gulp, psi4, and lammps are provided and integrated with the airss.pl script.


References
==========

[^1]: C.J. Pickard and R.J. Needs, [Phys. Rev. Lett., **97**, 045504 (2006)](https://doi.org/10.1103/PhysRevLett.97.045504)  
[^2]: C.J. Pickard and R.J. Needs, [J. Phys.: Condens. Matter, **23**, 053201 (2011)](https://doi.org/10.1088/0953-8984/23/5/053201)  
[^3]: C.J. Pickard and R.J. Needs, [Nat. Phys., **3**, 473 (2007)]()  
[^4]: C.J. Pickard and R.J. Needs, [Nat. Mater., **9**, 624 (2010)]()  
[^5]: C.J. Pickard and R.J. Needs, [Nat. Mater., **7**, 775 (2008)]()  
[^6]: A.J. Morris, C.J. Pickard and R.J. Needs, [Phys. Rev. B, **78**, 184102 (2008)]()  
[^7]: G. Schusteritsch and C.J. Pickard, [Phys. Rev. B, **90**, 035424 (2014)]()  
