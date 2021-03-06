---
title: "Buildcell Manual"
layout: single
classes: wide
lang: en
lang-ref: buildcell-manual
sidebar:
  nav: "docs"
---


    **Warning: this page is in a state of flux.**


The construction of reasonable, or _sensible_, random structures is central to AIRSS. The Fortran `buildcell` tool is provided in the AIRSS package for this purpose. It can build structures from scratch, or modify structures specified using the Castep `.cell` format. The random structures generated  are output in the Castep `.cell` format. `buildcell` reads from standard input (`stdin`), and writes to standard output (`stdout`). The output can be passed to the supplied Fortran `cabal` structure conversion tool. Additional information is reported to standard error (`stderr`).

```console
$ cat Al.cell

%BLOCK LATTICE_CART
2 0 0
0 2 0
0 0 2
%ENDBLOCK LATTICE_CART

%BLOCK POSITIONS_FRAC
Al 0.0 0.0 0.0 # Al1 % NUM=8
%ENDBLOCK POSITIONS_FRAC

#MINSEP=1.5

$ buildcell < Al.cell

Symm: P1         Nops:   1
Compacting the unit cell using a Niggli reduction
:----~
#              Generated by Buildcell
#
# at 12:00:00 (GMT+1.0) 1st June 2018
# in /Users/user/Al
# on
# by user
#
#      Author: C. J. Pickard (cjp20@cam.ac.uk)
#                Space group: P1
#

%BLOCK LATTICE_ABC
      3.24631      3.29003      6.29400
     92.47862    102.61547    110.29770
%ENDBLOCK LATTICE_ABC

%BLOCK POSITIONS_FRAC
  Al        0.9801918        0.2287102        0.0656909
  Al        0.7802843        0.4917482        0.8797641
  Al        0.2606859        0.2945663        0.8997372
  Al        0.4531337        0.7917730        0.6072080
  Al        0.5479149        0.1049917        0.1511797
  Al        0.6110164        0.7229138        0.0359822
  Al        0.4599122        0.2232240        0.3707223
  Al        0.6281024        0.9946698        0.8485314
%ENDBLOCK POSITIONS_FRAC

Timed:        0.00000
Total:        0.00610
Perc:         0.0%

$ buildcell < Al.cell | cabal cell res > Al.res

Symm: P1         Nops:   1
Compacting the unit cell using a Niggli reduction
:---~
Timed:        0.00000
Total:        0.01191
Perc:         0.0%

$ cat Al.res

TITL cabal-cell2res 0.0 64.004559 0.0 0 8 (n/a) n - 1
CELL 1.54180  3.35495  4.61258  4.86690 67.29051 70.27393 72.02060
LATT -1
SFAC Al
Al     1  0.7588650000000  0.6630602000000  0.4570794000000 1.0
Al     1  0.1109306000000  0.2909464000000  0.6328852000000 1.0
Al     1  0.8626585000000  0.6047508000000  0.9951972000000 1.0
Al     1  0.4688670000000  0.2785197000000  0.3177728000000 1.0
Al     1  0.3208300000000  0.8247542000000  0.7611079000000 1.0
Al     1  0.1007394000000  0.2634639000000  0.0993661000000 1.0
Al     1  0.0686251000000  0.7423036000000  0.1393760000000 1.0
Al     1  0.5536280000000  0.3415304000000  0.9743010000000 1.0
END
```

Cell size and shape
-------------------

Show bond angles, length distributions.

Cell content
------------

Cell symmetry
-------------

Units
-----

Glossary
--------

The following table lists all of the current directives that will be interpreted by `buildcell`, and provides a description of their intended usage.

