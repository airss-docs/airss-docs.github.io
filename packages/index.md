External utilities
==================

spglib
------

[This package is fetched and installed automatically]

An excellent library for finding and handling crystal symmetries,
written in C by Atsushi Togo.

http://atztogo.github.io/spglib/

cellsymm
--------

[This package is fetched and installed automatically]

This is a front-end to spglib, written by Michael Rutter.

http://www.tcm.phy.cam.ac.uk/sw/check2xsf/cellsym.tgz

[To be replaced by c2x in due course ...]

symmol
------

[This package is fetched and installed automatically]

This fortran code symmetrises a group of atoms

Pilati, T. & Forni, A. (2000). J. Appl. Cryst. 33, 417.
and J. Appl. Cryst. (1998). 31, 503-504 doi:10.1107/S0021889898002180

https://www.mtg.msm.cam.ac.uk/files/symmol.zip

Before compilation a patch is applied to symmol.f

Castep
------

A high performance plane wave pseudopotential total energy
code. It is written and maintained by the Castep Developers Group.

http://www.castep.org/

The executable should be given the name 'castep', i.e. copy the default castep.mpi
or castep.serial to castep (which should be placed in your path).

OPTADOS
-------

Calculates high quality theoretical DOS, Projected-DOS, Joint-DOS, Optics
and core-loss spectroscopy.

http://www.tcm.phy.cam.ac.uk/~ajm255/optados/index.html

Gulp [optional]
----

Structure prediction may be performed using a variety of empirical force fields,
as implemented in Julian Gale's powerful Gulp code.

http://gulp.curtin.edu.au/gulp/

LAMMPS [optional, not currently recommended]
------

http://lammps.sandia.gov/

Structural optimisation is not currently sufficiently stable in this MD focussed code.

pspot
-----

This is a directory containing the default Castep xx_00PBE.usp(cc)
pseudopotentials. In more recent versions of Castep the QC5 set of high
throughput potentials provide an alternative. These can be used for general
searching, but tailored OTFG potentials are recommended for accurate results,
and/or very high pressures. It is assumed that pspot is in your home
directory. If not, set PSPOT_DIR appropriately. 

qhull
-----

Calculates the convex hulls.

http://www.qhull.org/

hull [optional]
----

Ken Clarkson's convex hull code.

http://www.netlib.org/voronoi/hull.html

antiprism [optional]
---------

http://www.antiprism.com/files/antiprism-0.24.1.tar.gz

R/Rscript
---------

The statistical package R is used to visualise ternary convex hulls. The ternary.r
scripts can be executed using Rscript. The ggtern packaged is required.

https://cran.r-project.org/
http://www.ggtern.com/

xmgrace
-------

http://plasma-gate.weizmann.ac.il/Grace/

Xmgrace/grace is useful for visualising results. AGR scripts are generated to facilitate this.

cif2cell
--------

http://cif2cell.sourceforge.net/
http://www.sciencedirect.com/science/article/pii/S0010465511000336

This handy python utility can convert from cif files to a variety of electronic structure codes,
including Castep.

The core AIRSS Scripts and Codes
================================

airss.pl
--------

This perl script performs Ab Initio Random Structure Searching. See the set of
Examples for instruction in its use. Random structures are repeatedly generated
by buildcell, and relaxed with the chosen code (the default is castep).

buildcell
---------

This fortan code reads annotated Castep cell files, and generates
random "sensible" structures from them. The type of randomness introduced
can be controlled through hash-tagged directives (which are treated as
comments and ignored by Castep).

cabal
-----

A structure conversion tool.

Usage: cabal in out < seed.in > seed.out
  in==out : Niggli reduce
  supports castep+,cell,shx,res,gulp*,cif*,psi4*,xtl,xyz(e)
  *output only +input only

The following converts a castep cell file to a SHLX results file.

  cabal cell res < input.cell > output.res

ca
--

A bash wrapper for cryan (see below). Uses the same command line options as cryan.

cryan
-----

A general purpose fortran program to analyse large amounts of structure data.
The structures are read from STDIN, for example:

     cat *.res | cryan -s
     gunzip -c lots.res.gz | cryan -f H2O -r
     find . -name "*.res" | xargs cat | cryan -m
     cat H2O-P21c.res | cryan -g

Experience suggests that cryan is suitable for the analysis of up to about 100K structures.
Other techniques are required for larger data sets.

castep_relax
------------

This bash script performs a self consistent geometry optimisation of the
specified structure using Castep.

vasp_relax
----------

This bash script performs a self consistent geometry optimisation of the
specified structure using VASP.

gulp_relax
-----------

This bash script performs a geometry optimisation of the
specified structure using gulp.

pp3_relax
----------

This bash script performs a geometry optimisation of the
specified structure using pp3, a very simple pair potential code.

lammps_relax
------------

This bash script performs a geometry optimisation of the
specified structure using lammps.

[not currently recommended due to issues with structural optimisation]

psi4_relax
------------

This bash script performs a geometry optimisation of the
specified structure using psi4.

[not currently recommended due to issues with structural optimisation]

gencell
-------

This bash script will generate a set of recommended Castep .cell and .param files
from a supplied unit cell volume, and atoms contained in the cell. It is strongly recommended
that this is used as a starting point for most projects.

pp3
---

A simple pair potential code, for testing. It is used in the early chapters of 
the examples.

run.pl
------

This perl script runs a batch of castep jobs in a directory. It is
useful for "polishing" your results, and high-throughput computation
in general. Failed runs are placed into 'bad_castep'.

crud.pl
-------

The Castep run daemon. A perl script for high-throughput batch calculations.
The structures to be relaxed are placed in the 'hopper' directory. Successful
calculations are placed in 'good_castep', and those that fail into 'bad_castep'.
The script can be run as a daemon (i.e. continues running once the hopper is empty,
and waits for more).

spawn
-----

This script can be used to submit multiple jobs to the selection of
machines listed in the ~/.spawn file. For example:

node1 slots=8 root=
node2 slots=8 root=
node3 slots=12 root=

Typing 'spawn airss.pl -seed Carbon' on your root node (on which it is
not advisable to run large jobs) will start a total of 28 instances of
airss.pl using the Carbon.* input files, on your 3 remote nodes. Spawn
uses ssh to run the commands remotely. Passwordless access to the
resources in your .spawn file is convenient 

The alternative to spawn or mpirun is to use the queueing system of a
multiuser computer cluster to submit multiple jobs. This should be
discussed with your system administrators.

spawn-slow
----------

Similar to the spawn script, this more slowly requests remote jobs. This
is recommended when launching the run.pl and crud.pl scripts.

despawn
-------

The spawn scripts records the PIDs of the remotely spawned jobs. This script
can be used to halt calculations in a controlled manner.

stopairss
---------

A script to kill spawned jobs. It will kill all jobs owned by you on
the remote nodes - so use with care. Use despawn in preference.

symm
----

Finds the space group of the structure.

tidy.pl
-------

Removes the output of uncompleted calculations.

niggli
------

Performs a Niggli transformation on all the SHLX res files in the current
directory.

prim
----

Converts all the SHLX res files in the current directory to primitive cells.

conv
----

Converts all the SHLX res files in the current directory to conventional cells.


Structure Prediction
====================

The use of the AIRSS scripts for a variety of structure prediction problems 
is illustrated by the collection of examples. Please now head to airss/examples.