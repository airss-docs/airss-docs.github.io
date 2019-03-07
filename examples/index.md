---
title: "AIRSS Documentation"
layout: single
classes: wide
sidebar:
  nav: "docs"
---

AIRSS Examples
==============

[**1.01**](#example-101): Lennard–Jones solid with 8 atoms  
[**1.02**](#example-102): Lennard–Jones cluster with 13 atoms  
[**1.03**](#example-103): Lennard–Jones cluster with 2–13 atoms  
[**1.04**](#example-104): Lennard–Jones cluster with 38 atoms and symmetry  
[**1.05**](#example-105): Lennard–Jones cluster with 38 atoms and relax-and-shake (RASH)  
[**1.06**](#example-106): Lennard–Jones cluster LJ56, using LJ55 icosahedral core  
[**1.07**](#example-107): Lennard–Jones surface with adatom  
[**1.08**](#example-108): Binary Lennard–Jones with fixed composition  
[**1.09**](#example-109): Binary Lennard–Jones with variable composition  
[**1.10**](#example-110): Ternary Lennard–Jones with variable composition  
[**1.11**](#example-111): Binary Lennard–Jones defect calculation  
[**1.12**](#example-112): Binary Lennard–Jones nanowire in a nanotube  
[**1.13**](#example-113): Binary Lennard–Jones interface  
[**2.01**](#example-201): CASTEP free search for 2 atoms of carbon at 100 GPa  
[**2.02**](#example-202): CASTEP free search for 8 atoms of hydrogen at 100 GPa, followed by molecular units  
[**2.03**](#example-203): CASTEP fixed cell search for γ-B28  
[**2.04**](#example-204): CASTEP fixed cell search for γ-B28 with units  
[**3.01**](#example-301): Gulp free search for 2 atoms of carbon at 100 GPa, Tersoff potential  
[**3.02**](#example-302): Gulp coordination constrained search, 8 atoms of carbon, Tersoff potential  
[**3.03**](#example-303): Gulp coordination constrained cluster search, 20 atoms of carbon, Tersoff potential  
[**3.04**](#example-304): Gulp symmetry unconstrained search for SiO₂ polymorphs, Vashishta potential  
[**3.05**](#example-305): Gulp symmetry unconstrained search for layered SiO₂ structures, Vashishta potential  
[**3.06**](#example-306): Gulp symmetry unconstrained search for small SiO₂ clusters, Vashishta potential  
[**3.07**](#example-307): Gulp symmetry constrained search for CH₄ molecular crystals, Tersoff potential  
[**4.01**](#example-401): VASP free search for 2 atoms of carbon at 100 GPa  

Example 1.01
============

In this example we will use random searching to find the ground state of a Lennard–Jones solid.

```console
$ ls

Al.cell	Al.pp	README

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
```

`Al.cell` is the "seed" input file which describes how the random structures should be built (by `buildcell`). In this case, 8 atoms are placed randomly into a unit cell of random shape, and a volume within 50% of 8 Å³ per atom. If a larger number of atoms were requested then the volume of the cell would be scaled appropriately. Any configurations in which atoms are closer than 1.5 Å are rejected.

We now perform the search:

```console
$ airss.pl -pp3 -max 20 -seed Al
```

The AIRSS perl script (`airss.pl`) is executed. The `-pp3` pair potential code is chosen for the structural optimisations (the default is `-castep`), and 20 relaxations are requested (the default being a very large number—which would require manual halting of the scripts by the user). The `Al.pp` file contains the parameters for the pair potential. The results can now be analysed.

```console
$ ca -r

Al-91855-9500-1       -0.00     7.561    -6.659  8 Al           P63/mmc    1
Al-91855-9500-6       -0.00     7.561     0.000  8 Al           P63/mmc    1
Al-91855-9500-5       -0.00     7.561     0.000  8 Al           P63/mmc    1
Al-91855-9500-10       0.00     7.564     0.005  8 Al           P-1        1
Al-91855-9500-17      -0.00     7.564     0.005  8 Al           P-1        1
Al-91855-9500-3       -0.00     7.564     0.005  8 Al           C2/m       1
Al-91855-9500-18      -0.00     7.564     0.005  8 Al           Pmmm       1
Al-91855-9500-20      -0.00     7.564     0.005  8 Al           C2/m       1
Al-91855-9500-4       -0.00     7.564     0.005  8 Al           Pmmm       1
Al-91855-9500-16      -0.00     7.564     0.005  8 Al           C2/m       1
Al-91855-9500-2       -0.00     7.564     0.005  8 Al           Cmmm       1
Al-91855-9500-11      -0.00     7.564     0.005  8 Al           C2/m       1
Al-91855-9500-9       -0.00     7.564     0.005  8 Al           Cmmm       1
Al-91855-9500-8        0.00     7.564     0.005  8 Al           Fmmm       1
Al-91855-9500-19       0.00     7.784     0.260  8 Al           C2/m       1
Al-91855-9500-12      -0.00     8.119     0.700  8 Al           R-3c       1
Al-91855-9500-14      -0.00     8.446     0.705  8 Al           Cm         1
Al-91855-9500-13       0.00     8.453     0.794  8 Al           C2/m       1
Al-91855-9500-15       0.00     8.465     0.802  8 Al           C2/m       1
Al-91855-9500-7        0.00     8.505     0.834  8 Al           P21/m      1
```

The above is a "ranking" of the results according to energy (strictly, enthalpy). Columns are: (1) Unique structure name; (2) Pressure (GPa); (3) Volume (Å³ per formula unit); (4) Enthalpy (eV per formula unit) for the first structure, and then relative to that; (5) Number of formula units; (6) Chemical formula; (7) Symmetry; (8) Number of repeats.

For such a simple system you are likely to find the HCP ground state (space group `P63/mmc`) within these 20 attempts. Of course, this is a stochastic approach, and you may be unlucky. If so, simply run the AIRSS script again.

In the above example, we found the HCP structure 3 times. It can be helpful to unify repeated structures.

```console
$ ca -u 0.01 -r

Al-91855-9500-1       -0.00     7.561    -6.659  8 Al           P63/mmc    3
Al-91855-9500-10       0.00     7.564     0.005  8 Al           P-1       11
Al-91855-9500-19       0.00     7.784     0.260  8 Al           C2/m       1
Al-91855-9500-12      -0.00     8.119     0.700  8 Al           R-3c       1
Al-91855-9500-14      -0.00     8.446     0.705  8 Al           Cm         1
Al-91855-9500-13       0.00     8.453     0.794  8 Al           C2/m       1
Al-91855-9500-15       0.00     8.465     0.802  8 Al           C2/m       1
Al-91855-9500-7        0.00     8.505     0.834  8 Al           P21/m      1
```

The value `0.01` is the threshold of similarity. If you are happy with unification, you can make it permanent.

```console
$ ca -u 0.01 -r --delete

Deleting files. To confirm type <Enter>
Al-91855-9500-1       -0.00     7.561    -6.659  8 Al           P63/mmc    3
Al-91855-9500-10       0.00     7.564     0.005  8 Al           P-1       11
Al-91855-9500-19       0.00     7.784     0.260  8 Al           C2/m       1
Al-91855-9500-12      -0.00     8.119     0.700  8 Al           R-3c       1
Al-91855-9500-14      -0.00     8.446     0.705  8 Al           Cm         1
Al-91855-9500-13       0.00     8.453     0.794  8 Al           C2/m       1
Al-91855-9500-15       0.00     8.465     0.802  8 Al           C2/m       1
Al-91855-9500-7        0.00     8.505     0.834  8 Al           P21/m      1

$ ca -r

Al-91855-9500-1       -0.00     7.561    -6.659  8 Al           P63/mmc    3
Al-91855-9500-10       0.00     7.564     0.005  8 Al           P-1       11
Al-91855-9500-19       0.00     7.784     0.260  8 Al           C2/m       1
Al-91855-9500-12      -0.00     8.119     0.700  8 Al           R-3c       1
Al-91855-9500-14      -0.00     8.446     0.705  8 Al           Cm         1
Al-91855-9500-13       0.00     8.453     0.794  8 Al           C2/m       1
Al-91855-9500-15       0.00     8.465     0.802  8 Al           C2/m       1
Al-91855-9500-7        0.00     8.505     0.834  8 Al           P21/m      1
```

Example 1.02
============

In this example we will use random searching to find the ground state of a Lennard–Jones cluster.

```console
$ ls

Al.cell	Al.pp	README

$ cat Al.cell

%BLOCK LATTICE_CART
20 0 0
0 20 0
0 0 20
#FIX
%ENDBLOCK LATTICE_CART

%BLOCK POSITIONS_FRAC
Al 0.0 0.0 0.0 # Al1 % NUM=13
%ENDBLOCK POSITIONS_FRAC

FIX_ALL_CELL : true

#MINSEP=1.5
#CLUSTER
#POSAMP=3.0
```

13 atoms are placed in a large fixed box. Centred on the origin, they are uniformly distributed over a sphere of radius 3.0 Å. If any atoms are closer than 1.5 Å the configuration is rejected.

We now perform the search:

```console
$ airss.pl -pp3 -cluster -max 20 -seed Al
$ ca -r

Al-72120-6057-19         0.00   615.385      -3.410  13 Al           Ih         1
Al-72120-6057-4          0.00   615.385       0.000  13 Al           Ih         1
Al-72120-6057-7          0.00   615.385       0.220  13 Al           Cs         1
Al-72120-6057-8          0.00   615.385       0.220  13 Al           Cs         1
Al-72120-6057-3          0.00   615.385       0.220  13 Al           Cs         1
Al-72120-6057-14         0.00   615.385       0.274  13 Al           C2v        1
Al-72120-6057-5          0.00   615.385       0.285  13 Al           C1         1
Al-72120-6057-10         0.00   615.385       0.285  13 Al           C1         1
Al-72120-6057-16         0.00   615.385       0.323  13 Al           Cs         1
Al-72120-6057-15         0.00   615.385       0.327  13 Al           Cs         1
Al-72120-6057-9          0.00   615.385       0.348  13 Al           C1         1
Al-72120-6057-12         0.00   615.385       0.351  13 Al           C1         1
Al-72120-6057-13         0.00   615.385       0.354  13 Al           C1         1
Al-72120-6057-11         0.00   615.385       0.354  13 Al           C1         1
Al-72120-6057-2          0.00   615.385       0.356  13 Al           C1         1
Al-72120-6057-18         0.00   615.385       0.359  13 Al           C1         1
Al-72120-6057-20         0.00   615.385       0.390  13 Al           C1         1
Al-72120-6057-6          0.00   615.385       0.416  13 Al           C1         1
Al-72120-6057-1          0.00   615.385       0.439  13 Al           C1         1
Al-72120-6057-17         0.00   615.385       0.490  13 Al           C1         1

$ ca -u 0.01 -r

Al-72120-6057-19         0.00   615.385      -3.410  13 Al           Ih         2
Al-72120-6057-8          0.00   615.385       0.220  13 Al           Cs         3
Al-72120-6057-14         0.00   615.385       0.274  13 Al           C2v        1
Al-72120-6057-5          0.00   615.385       0.285  13 Al           C1         2
Al-72120-6057-16         0.00   615.385       0.323  13 Al           Cs         1
Al-72120-6057-15         0.00   615.385       0.327  13 Al           Cs         1
Al-72120-6057-9          0.00   615.385       0.348  13 Al           C1         1
Al-72120-6057-12         0.00   615.385       0.351  13 Al           C1         1
Al-72120-6057-13         0.00   615.385       0.354  13 Al           C1         1
Al-72120-6057-11         0.00   615.385       0.354  13 Al           C1         1
Al-72120-6057-2          0.00   615.385       0.356  13 Al           C1         1
Al-72120-6057-18         0.00   615.385       0.359  13 Al           C1         1
Al-72120-6057-20         0.00   615.385       0.390  13 Al           C1         1
Al-72120-6057-6          0.00   615.385       0.416  13 Al           C1         1
Al-72120-6057-1          0.00   615.385       0.439  13 Al           C1         1
Al-72120-6057-17         0.00   615.385       0.490  13 Al           C1         1
```

The known icosahedral ground state has been found 6 times in this test. The resulting structures are stored in SHELX format `.res` files and can be converted to other formats using the cabal utility.

```console
$ cat Al-72120-6057-19.res

TITL Al-72120-6057-19 0.0000000000 8000.0000000000 -44.3248941562 0 0 13 (Ih) n - 1
REM
REM in /Users/user/examples/1.2
REM
REM
REM
REM
REM
REM buildcell < ./Al.cell (8f0fc5c9d882ed20c27978dd052da9d4)
REM AIRSS Version 0.9.1 July 2018 build 92bfd83db9d4+ Sat, 30 Jun 2018 13:13:37 +0100
REM compiler GCC version 5.5.0
REM options -fPIC -feliminate-unused-debug-symbols -mmacosx-version-min=10.13.6 -mtune=core2 -g -O0
REM seed -1471360510 667860809 1027640838 1038292373 -213802532 -1539206485 -1872642957 -340800072 -697171857 -761177086 -1542735574 -885338462
REM
CELL 1.54180   20.00000   20.00000   20.00000   90.00000   90.00000   90.00000
LATT -1
SFAC Al
Al     1  0.6038930453823  0.4836194826238  0.5253309492534 1.0
Al     1  0.5723861716435  0.5636803790552  0.4509205384933 1.0
Al     1  0.4999999997846  0.5000000000790  0.5000000075654 1.0
Al     1  0.4276138527499  0.4363195677348  0.5490794435604 1.0
Al     1  0.4701141676138  0.6024370215993  0.4821888755138 1.0
Al     1  0.5298858276893  0.3975629918030  0.5178112246213 1.0
Al     1  0.4526399878259  0.4244380397184  0.4387532754310 1.0
Al     1  0.5473600525560  0.5755619778738  0.5612466872006 1.0
Al     1  0.4789066108987  0.5271042987692  0.3974126364780 1.0
Al     1  0.3961069520357  0.5163804721092  0.4746690478913 1.0
Al     1  0.5210933793772  0.4728958126855  0.6025874103552 1.0
Al     1  0.4384134677060  0.5463289943787  0.5759240890051 1.0
Al     1  0.5615864847371  0.4536709615702  0.4240759146313 1.0
END
```

The first line (`TITL`) contains stored data in the following format:

`TITL <name> <pressure> <volume> <enthalpy> <spin> <modspin> <#ions> <(symmetry)> n - <#copies>`

The cabal utility can be used to convert the `.res` file into an `.xyz` file.

```console
$ cabal res xyz < Al-72120-6057-19.res

13

Al     1.1791750764873     1.7827258262017    -0.3360534169762
Al     0.7363476807396     1.0192632282492     1.7607929157278
Al    -0.0000000029634    -0.0000000036287     0.0000000023555
Al    -0.7363484783607    -1.0192621771574    -1.7607931876182
Al     0.5875415154852    -1.2489218391593     1.6662791865905
Al    -0.5875402622518     1.2489211530929    -1.6662801418123
Al    -2.1222590886862     0.1555499373659    -0.3915580422390
Al     2.1222589581482    -0.1555490740574     0.3915591037870
Al    -1.3040498658070     0.0136119341742     1.7264894398813
Al    -1.1791758452594    -1.7827253947705     0.3360529947887
Al     1.3040514186501    -0.0136131959079    -1.7264882570909
Al     0.9383998151967    -1.8872737156990    -0.4889793760508
Al    -0.9384009213786     1.8872733212961     0.4889787786564
```

Example 1.03
============

In this example we will use random searching to find the ground state of Lennard–Jones clusters of a range of sizes.

```console
$ cat Al.cell

%BLOCK LATTICE_CART
20 0 0
0 20 0
0 0 20
#FIX
%ENDBLOCK LATTICE_CART

%BLOCK POSITIONS_FRAC
Al 0.0 0.0 0.0 # Al1 % NUM=8-16
%ENDBLOCK POSITIONS_FRAC

FIX_ALL_CELL : true

#MINSEP=1.5
#CLUSTER
#POSAMP=3.0
```

This is the same seed file as for Example 1.02, except this time a range of clusters sizes is explored.

```console
$ airss.pl -pp3 -cluster -max 200 -seed Al
$ ca -s

Al-70852-9173-163        0.00   500.000      -3.551  16 Al           Cs        1   200      ~
Number of structures   :    200
Number of compositions :      1
```

The command `ca -s` produces a summary of the results indicating the most stable structure (judged by energy per formula unit), which in this case is obviously the largest cluster.

```console
$ ca -u 0.01 -s -cl

Al-70852-9173-70         -16.505    7 Al           D5h       5    19      ~
Al-70852-9173-99         -19.821    8 Al           Cs       16    22    -0.976
Al-70852-9173-167        -24.112    9 Al           C2v       2    15    -0.017
Al-70852-9173-146        -28.421   10 Al           C3v       5    26    -0.034
Al-70852-9173-43         -32.765   11 Al           C2v       5    22    -0.858
Al-70852-9173-77         -37.966   12 Al           C5v       2    13    -1.158
Al-70852-9173-174        -44.325   13 Al           Ih        3    23     2.841
Al-70852-9173-19         -47.843   14 Al           C3v       5    21    -0.959
Al-70852-9173-144        -52.320   15 Al           C2v       6    22    -0.016
Al-70852-9173-163        -56.813   16 Al           Cs        4    17      ~
Number of structures   :    200
Number of compositions :     10
```

The above is another summary of the results, this time in cluster mode, indicating the most stable structures for each cluster size. The sixth and seventh columns give the number of times the most stable structure is found, and the total number of structures. You may check your results using [this excellent resource](http://doye.chem.ox.ac.uk/jon/structures/LJ/tables.150.html).

Plotting the energy/atom as a function of cluster size is a good way to identify "magic number"—or particularly stable—clusters.

```console
$ ca -u 0.01 -s -cl | awk '{print $3,$2/$3}' | xmgrace -pipe
```

It should be clear from your results that the 13 atom `Ih` symmetry cluster is particularly stable. Indeed, the final column of the summary presents the second derivative of the total energy with cluster size. The curvature is strongly positive for the 13 atom cluster, indicating high stability as compared to its neighbours.

Example 1.04
============

In this example we will use random searching to find the ground state of Lennard–Jones cluster with 38 atoms, using symmetry.

```console
$ cat Al.cell

%BLOCK LATTICE_CART
20 0 0
0 20 0
0 0 20
#FIX
%ENDBLOCK LATTICE_CART

%BLOCK POSITIONS_FRAC
Al 0.0 0.0 0.0 # Al1 % NUM=38
%ENDBLOCK POSITIONS_FRAC

FIX_ALL_CELL : true

#MINSEP=1.5
#CLUSTER
#POSAMP=3.75
#SYMMOPS=2-5
#NFORM=1
```

Random clusters will be built containing 38 atoms and with the symmetry of a randomly chosen point group with between two and five symmetry operations (`#SYMMOPS=2-5`). If `NFORM=1` is omitted, the default behaviour is that the number of atoms in the cluster will be nops x 38, which is unlikely to be satisfied in a sphere of radius 3.75 and minimum separation of 1.5.

```console
$ airss.pl -pp3 -cluster -max 200 -seed Al &
```

The searches can be run in the background and monitored during their progress using the `ca` command.

```console
$ ca -u 0.01 -s -cl -r -t 10

Al-65004-3113-183     -173.928  38 Al           Oh        2   200
Number of structures   :    200
Number of compositions :      1
Al-65004-3113-183      0.00   210.526    -4.577 38 Al           Oh         2
Al-65004-3113-100      0.00   210.526     0.018 38 Al           C5v        3
Al-65004-3113-14       0.00   210.526     0.026 38 Al           C5v        7
Al-65004-3113-128      0.00   210.526     0.078 38 Al           D4h        5
Al-65004-3113-111      0.00   210.526     0.078 38 Al           C3         1
Al-65004-3113-110      0.00   210.526     0.083 38 Al           C2h        1
Al-65004-3113-33       0.00   210.526     0.097 38 Al           C1         1
Al-65004-3113-50       0.00   210.526     0.100 38 Al           C2         1
Al-65004-3113-164      0.00   210.526     0.100 38 Al           D2         2
Al-65004-3113-65       0.00   210.526     0.105 38 Al           C1         1
```

The above is a summary of the completed run, followed by the top ten (`-t`) structures. The known Oh ground state of LJ38 is found twice in 200 attempts in this example, and the C5v icosahedral minima is also located. The use of symmetry is highly recommended—compare to the results of Example 1.05.

Example 1.05
============

In this example we will use random searching to find the ground state of Lennard–Jones cluster with 38 atoms, and the relax and shake algorithm.

```console
$ cat Al.cell

%BLOCK LATTICE_CART
20 0 0
0 20 0
0 0 20
#FIX
%ENDBLOCK LATTICE_CART

%BLOCK POSITIONS_FRAC
Al 0.0 0.0 0.0 # Al1 % NUM=38
%ENDBLOCK POSITIONS_FRAC

FIX_ALL_CELL : true

#MINSEP=1.5
#CLUSTER
#POSAMP=3.75
```

38 atoms are placed uniformly in a sphere of radius 3.75, with any structures with nearest neighbour distances less than 1.5 Å rejected.

```console
$ airss.pl -pp3 -cluster -max 3000 -seed Al &
$ ca -u 0.01 -s -cl -r -t 10

Al-66136-6501-1502    -173.928  38 Al           Oh        3  3000
Number of structures   :   3000
Number of compositions :      1
Al-66136-6501-1502     0.00   210.526    -4.577 38 Al           Oh         3
Al-66136-6501-1916     0.00   210.526     0.021 38 Al           Cs         4
Al-66136-6501-1043     0.00   210.526     0.028 38 Al           C1         1
Al-66136-6501-489      0.00   210.526     0.042 38 Al           Cs         1
Al-66136-6501-1476     0.00   210.526     0.054 38 Al           C1         3
Al-66136-6501-2801     0.00   210.526     0.055 38 Al           C1         1
Al-66136-6501-2204     0.00   210.526     0.062 38 Al           Cs         3
Al-66136-6501-1936     0.00   210.526     0.064 38 Al           C3v        1
Al-66136-6501-2012     0.00   210.526     0.066 38 Al           C1         1
Al-66136-6501-1839     0.00   210.526     0.066 38 Al           C1         1
```

The known Oh LJ38 cluster is located 3 times in 3000 attempts.

```console
$ airss.pl -pp3 -cluster -max 3000 -num 100 -amp 1.0 -seed Al &
```

The relax and shake (RASH) algorithm is a minimal parameter "learning" algorithm. The first step is a structural optimisation of a random structure, which becomes the current "best" structure. The resulting local minima is "shaken"—a random displacement of amplitude 1.0 Å (`-amp 1.0`) is applied to all atoms (subject to the minimum separation constraint) and relaxed. This shaking of the best (or most stable) structure is repeated `-num` times (after which a new random structure is generated) or until a better structure is found, and then itself shaken up to `-num` times.

```console
$ ca -u 0.01 -s -cl -r

Al-46696-5893-2640    -173.928  38 Al           Oh        1    15
Number of structures   :     15
Number of compositions :      1
Al-46696-5893-2640     0.00   210.526    -4.577 38 Al           Oh         1
Al-46696-5893-834      0.00   210.526     0.018 38 Al           C5v        1
Al-46696-5893-1036     0.00   210.526     0.021 38 Al           Cs         9
Al-46696-5893-2807     0.00   210.526     0.045 38 Al           C1         1
Al-46696-5893-1475     0.00   210.526     0.066 38 Al           C1         1
Al-46696-5893-1363     0.00   210.526     0.067 38 Al           C2v        2
```

By default the rejected structures are removed so we are only left with 15 distinct structures. The AIRSS script can be called with the `-track` flag to suppress this behaviour.

In the above example, in both random search, and RASH, the known ground state for LJ38 was located—with random search apparently more successful. The mean number of attempts until first encounter (Na) is around 10,000 for random search and 1,000 for RASH with the parameters used, and due to the exponential probability distribution of such success/fail computational experiments the standard deviation is also Na, and hence large. Many runs must be performed before a good estimate of Na is obtained and reasonable assessment of the relative performance of methods may be made. An advantage of the RASH algorithm is that the structural optimisations following a shake require fewer steps than the relaxtion of a completely random structure. A disadvantage is that the values of `num` and `amp` must be determined—and if `num` is large (so that the search is stuck in unpromising basins) or `amp` is small (so the local minimum is never escaped on shaking), Na will tend to infinity. A good recommendation for `amp` is about half a typical bond length. For large systems RASH is a very effective way to obtain "quite a good solution quite quickly."

It is clear, for this problem, that the use of symmetry explored in 1.04 is recommended.

Example 1.06
============

In this example we will use random searching to find the ground state of the Lennard–Jones 56 atom cluster, using a pre-built LJ55 icosahedral core.

```console
$ cat Al.cell

%BLOCK LATTICE_CART
20 0 0
0 20 0
0 0 20
#FIX
%ENDBLOCK LATTICE_CART

%BLOCK POSITIONS_FRAC
Al       0.1311103     0.0067124     0.1719006 # Ih
Al       0.0306298     0.0572550     0.1731731 # Ih
Al      -0.0702839     0.1069271     0.1719000 # Ih
Al      -0.0638148    -0.0053629     0.1731728 # Ih
Al      -0.0563753    -0.1175927     0.1719000 # Ih
Al       0.0376361    -0.0558453     0.1731731 # Ih
Al       0.0667831     0.0027273    -0.1693897 # Ih
Al       0.0593436     0.1149571    -0.1681170 # Ih
Al      -0.0346679     0.0532098    -0.1693899 # Ih
Al      -0.1281420    -0.0093480    -0.1681174 # Ih
Al      -0.0276615    -0.0598906    -0.1693899 # Ih
Al       0.0732522    -0.1095627    -0.1681169 # Ih
Al      -0.0278654     0.1454824    -0.1039667 # Ih
Al      -0.1223100     0.0828646    -0.1039667 # Ih
Al      -0.1109734    -0.1001356    -0.1039667 # Ih
Al      -0.0095222    -0.1506181    -0.1039667 # Ih
Al       0.1432920    -0.0493000    -0.1039658 # Ih
Al       0.1362857     0.0638000    -0.1039658 # Ih
Al       0.0124908     0.1479825     0.1077493 # Ih
Al      -0.1403238     0.0466647     0.1077489 # Ih
Al      -0.1333174    -0.0664356     0.1077490 # Ih
Al       0.0308333    -0.1481180     0.1077494 # Ih
Al       0.1252778    -0.0855000     0.1077497 # Ih
Al       0.1139417     0.0975000     0.1077496 # Ih
Al      -0.2082562    -0.0143111     0.0420247 # Ih
Al      -0.0748214    -0.1546632     0.0673151 # Ih
Al       0.1176064    -0.1764616     0.0420255 # Ih
Al       0.1724377     0.0092725     0.0673157 # Ih
Al       0.0951018     0.1868191     0.0420254 # Ih
Al      -0.0931643     0.1414373     0.0673150 # Ih
Al      -0.1694694    -0.0119081    -0.0635325 # Ih
Al      -0.0921333    -0.1894547    -0.0382422 # Ih
Al       0.0961326    -0.1440729    -0.0635318 # Ih
Al       0.2112244     0.0116752    -0.0382415 # Ih
Al       0.0777897     0.1520276    -0.0635319 # Ih
Al      -0.1146382     0.1738260    -0.0382424 # Ih
Al       0.0651309     0.0026250     0.0853667 # Ih
Al       0.0474506     0.0910579     0.0215974 # Ih
Al      -0.0337541     0.0518307     0.0853667 # Ih
Al      -0.1014989    -0.0076974     0.0215970 # Ih
Al      -0.0269250    -0.0584091     0.0853667 # Ih
Al       0.0585000    -0.0873138     0.0215975 # Ih
Al       0.1044667     0.0050618    -0.0178138 # Ih
Al       0.0298932     0.0557735    -0.0815830 # Ih
Al      -0.0555322     0.0846778    -0.0178143 # Ih
Al      -0.0621626    -0.0052606    -0.0815832 # Ih
Al      -0.0444824    -0.0936934    -0.0178142 # Ih
Al       0.0367222    -0.0544667    -0.0815830 # Ih
Al      -0.1513304    -0.1026356     0.0018912 # Ih
Al       0.1542987     0.1000000     0.0018919 # Ih
Al       0.0128207    -0.1843180     0.0018917 # Ih
Al       0.1656352    -0.0830000     0.0018920 # Ih
Al      -0.0098524     0.1816824     0.0018915 # Ih
Al      -0.1626667     0.0803646     0.0018911 # Ih
Al       0.0014841    -0.0013178     0.0018916 # Ih
Al       0.0000000     0.0000000     0.0000000 # Al % NUM=1 POSAMP=6 MINAMP=5
%ENDBLOCK POSITIONS_FRAC

FIX_ALL_CELL : true

#MINSEP=1.5
#CLUSTER
#POSAMP=0
#ANGAMP=0
```

We start with a 55 atom icosahedral (point group `Ih`) cluster, centred on the origin. On generating the random structure this cluster is neither shifted (`#POSAMP=0`) nor rotated (`#ANGAMP=0`). The `# Ih` label is identical for all the 55 atoms of the cluster. The 56th atom is randomly located in a shell of outer radius 6 Å (`#POSAMP=6`) and inner radius 5 Å (`#POSAMP=5`). Structures in which any atoms are closer than 1.5 Å are rejected.

```console
$ airss.pl -pp3 -cluster -max 10 -seed Al
$ ca -s -cl -r

Al-52722-9075-6       -283.643  56 Al           C3v       1    10
Number of structures   :     10
Number of compositions :      1
Al-52722-9075-6        0.00   142.857    -5.065 56 Al           C3v        1
Al-52722-9075-1        0.00   142.857     0.006 56 Al           Cs         1
Al-52722-9075-10       0.00   142.857     0.006 56 Al           Cs         1
Al-52722-9075-8        0.00   142.857     0.006 56 Al           Cs         1
Al-52722-9075-5        0.00   142.857     0.006 56 Al           Cs         1
Al-52722-9075-3        0.00   142.857     0.006 56 Al           Cs         1
Al-52722-9075-9        0.00   142.857     0.006 56 Al           Cs         1
Al-52722-9075-7        0.00   142.857     0.006 56 Al           Cs         1
Al-52722-9075-4        0.00   142.857     0.006 56 Al           Cs         1
Al-52722-9075-2        0.00   142.857     0.006 56 Al           Cs         1
```

In the above run the known ground state structure of LJ56 is located after 10 attempts.

Example 1.07
============

In this example we build a slab supercell of Lennard–Jones HCP crystal, and add another atom to the surface randomly.

```console
$ cat Al.cell

%BLOCK LATTICE_CART
       1.908194408661965      -1.101696555507125       0.000000000000000
      -0.000000000000000       2.203393111014249       0.000000000000000
       0.000000000000000       0.000000000000000      35.964580000000005
#FIX
%ENDBLOCK LATTICE_CART

%BLOCK POSITIONS_FRAC
 Al   0.3333333333333333  -0.3333333333333333  0.3499999999999998 # Al1
 Al   0.3333333333333333  -0.3333333333333333  0.4499999999999998 # Al2
 Al   0.0000000000000000   0.0000000000000000  0.2999999999999998 # Al3
 Al   0.0000000000000000   0.0000000000000000  0.3999999999999999 # Al4
 Al   0.0000000000000000   0.0000000000000000  0.6000000000000000 # Al5 % NUM=1 ADATOM ZAMP=2.0
%ENDBLOCK POSITIONS_FRAC

FIX_ALL_CELL : true

#SUPERCELL=2 2 1
#SLAB
#POSAMP=0
#MINSEP=2.2
```

The shape of the primitive slab (atoms `Al1` to `Al4`) is fixed by `#FIX` in the last line of the `LATTICE_CART` block. The atom `Al5` is an `ADATOM`. This means that is it added after the slab supercell has been built. The supercell is generated using `#SUPERCELL=2 2 1`. In this case a 2 × 2 × 1 supercell is generated. You might also use `#SUPERCELL=4`, which generates a random supercell of area 4. If `#SLAB` is omitted then the random supercell has a random volume of 4 (i.e. 2 × 1 × 2 is permitted). The global `POSAMP` is 0.0, but the added atom has `#ZAMP=2.0`, which means that the X and Y coordinates are completely random, but the Z amplitude is less than or equal to 2.0 Å. Initial configurations with atomic separations of less than 2.2 Å are rejected. The shape of the unit cell is not optimised during the relaxation.

```console
$ airss.pl -pp3 -max 10 -seed Al
$ ca -r

Al-87679-1265-4        0.00    35.580    -5.570 17 Al           P3m1       1
Al-87679-1265-2        0.00    35.580     0.223 17 Al           P3m1       1
Al-87679-1265-7        0.00    35.580     0.223 17 Al           P3m1       1
Al-87679-1265-9        0.00    35.580     0.223 17 Al           P3m1       1
Al-87679-1265-5        0.00    35.580     0.223 17 Al           P3m1       1
Al-87679-1265-6        0.00    35.580     0.223 17 Al           P3m1       1
Al-87679-1265-3        0.00    35.580     0.223 17 Al           P3m1       1
Al-87679-1265-8        0.00    35.580     0.223 17 Al           P3m1       1
Al-87679-1265-10       0.00    35.580     0.223 17 Al           P3m1       1
Al-87679-1265-1        0.00    35.580     0.223 17 Al           P3m1       1
```

Example 1.08
============

In this example we search for stable crystalline configurations in the well known Kob–Anderson Binary Lennard–Jones system, at the A:B 80:20 composition.

```console
$ cat AB.cell

#VARVOL=15
#SPECIES=A%NUM=4,B%NUM=1
#NFORM=2
#MINSEP=1.5

$ cat AB.pp

2 12 6 5
A B
# Epsilon
1.00 1.50
0.50
# Sigma
2.00 1.60
1.76

$ airss.pl -pp3 -max 100 -seed AB
$ ca -r -t 10

AB-85154-3384-55       0.00    30.784     -45.812   2 BA4          C2/m       1
AB-85154-3384-43       0.00    30.784       0.000   2 BA4          C2/m       1
AB-85154-3384-13      -0.00    30.836       0.045   2 BA4          P21/m      1
AB-85154-3384-64      -0.00    30.836       0.045   2 BA4          P21/m      1
AB-85154-3384-96       0.00    30.836       0.045   2 BA4          P21/m      1
AB-85154-3384-99       0.00    30.836       0.045   2 BA4          P21/m      1
AB-85154-3384-77      -0.00    30.748       0.361   2 BA4          I4/mmm     1
AB-85154-3384-14      -0.00    30.748       0.361   2 BA4          I4/mmm     1
AB-85154-3384-56      -0.00    30.748       0.361   2 BA4          I4/mmm     1
AB-85154-3384-17       0.00    31.273       0.716   2 BA4          Cmmm       1
```

Example 1.09
============

In this example we explore a range of compositions using the Kob–Anderson Binary Lennard–Jones parameters.

```console
$ cat AB.cell

#VARVOL=15
#SPECIES=A,B
#NATOM=2-8
#MINSEP=1.5

$ airss.pl -pp3 -max 100 -seed AB
```

If the qhull package has been installed, we can compute the binary convex hull (or Maxwell construction).

```
$ ca -m | sort -n -k 6 -k 5

AB-98628-151-25        0.00     8.849    -21.276  -4.371   0.000 +  1 BA           Pm-3m      1
AB-98628-151-86        0.00    34.305    -76.365  -3.801   0.000 +  1 B5A3         Cmmm       1
AB-98628-151-90       -0.00    12.815    -27.218  -3.502   0.000 +  2 B2A          C2/m       1
AB-98628-151-10        0.00    17.018    -32.238  -2.837   0.000 +  2 B3A          P63/mmc    1
AB-98628-151-17        0.00     7.357     -8.356   0.000   0.000 +  6 A            P63/mmc    1
AB-98628-151-73        0.00     5.013     -4.178   0.000  -0.000 +  2 B            P63/mmc    1
AB-98628-151-20       -0.00    39.596    -66.698  -2.366   0.132 -  1 B2A5         C2/m       1
AB-98628-151-98        0.00    21.904    -35.578  -2.102   0.168 -  1 B4A          I4/m       1
AB-98628-151-88       -0.00    26.886    -39.512  -1.711   0.180 -  1 B5A          Cmmm       1
AB-98628-151-43        0.00    31.838    -43.343  -1.417   0.204 -  1 B6A          C2/m       1
AB-98628-151-27        0.00    16.098    -29.004  -2.704   0.210 -  1 BA2          I4/mmm     1
AB-98628-151-69       -0.00    54.371    -68.533  -0.733   0.360 -  1 BA7          Cm         1
AB-98628-151-94       -0.00    31.225    -55.335  -2.533   0.589 -  1 B5A2         P-1        1
AB-98628-151-12        0.00    24.259    -35.475  -1.557   0.628 -  1 BA3          Imm2       1
AB-98628-151-19        0.00    22.648    -45.520  -3.255   0.660 -  1 B3A2         C2/m       1
AB-98628-151-36       -0.00    31.939    -65.356  -3.368   0.677 -  1 B4A3         R-3m       1
AB-98628-151-92       -0.00    35.901    -67.436  -3.068   0.678 -  1 B3A4         Cmmm       1
AB-98628-151-24       -0.00    48.190    -58.304  -0.570   0.679 -  1 BA6          Cm         1
AB-98628-151-26        0.00    26.771    -46.922  -2.699   0.798 -  1 B2A3         C2/m       1
AB-98628-151-14       -0.00    40.615    -48.965  -0.501   0.956 -  1 BA5          C2         1
AB-98628-151-59       -0.00    44.751    -71.411  -2.137   1.141 -  1 B3A5         P1         1
```

Using `xmgrace` the `hull.agr` file generate can be examined. The end members for the convex hull calculation can be specified. Stable compounds are denoted by a "+" symbol.

```console
$ ca -m -1 BA -2 B | sort -n -k 6 -k 5

AB-98628-151-90       -0.00     4.272     -9.073  -0.881  -0.000 +  2 B2A          C2/m       1
AB-98628-151-10        0.00    34.035    -64.476  -0.868  -0.000 +  2 B3A          P63/mmc    1
AB-98628-151-86        0.00    17.153    -38.183  -0.836  -0.000 +  1 B5A3         Cmmm       1
AB-98628-151-25        0.00     4.424    -10.638   0.000   0.000 +  1 BA           Pm-3m      1
AB-98628-151-73        0.00    10.027     -8.356   0.000  -0.000 +  2 B            P63/mmc    1
AB-98628-151-98        0.00    21.904    -35.578  -0.442   0.210 -  1 B4A          I4/m       1
AB-98628-151-88       -0.00    13.443    -19.756  -0.305   0.217 -  1 B5A          Cmmm       1
AB-98628-151-43        0.00    31.838    -43.343  -0.196   0.238 -  1 B6A          C2/m       1
AB-98628-151-94       -0.00    31.225    -55.335  -0.050   0.824 -  1 B5A2         P-1        1
AB-98628-151-19        0.00    22.648    -45.520   0.404   1.100 -  1 B3A2         C2/m       1
AB-98628-151-36       -0.00    31.939    -65.356   0.663   1.185 -  1 B4A3         R-3m       1
```

Example 1.10
============

We now perform a ternary search.

```console
$ airss.pl -pp3 -max 1000 -seed ABC
```

The output can be analysed using cryan. `ca -s` will give a summary of the covered compositions, and
`ca -m` computes the convex hull. Both will generate a lot of output, so here we only consider those
compositions on the convex hull, using the `-de 0` flag, which sets the energy window to zero.

```console
$ ca -de 0 -m | sort -n -k 6 -k 5

ABC-20883-7172-331       0.00     8.849    -21.276  -4.371   0.000 +  4 BA           Pm-3m      1
ABC-20883-7172-679      -0.00    15.261    -38.136  -4.102  -0.000 +  1 CB2A         Fm-3m      1
ABC-20883-7172-932       0.00    34.558    -75.445  -3.686   0.000 +  1 B5A3         R-3m       1
ABC-20883-7172-215      -0.00    22.223    -51.280  -3.394  -0.000 +  1 C2B3A        P-3m1      1
ABC-20883-7172-880      -0.00    12.906    -26.894  -3.394   0.000 +  1 B2A          I4/mmm     1
ABC-20883-7172-887       0.00    10.612    -19.048  -2.839  -0.000 +  1 CA           Pm-3m      1
ABC-20883-7172-497      -0.00    16.996    -32.096  -2.801   0.000 +  1 B3A          I4/mmm     1
ABC-20883-7172-570      -0.00    20.333    -31.061  -1.916   0.000 +  2 C3A          Pmmn       1
ABC-20883-7172-656       0.00     7.004    -12.950  -1.879   0.000 +  1 CB           Pm-3m      1
ABC-20883-7172-383       0.00     5.013     -4.178   0.000   0.000 +  4 B            P63/mmc    1
ABC-20883-7172-948      -0.00     7.357     -8.356   0.000   0.000 +  2 A            P63/mmc    1
ABC-20883-7172-960       0.00     5.363     -5.014   0.000   0.000 +  2 C            P63/mmc    1
```

If R is installed, along with the ggtern package, then the terary convex hull can be visualised by
running `Rscript ternary.r`, and opening the resulting PDF file.

Example 1.11
============

Here we search for defects by digging a hole in a bulk crystal, and randomising the positions of the atoms in the hole. It is particularly important to visualise the output of the randomisation phase before optimisation for these more complex geometries.

```console
$ buildcell < AB.cell | cabal cell res > check-this.res
$ airss.pl -pp3 -max 100 -seed AB
$ ca -r -t 10

AB-93091-2716-27         0.00  4458.846   -1506.227   1 B4A179       Pm         1
AB-93091-2716-47         0.00  4458.846       1.067   1 B4A179       P1         1
AB-93091-2716-93         0.00  4458.846       1.128   1 B4A179       P2         1
AB-93091-2716-71         0.00  4458.846       3.643   1 B4A179       P1         1
AB-93091-2716-38         0.00  4458.846       3.643   1 B4A179       P1         1
AB-93091-2716-23         0.00  4458.846       5.003   1 B4A179       P1         1
AB-93091-2716-52         0.00  4458.846       5.003   1 B4A179       P1         1
AB-93091-2716-15         0.00  4458.846       5.003   1 B4A179       P1         1
AB-93091-2716-89         0.00  4458.846       5.023   1 B4A179       P1         1
AB-93091-2716-90         0.00  4458.846       5.143   1 B4A179       P1         1
```

It appears that a particularly low energy defect has been located which possesses some symmetry. It is clear that the search is not exhaustive, given the lack of low energy repeats.

Example 1.12
============

Here we pack a number of B atoms into a fixed A atom nanotube.

```console
$ airss.pl -pp3 -max 100 -seed AB
$ ca -r -t 10

AB-86038-5186-61         0.00   122.976      -5.629  24 BA4          P-4        1
AB-86038-5186-22         0.00   122.976       0.167  24 BA4          P1         1
AB-86038-5186-36         0.00   122.976       0.167  24 BA4          P1         1
AB-86038-5186-84         0.00   122.976       0.167  24 BA4          P1         1
AB-86038-5186-66         0.00   122.976       0.167  24 BA4          P1         1
AB-86038-5186-87         0.00   122.976       0.167  24 BA4          P1         1
AB-86038-5186-32         0.00   122.976       0.167  24 BA4          P1         1
AB-86038-5186-83         0.00   122.976       0.167  24 BA4          P1         1
AB-86038-5186-4          0.00   122.976       0.167  24 BA4          P1         1
AB-86038-5186-35         0.00   122.976       0.167  24 BA4          P1         1
```

Example 1.13
============

Here we search for an interface between the A and B binary Lennard–Jones atoms. Again, check the output
of the random structure generation before optimisation.

```console
$ airss.pl -pp3 -max 100 -seed AB
$ ca -r -t 10

AB-93141-9769-45         0.00    99.314     -32.073  24 B2A3         P21/m      1
AB-93141-9769-60         0.00    99.314       0.000  24 B2A3         P21/m      1
AB-93141-9769-43         0.00    99.314       0.000  24 B2A3         P21/m      1
AB-93141-9769-86         0.00    99.314       0.052  24 B2A3         Pmm2       1
AB-93141-9769-75         0.00    99.314       0.052  24 B2A3         Pmm2       1
AB-93141-9769-27         0.00    99.314       0.052  24 B2A3         Pmm2       1
AB-93141-9769-34         0.00    99.314       0.052  24 B2A3         Pmm2       1
AB-93141-9769-41         0.00    99.314       0.052  24 B2A3         Pmm2       1
AB-93141-9769-48         0.00    99.314       0.052  24 B2A3         Pmm2       1
AB-93141-9769-69         0.00    99.314       0.052  24 B2A3         Pmm2       1
```

A low energy, relatively high symmetry structure has been found three times this time.

Example 2.01
============

In this example we will explore the energy landscape of Carbon at 100 GPa (or 1 MBar). For comparison, the pressure at the centre of the Earth is about 350 GPa.

```console
$ cat C2.cell

%BLOCK LATTICE_CART
1.709975 0 0
0 1.709975 0
0 0 1.709975
%ENDBLOCK LATTICE_CART

#VARVOL=5

%BLOCK POSITIONS_FRAC
C 0.0 0.0 0.0 # C1 % NUM=1
C 0.0 0.0 0.0 # C2 % NUM=1
%ENDBLOCK POSITIONS_FRAC

#MINSEP=1.3

KPOINTS_MP_SPACING 0.07
```

The only AIRSS related command in the cell file is `#MINSEP=1.3`. This is not essential for the light elements, but for transition metals it is important to avoid core overlap to prevent poor convergence of the electronic structure. The k-points sampling density is 0.07, with 0.05 more appropriate for metallic (or nearly metallic) systems.

```console
$ airss.pl -press 100 -max 10 -seed C2
```

During the run `xmgrace` may be used to plot the `.conv` files in order to monitor progress.

```console
$ xmgrace *.conv
```

```console
$ ca -r

C2-90568-5971-4      100.00     4.699  -151.660  2 C            Fd-3m      1
C2-90568-5971-5      100.00     4.699     0.000  2 C            Fd-3m      1
C2-90568-5971-7      100.01     4.698     0.002  2 C            Fd-3m      1
C2-90568-5971-8       99.99     4.698     0.002  2 C            Fd-3m      1
C2-90568-5971-2       99.99     4.697     0.003  2 C            Fd-3m      1
C2-90568-5971-1       99.94     4.698     0.003  2 C            Fd-3m      1
C2-90568-5971-6      100.00     4.695     0.005  2 C            Fd-3m      1
C2-90568-5971-10     100.00     4.691     0.010  2 C            Fd-3m      1
C2-90568-5971-3      100.05     5.379     0.871  2 C            C2/m       1
C2-90568-5971-9      100.03     4.477     2.555  2 C            P1         1
```

In this very small cell, most of the structures found are the diamond structure (space group `Fd-3m`). If the search is repeated at lower pressures, for example 1 GPa, more graphitic structures will be found.

Example 2.02
============

In this example we explore hydrogen at 100 GPa. First we perform a free search.

```console
$ cat H.cell

#VARVOL=2.5
#SPECIES=H
#NATOM=8

#SLACK=0.25
#OVERLAP=0.1
#MINSEP=1 H-H=0.7
#COMPACT

KPOINTS_MP_SPACING 0.07

SYMMETRY_GENERATE
SNAP_TO_SYMMETRY

%BLOCK SPECIES_POT
QC5
%ENDBLOCK SPECIES_POT

$ airss.pl -press 100 -max 10 -seed H
$ ca -r

H-99432-8459-3       100.01     2.301     -13.670   8 H            P21/c      1
H-99432-8459-10      100.04     2.301       0.001   8 H            Pnma       1
H-99432-8459-6       100.00     2.305       0.002   8 H            C2         1
H-99432-8459-9       100.02     2.309       0.013   8 H            P1         1
H-99432-8459-5        99.95     2.334       0.015   8 H            Cmce       1
H-99432-8459-1       100.01     2.335       0.016   8 H            I41/acd    1
H-99432-8459-8        99.99     2.336       0.016   8 H            I41/acd    1
H-99432-8459-7        99.97     2.341       0.022   8 H            C2/c       1
H-99432-8459-4        99.98     2.243       0.028   8 H            Fmmm       1
H-99432-8459-2       100.00     2.172       0.098   8 H            P-1        1
```

Then we extract the molecular units that emerge, using the cryan code directly to analyse a single structure:

```console
$ cryan -g -bs 1 < H-99432-8459-3.res > H2.cell

Degrees of freedom:    14  0.737

Number of units:        4

$ cat H2.cell

%BLOCK LATTICE_CART
   1.89795   0.00000   0.00000
  -0.00132   2.97936   0.00000
   0.00189   0.00094   3.25581
%ENDBLOCK LATTICE_CART

#TARGVOL=4.60

%BLOCK POSITIONS_ABS
    H   1.43077   1.95640   3.12157 # 1-D(inf)h % NUM=1
    H   0.91703   2.16131   2.62420 # 1-D(inf)h
%ENDBLOCK POSITIONS_ABS

#SYMMOPS=1
##SGRANK=20
#NFORM=4
#SLACK=0.25
#OVERLAP=0.1
#COMPACT
#MINSEP=1.0 H-H=1.44
```

Add the lines `KPOINTS_MP_SPACING 0.07` and below from `H.cell` to your new `H2.cell`, and copy `H.param` to `H2.param`.

```console
$ airss.pl -press 100 -max 10 -seed H2
$ ca -r

H-99432-8459-3       100.01     2.301     -13.670   8 H            P21/c      1
H2-3639-9598-1       100.01     2.301       0.000   8 H            P21/c      1
H2-3639-9598-9        99.99     2.305       0.000   8 H            Pca21      1
H-99432-8459-10      100.04     2.301       0.001   8 H            Pnma       1
H2-3639-9598-7       100.04     2.304       0.002   8 H            C2         1
H-99432-8459-6       100.00     2.305       0.002   8 H            C2         1
H2-3639-9598-3        99.93     2.306       0.004   8 H            P-1        1
H-99432-8459-9       100.02     2.309       0.013   8 H            P1         1
H-99432-8459-5        99.95     2.334       0.015   8 H            Cmce       1
H2-3639-9598-8        99.98     2.334       0.016   8 H            Cmce       1
H2-3639-9598-2       100.02     2.334       0.016   8 H            Cmce       1
H2-3639-9598-5       100.01     2.335       0.016   8 H            Pnnm       1
H-99432-8459-1       100.01     2.335       0.016   8 H            I41/acd    1
H-99432-8459-8        99.99     2.336       0.016   8 H            I41/acd    1
H2-3639-9598-10       99.99     2.341       0.020   8 H            Cc         1
H-99432-8459-7        99.97     2.341       0.022   8 H            C2/c       1
H2-3639-9598-4       100.03     2.263       0.025   8 H            C2/m       1
H-99432-8459-4        99.98     2.243       0.028   8 H            Fmmm       1
H2-3639-9598-6        99.96     2.193       0.097   8 H            P-1        1
H-99432-8459-2       100.00     2.172       0.098   8 H            P-1        1
```

Example 2.03
============

This is a fixed unit cell search for γ-B, using experimental lattice parameters and 28 B atoms.

```console
$ spawn airss.pl -mpinp 4 -steps 0 -seed B
$ despawn
$ tidy.pl

Files will be removed - <ENTER> to continue

$ ca -r -t 10

B-69670-3073-1         -22.43     7.088     -79.005  28 B            Pnnm       1
B-69670-3073-6         -22.43     7.088       0.000  28 B            Pnnm       1
B-49774-4492-7         -22.43     7.088       0.000  28 B            Pnnm       1
B-58963-5042-7         -22.44     7.088       0.000  28 B            Pnnm       1
B-30179-6593-3         -22.00     7.088       0.009  28 B            P1         1
B-34447-3726-7         -19.00     7.088       0.112  28 B            P1         1
B-36890-8534-9         -19.02     7.088       0.112  28 B            P1         1
B-41988-6108-2         -23.76     7.088       0.127  28 B            P1         1
B-69681-3822-2         -23.78     7.088       0.127  28 B            P1         1
B-34459-3715-1         -24.72     7.088       0.150  28 B            P1         1

$ ca -s

B-69670-3073-1         -22.43     7.088     -79.005  28 B            Pnnm      1  1053      ~
Number of structures   :   1053
Number of compositions :      1
```

The pressures are negative as a result of the poor k-point and basis set convergence. But for a fixed cell
search the energy landscape is good enough to permit the known `Pnnm` structure of γ-B to be rapidly
located. In this case it has been located four times out of 1053 attempts. In a longer run it has been located
113 times in 21,222 attempts, suggesting a mean number of attempts to identify the ground state of 188.

Example 2.04
============

We can analyse the `Pmmn` γ-B structure identified in Example 2.3 using a modularity detection algorithm as
applied to the complex network of atomic contacts. See the following paper for more details [S. E. Ahnert, W. P. Grant, and C. J. Pickard "Revealing and exploiting hierarchical material structure through complex atomic networks."  _npj Computational Materials_, 2017, **3**, 35.](https://doi.org/10.1038/s41524-017-0035-x)

First we perform the modular decomposition.

```console
$ cryan -g -bs -1.1 < ../2.3/B-69670-3073-1.res

Iteration:   1  0
 }   0.3260869565    2
Maximum q:   2 0.32609
Iteration:   1  0
 }   0.3260869565    2
Iteration:   2  1
 }   0.3260869565    2
Iteration:   3  2
 }   0.3260869565    2

Degrees of freedom:    12  0.152

Number of units:        2

%BLOCK LATTICE_CART
   5.05440   0.00000   0.00000
   0.00000   5.61990   0.00000
   0.00000   0.00000   6.98730
%ENDBLOCK LATTICE_CART

#TARGVOL=99.24

%BLOCK POSITIONS_ABS
    B   3.91409  -0.15113   0.95482 # 1-D2h % NUM=1
    B   3.91410  -0.15061   2.71893 # 1-D2h
    B   2.28386   2.31961   0.95492 # 1-D2h
    B   1.39268   1.04217   1.83717 # 1-D2h
    B   2.31622   0.56462   3.30280 # 1-D2h
    B   4.76984  -1.62220   1.83839 # 1-D2h
    B   2.31571   0.56411   0.37147 # 1-D2h
    B   3.88218   1.60450   0.37142 # 1-D2h
    B   4.80548   1.12636   1.83718 # 1-D2h
    B   3.88189   1.60431   3.30284 # 1-D2h
    B   3.85415   2.60738   1.83745 # 1-D2h
    B   2.28415   2.31959   2.71899 # 1-D2h
    B   2.34405  -0.43858   1.83738 # 1-D2h
    B   1.42838   3.79048   1.83843 # 1-D2h
%ENDBLOCK POSITIONS_ABS

#SYMMOPS=1
##SGRANK=20
#NFORM=2
#SLACK=0.25
#OVERLAP=0.1
#COMPACT
#MINSEP=1.0 B-B=1.64
```

Using this decomposition of the structure, we can rapidly rediscover it by randomly placing these D2h units into the same fixed cell.

```console
$ cat B.cell

%BLOCK LATTICE_ABC
5.0544 5.6199 6.9873
90 90 90
#FIX
%ENDBLOCK LATTICE_ABC

%BLOCK POSITIONS_ABS
    B   2.58544   4.77680   4.32147 # 1-D2h % NUM=2
    B   1.69796   8.00288   2.85360 # 1-D2h
    B   2.61373   3.77359   2.85525 # 1-D2h
    B   4.18411   4.06190   3.73764 # 1-D2h
    B   5.07510   5.33975   2.85470 # 1-D2h
    B   4.18329   4.06211   1.97364 # 1-D2h
    B   2.55309   6.53193   1.97357 # 1-D2h
    B   2.58485   4.77663   1.39006 # 1-D2h
    B   2.55305   6.53194   3.73757 # 1-D2h
    B   5.03988   2.59079   2.85345 # 1-D2h
    B   4.15056   5.81724   1.39010 # 1-D2h
    B   1.66210   5.25490   2.85543 # 1-D2h
    B   4.12347   6.82062   2.85477 # 1-D2h
    B   4.15140   5.81769   4.32009 # 1-D2h
%ENDBLOCK POSITIONS_ABS

#SLACK=0.25
#OVERLAP=0.1
#COMPACT
#MINSEP=1.64

KPOINTS_MP_SPACING : 0.1

FIX_ALL_CELL : true

%BLOCK SPECIES_POT
B 2|1.6|7|7|9|20:21(qc=4)
%ENDBLOCK SPECIES_POT

$ spawn airss.pl -mpinp 4 -steps 0 -seed B
$ despawn
$ tidy.pl

Files will be removed - <ENTER> to continue

$ ca -r -t 10

B-31951-340-1          -22.43     7.088     -79.005  28 B            Pnnm       1
B-50523-9074-1         -22.43     7.088       0.000  28 B            Pnnm       1
B-55899-7485-2         -22.45     7.088       0.000  28 B            Pnnm       1
B-48183-3899-1         -19.27     7.088       0.131  28 B            P1         1
B-77413-4166-1         -19.68     7.088       0.133  28 B            P1         1
B-48179-3887-2         -21.96     7.088       0.144  28 B            P1         1
B-74323-1468-1         -18.67     7.088       0.165  28 B            P1         1
B-55889-7368-1         -19.42     7.088       0.166  28 B            P1         1
B-53810-2553-1         -19.66     7.088       0.173  28 B            P1         1
B-31957-225-1          -17.15     7.088       0.176  28 B            P1         1

$ ca -s

B-31951-340-1          -22.43     7.088     -79.005  28 B            Pnnm      1   187      ~
Number of structures   :    187
Number of compositions :      1
```

This time the γ-B structure was located three times out of 187 attempts.

Example 3.01
============

This example is a repeat of Example 2.1 using `-gulp` and a Tersoff potential.

````console
$ airss.pl -gulp -press 100 -max 10 -seed C2

$ ca -r

C2-41554-5032-8      100.00     4.892      -4.214   2 C            C2/m       1
C2-41554-5032-2      100.00     4.893       0.000   2 C            C2/m       1
C2-41554-5032-3      100.00     4.892       0.000   2 C            C2/m       1
C2-41554-5032-10     100.00     4.784       0.070   2 C            Fd-3m      1
C2-41554-5032-1      100.00     4.784       0.070   2 C            Fd-3m      1
C2-41554-5032-6      100.00     4.387       2.732   2 C            P-1        1
C2-41554-5032-9      100.00     4.233       2.860   2 C            P-1        1
C2-41554-5032-4      100.00     4.602       3.055   2 C            I4/mmm     1
C2-41554-5032-7      100.00     4.643       3.070   2 C            I4/mmm     1
C2-41554-5032-5      100.00     4.693       3.236   2 C            P-1        1
````

Example 3.02
============

In this example `-gulp` and a Tersoff potential are used to relax structures generated with coordination constraints.
Random 'sensible' structures with 8 carbon atoms are constructed, such that each atom is four-fold coordinated, and
the minimum bond angle encountered is 91°. This choice excludes the generation of structures with 4-fold rings.

````console
$ cat C.cell

#TARGVOL=5.6
#SPECIES=C
#NATOM=8

#SYMMOPS=1
#NFORM=1

#COMPACT
#MINSEP=1.5
#COORD=4
#MINBANGLE=91
#MAXBANGLE=180
````

Run the `airss.pl` using `-gulp`. Similar structures are unified on-the-fly.

````console
$ airss.pl -gulp -max 100 -sim 0.1 -seed C
$ ca -r

C-17050-333-94           0.00     6.408      -7.371   8 C            C2/m       1
C-17050-333-1            0.00     5.667       0.001   8 C            P63/mmc   11
C-17050-333-2            0.00     5.667       0.001   8 C            Fd-3m     57
C-17050-333-12           0.00     5.853       0.002   8 C            C2/m       4
C-17050-333-89           0.00     6.012       0.041   8 C            Cmmm       1
C-17050-333-82           0.00     7.699       0.045   8 C            Cm         1
C-17050-333-85           0.00     6.088       0.053   8 C            C2/m       1
C-17050-333-61           0.00     6.093       0.055   8 C            P-1        1
C-17050-333-68           0.00     5.806       0.114   8 C            P2/m       1
C-17050-333-45           0.00     6.197       0.118   8 C            P-1        1
C-17050-333-96           0.00     5.822       0.124   8 C            P-1        1
C-17050-333-27           0.00     5.816       0.124   8 C            C2/m       1
C-17050-333-49           0.00     5.959       0.217   8 C            P1         1
C-17050-333-3            0.00     5.776       0.235   8 C            P-1        1
C-17050-333-77           0.00     5.641       0.235   8 C            C2/m       1
C-17050-333-25           0.00     5.889       0.238   8 C            Cmmm       1
C-17050-333-81           0.00     6.094       0.457   8 C            P-1        1
C-17050-333-14           0.00     6.020       0.457   8 C            P-1        1
C-17050-333-21           0.00     6.028       0.457   8 C            P-1        1
C-17050-333-65           0.00     6.224       0.524   8 C            P1         1
C-17050-333-80           0.00     7.437       0.546   8 C            Immm       1
C-17050-333-97           0.00     5.936       0.549   8 C            P-1        1
C-17050-333-40           0.00     5.867       0.580   8 C            C2/m       1
C-17050-333-66           0.00     5.647       0.585   8 C            P1         1
C-17050-333-16           0.00     5.840       0.627   8 C            C2         2
C-17050-333-24           0.00     6.087       0.725   8 C            P1         1
C-17050-333-78           0.00     5.997       0.860   8 C            P2         1
C-17050-333-60           0.00     5.576       0.972   8 C            R-3        1
C-17050-333-36           0.00     6.051       1.068   8 C            P1         1
C-17050-333-11           0.00     5.555       1.326   8 C            C2         1
````

Now we use the cryan script (through the `ca` wrapper) to extract all the 2D structures in the set. To determine the dimensionality, a contact distance of 2 Å is chosen. The adjacency matrix generated using this contact distance is analysed, and the layered structures are highlighted.

````console
$ ca -bl 2 -d 2 -r

C-17050-333-49           0.00     5.959      -7.154   8 C            P1         1    3 2.00
C-17050-333-77           0.00     5.641       0.235   8 C            C2/m       1    1 2.00
C-17050-333-3            0.00     5.776       0.235   8 C            P-1        1    1 2.00
C-17050-333-81           0.00     6.094       0.457   8 C            P-1        1    1 2.00
C-17050-333-14           0.00     6.020       0.457   8 C            P-1        1    1 2.00
C-17050-333-21           0.00     6.028       0.457   8 C            P-1        1    1 2.00
C-17050-333-24           0.00     6.087       0.725   8 C            P1         1    1 2.00
````

Example 3.03
============

In this example we use coordination constraints to rapidly locate the icosahedral C₂₀ fullerene.

````console
$ cat C.cell

%BLOCK LATTICE_CART
20 0 0
0 20 0
0 0 20
#FIX
%ENDBLOCK LATTICE_CART

%BLOCK POSITIONS_FRAC
C 0.0 0.0 0.0 # C1 % NUM=20
%ENDBLOCK POSITIONS_FRAC

#SYMMOPS=1
#NFORM=1

#SPHERE=2.1
#POSAMP=2.5

#CLUSTER

#MINSEP=1.44
#COORD=3
#MINBANGLE=91

FIX_ALL_CELL : true
````

In the above, 20 carbon atoms are initially randomly placed in a sphere of radius 2.5 Å centered on the origin.
They are subsequently confined in a sphere of radius 2.1 Å, such that the minimum separation is 1.44 Å, and
all the atoms are connected to three others, with a minimum bond angle of 91° (so as to exclude 4-fold rings).

````console
$ airss.pl -gulp -max 10 -cluster -seed C
$ ca -r

C-68766-2151-4           0.00   400.000      -5.779  20 C            Ih         1
C-68766-2151-10          0.00   400.000       0.000  20 C            Ih         1
C-68766-2151-3           0.00   400.000       0.072  20 C            C2v        1
C-68766-2151-1           0.00   400.000       0.072  20 C            C2v        1
C-68766-2151-7           0.00   400.000       0.072  20 C            C2v        1
C-68766-2151-2           0.00   400.000       0.072  20 C            C2v        1
C-68766-2151-8           0.00   400.000       0.072  20 C            C2v        1
C-68766-2151-5           0.00   400.000       0.072  20 C            C2v        1
C-68766-2151-6           0.00   400.000       0.072  20 C            C2v        1
C-68766-2151-9           0.00   400.000       0.109  20 C            Cs         1
````

Example 3.04
============

In this example we perform a symmetry unconstrained search for SiO₂ polymorphs using the Vashishta potential.

````console
$ cat SiO2.cell

#VARVOL=35
#SPECIES=Si%NUM=1,O%NUM=2
#NFORM=2-4
#MINSEP=1.0 Si-Si=3.00 Si-O=1.60 O-O=2.58
#COMPACT
````

````console
$ airss.pl -gulp -max 100 -seed SiO2
$ ca -u 0.1 -r -t 10

SiO2-78871-6919-74       0.00    35.097     -33.285   3 SiO2         P3221      2
SiO2-78871-6919-96       0.00    36.042       0.095   4 SiO2         Fdd2       2
SiO2-78871-6919-93       0.00    36.509       0.216   4 SiO2         C2/c       2
SiO2-78871-6919-56       0.00    33.077       0.265   4 SiO2         P21/c      5
SiO2-78871-6919-7        0.00    38.882       0.271   2 SiO2         I-42d      4
SiO2-78871-6919-47       0.00    39.653       0.333   4 SiO2         Cc         1
SiO2-78871-6919-78       0.00    41.951       0.419   2 SiO2         Ima2       1
SiO2-78871-6919-63       0.00    32.581       0.461   2 SiO2         Cmcm       1
SiO2-78871-6919-10       0.00    33.569       0.479   2 SiO2         Cmc21      5
SiO2-78871-6919-89       0.00    43.827       0.493   2 SiO2         R32        1
````

Example 3.05
============

In this example we will perform a search for 2D silica structures using `-gulp` and the Vashishta potential. First take a look at the input file for `buildcell`:

````console
$ cat SiO2.cell

%BLOCK LATTICE_CART
1 0 0
0 1 0
0 0 15
#CFIX
%ENDBLOCK LATTICE_CART

#TARGVOL=50

#SLAB
#WIDTH=4.0
#SHIFT=7.5

#SPECIES=Si%NUM=1,O%NUM=2
#NFORM=4

#MINSEP=1.0 Si-Si=3.00 Si-O=1.60 O-O=2.58

#COMPACT
````

The lattice vectors are specified so as to provide a fixed length for the c-axis (15 Å). The a- and b-axis lengths (and directions) will be chosen by `buildcell` so that the overall unit cell has a volume of 50 Å³ (set by `TARGVOL=50`). A larger value for `TARGVOL` will lead to thinner layers (for a given number of atoms).

The SLAB setting instructs `buildcell` to use internal settings appropriate for this geometry. Random structures are built of 4 formula units of SiO₂, so that the `MINSEP` distances are satisfied. They are confined by hard wall planes perpendicular to the c-axis and separated by 4 Å. The centre of the resulting slab is shifted by 7.5 Å, so that it sits in the centre of the unit cell.

````
$ airss.pl -gulp -slab -max 100 -seed SiO2

The `-slab` flag is set to force gulp to perform a fixed volume calculation. If you were using CASTEP, the cell constraints would be set through the seed.cell file.

host:3.5 cjp10$  ca -r -t 10
SiO2-1291-404-74         0.00    75.000     -31.962   4 SiO2         P-1        1
SiO2-1291-404-49         0.00    74.996       0.637   4 SiO2         P1         1
SiO2-1291-404-48         0.00    75.001       0.742   4 SiO2         Cm         1
SiO2-1291-404-91         0.00    75.005       0.789   4 SiO2         Cm         1
SiO2-1291-404-9          0.00    74.996       0.817   4 SiO2         P1         1
SiO2-1291-404-72         0.00    75.012       0.831   4 SiO2         Pc         1
SiO2-1291-404-96         0.00    74.995       0.868   4 SiO2         Cm         1
SiO2-1291-404-59         0.00    75.003       0.956   4 SiO2         P-1        1
SiO2-1291-404-54         0.00    75.002       0.965   4 SiO2         P1         1
SiO2-1291-404-83         0.00    75.007       0.971   4 SiO2         P-1        1
````

Visualise your results. The lower energy structures that you find should be some variant of bilayer hexagonal silica. It is clear that 100 samples is not exhaustive, since the are no repeated low energy structures.

Example 3.06
============

In this example we search for small SiO₂ clusters using a Vashishta potential.

````console
$ cat SiO2.cell

%BLOCK LATTICE_CART
20 0 0
0 20 0
0 0 20
#FIX
%ENDBLOCK LATTICE_CART

#CLUSTER
#ELLIPSOID=2.5 0.75

#SPECIES=Si%NUM=1,O%NUM=2
#NFORM=4

#MINSEP=1.0 Si-Si=3.00 Si-O=1.60 O-O=2.58
````

A random cluster of 4 × SiO₂ is built, subject to the specified minimum distances, and confined within an ellipsoid of volume 4/3 × π × r³, where r = 2.5 Å. The second number, 0.75 in this case, describes how spherical the ellipsoids will be on average. A value of 0.0 enforces sphericity.

````console
$ airss.pl -gulp -cluster -max 10 -seed SiO2
````

The `-cluster` setting enforces a constant volume optimisation, and the point group analysis of the resulting structures.

````console
$ ca -r

SiO2-33704-5555-6        0.00  2000.000     -28.907   4 SiO2         Cs         1
SiO2-33704-5555-10       0.00  2000.000       0.002   4 SiO2         Cs         1
SiO2-33704-5555-7        0.00  2000.000       0.051   4 SiO2         C1         1
SiO2-33704-5555-5        0.00  2000.000       0.100   4 SiO2         C2v        1
SiO2-33704-5555-4        0.00  2000.000       0.100   4 SiO2         C2v        1
SiO2-33704-5555-8        0.00  2000.000       1.880   4 SiO2         C1         1
SiO2-33704-5555-3        0.00  2000.000       2.698   4 SiO2         C1         1
SiO2-33704-5555-9        0.00  2000.000       4.079   4 SiO2         C1         1
SiO2-33704-5555-2        0.00  2000.000       4.595   4 SiO2         C1         1
SiO2-33704-5555-1        0.00  2000.000      11.250   4 SiO2         C1         1
````

Example 3.07
============

In this example we search for well packed methane (CH₄) molecular crystals. The Tersoff potential is used to determine the interatomic interactions, and it is expected to provide only a coarse approximation.

````console
$ cat CH4.cell

#TARGVOL=7.69

%BLOCK POSITIONS_ABS
    C   0.86267   0.87843   0.69644 # 1-Td % NUM=1
    H  -0.15687   1.20067   0.63115 # 1-Td
    H   1.19106   0.93720   1.71422 # 1-Td
    H   0.93994  -0.13355   0.35439 # 1-Td
    H   1.47618   1.50890   0.08615 # 1-Td
%ENDBLOCK POSITIONS_ABS

#SYMMOPS=4
#SGRANK=20
#SLACK=0.25
#OVERLAP=1
##SYSTEM=Cubi
#MINSEP=1.0 C-C=2.10 C-H=1.45 H-H=1.00
````

Using this input file, `buildcell` will attempt to construct a random unit cell, choosing a space group randomly with four symmetry operators from the top 20 most common space groups for molecular crystals. The CH₄ tetrahedral unit (or molecule) is specified in absolute coordinates. All atoms are labelled in the same way (`1-Td`) to tie the atoms together into a unit. The miniumum distance constraints are first applied by shifting the molecules, using a `SLACK` parameter of 0.25 (the `MINSEP` distances are multiplied by 0.75). Once these relaxed constraints are satisfied, an optimisation using hard sphere potentials for the specified `MINSEP` distances is applied, and rotations of the unit are permitted. A final `OVERLAP` of 1 is tolerated. This may be reduced to 0.1 or lower, at a greater computational cost.

````console
$ airss.pl -gulp -press 1 -max 10 -seed CH4
````

A pressure of 1 GPa is applied to favour well-packed solutions.

````console
$ ca -r

CH4-32238-381-10         1.00     8.111     -20.738   4 CH4          P212121    1
CH4-32238-381-8          1.00     8.390       0.002   4 CH4          P212121    1
CH4-32238-381-5          1.00     8.516       0.003   4 CH4          P21        1
CH4-32238-381-9          1.00     8.606       0.003   4 CH4          Pca21      1
CH4-32238-381-2          1.00     8.538       0.003   4 CH4          Pca21      1
CH4-32238-381-3          1.00     8.660       0.003   4 CH4          Pca21      1
CH4-32238-381-4          1.00     8.881       0.005   4 CH4          P21212     1
CH4-32238-381-1          1.00     8.902       0.005   4 CH4          P21        1
CH4-32238-381-6          1.00     9.625       0.009   4 CH4          P21212     1
CH4-32238-381-7          1.00     8.980       0.730   4 CH4          P1         1
````

The ten samples above are not exhaustive. A low energy solution is known to be cubic with space group `P213`, and cubic solutions can be encouraged by uncommenting the `SYSTEM=Cubi` line in `CH4.cell`.

Example 4.01
============

In this example we will explore the energy landscape of Carbon at 100 GPa (or 1 MBar). For comparison, the pressure at the centre of the Earth is about 350 GPa.

````console
$ cat C2.cell

%BLOCK LATTICE_CART
1.709975 0 0
0 1.709975 0
0 0 1.709975
%ENDBLOCK LATTICE_CART

#VARVOL=5

%BLOCK POSITIONS_FRAC
C 0.0 0.0 0.0 # C1 % NUM=1
C 0.0 0.0 0.0 # C2 % NUM=1
%ENDBLOCK POSITIONS_FRAC

#MINSEP=1.3
````

The only AIRSS related command in the cell file is `#MINSEP=1.3`. This is not essential for the light elements, but for transition metals it is important to avoid core overlap to prevent poor convergence of the electronic structure.

The search is a repeat of that performed in Example 2.1, except this time the search will be performed using the VASP code. First create a `C2.POTCAR` file. The `airss.pl` script is used in the usual way, except the `-vasp` flag it set.

````console
$ airss.pl -vasp -press 100 -max 20 -seed C2
````

The results of the search can be ranked using the "ca" wrapper to the cryan code.

````console
$ ca -r

C2-13385-9319-6        100.00     4.545      -6.999   2 C            Fd-3m      1
C2-13385-9319-10        99.99     5.070       0.862   2 C            C2/m       1
C2-13385-9319-7        100.02     4.990       0.902   2 C            C2/m       1
C2-13385-9319-5        100.01     4.565       2.196   2 C            P2/m       1
C2-13385-9319-3        100.04     4.270       2.386   2 C            P2/m       1
C2-13385-9319-9         99.97     4.245       2.447   2 C            Pm-3m      1
C2-13385-9319-8        100.01     4.555       2.653   2 C            P-1        1
C2-13385-9319-1         99.92     4.500       2.789   2 C            P-1        1
C2-13385-9319-2        100.02     4.575       3.259   2 C            Cmcm       1
C2-13385-9319-4        100.01     4.500       3.653   2 C            P-1        1
````