**Directive**     | **Description** | **Default**
:------------     | :-------------- | :----------
**ABFIX**         | Fix the a- and b-axis. Should be placed in the `LATTICE_CART`/`LATTICE_ABC` block. | `false`
**ACONS**         | Rejects unit cells that are too flat. It takes a value of less than 1.0, and larger values favour more three dimensional cells. The volume of the unit cell is given below. The quantity in the square root must be greater than `ACONS` for the unit cell to be accepted.
**ADJGEN**        | A value of 0 enforces the maximum possible use of symmetry related general positions. Larger values permit greater use of special positions. It is increased dynamically if it proves difficult to generate stuctures with smaller values. | `0`
**ANGAMP**        | Amplitude of rotation for units (or molecules). It is to be supplied in degrees, and a negative value implies full rotation. It can be specified on a unit by unit basis. | `−1`
**BREAKAMP**      | Amplitude of random displacement of atoms to break symmetry. | `0`
**CELLADAPT**     | Permit a change in shape of the unit cell when applying distance constraints through the hard sphere potentials. | `false`
**CELLAMP**       | Amplitude for the random variation of a supplied unit cell. A negative value implies no relation to original cell. | `−1`
**CELLCON**       | Apply cell contraints. Specified as a vector (*a*, *b*, *c*, *α*, *β*, *γ*). For example `#CELLCON = -1 -1 -1 90 90 90` specifies a cubic unit cell, and `#CELLCON = -1 -1 -1 -1 -1 -1` a rhombohedral cell. `#CELLCON` conflicts with `#SYSTEM`.
**CFIX**          | Fix the c-axis. Should be placed in the `LATTICE_CART`/`LATTICE_ABC` block. | `false`
**CLUSTER**       | Selects internal settings for cluster geometries. | `false`
**COMPACT**       | Force a Niggli reduction of the unit cell. False by default if `#CLUSTER` is false, and the unit cell is not fixed using `#FIX`, `#ABFIX`, `#CFIX` or `#CELLAMP=0`. | `true`
**CONS**          | 
**COORD**         |
**CYLINDER**      |
**FIX**           | Fix the unit cell. | `false`
**FLIP**          | Randomly mirror the structural units. | `false`
**FOCUS**         | Focus the compositional search on elements (`#FOCUS=1`), binaries (`#FOCUS=2`), ternaries (`#FOCUS=3`), etc. | `0`
**MAXBANGLE**     | Maximum bond angle in coordination constraint application | `?`
**MAXTIME**       | Determines how long, in seconds, should be spent attempting to build structures with given settings. | `1`
**MINAMP**        | 
**MINBANGLE**     | Minimum bond angle in coordination constraint application | `?`
**MINSEP**        |
**NATOM**         | Generate structures with `NATOM` atoms. Ranges are allowed (e.g., #NATOM=5-10).
**NFORM**         | Number of formula units.
**NOCOMPACT**     | Force no Niggli reduction of the unit cell. | `false`
**NOPUSH**        | 
**OCTET**         | Checks if number of valence electrons is a multiple of eight. | `false`
**OVERLAP**       |
**PERMFRAC**      |
**PERMUTE**       |
**POSAMP**        |
**RAD**           |
**REMOVE**        |
**SGRANK**        |
**SHIFT**         |
**SLAB**          |
**SLACK**         |
**SPECIES**       | Species to include (e.g., `#SPECIES=H,Li`).
**SPHERE**        |
**SPIN**          |
**SUPERCELL**     |
**SURFACE**       |
**SYMM**          |
**SYMMNO**        |
**SYMMOPS**       | Build structures having a specified number of symmetry operations. For crystals, the allowed values are (1,2,3,4,6,8,12,16,24,48). For clusters (indicated with #CLUSTER), the allowed values are (1,2,3,5,4,6,7,8,9,10,11,12,24). Ranges are allowed (e.g., #SYMMOPS=1-4).
**SYMMORPHIC**    |
**SYSTEM**        | Enforce a crystal system. Allowed values are {Rhom,Tric,Mono,Cubi,Hexa,Orth,Tetra} | `none`
**TARGVOL**       | Target volume per atom (?) for random structures. Ranges are allowed.
**THREE**         |
**VACANCIES**     |
**VACUUM**        |
**VARVOL**        | Volume per atom (?) for random structures. Generated structures will have volumes within +-5% of this amount.
**WIDTH**         |
**XAMP**          |
**YAMP**          |
**ZAMP**          |
