---
title: "Examples"
layout: single
classes: wide
lang: en
lang-ref: examples
sidebar:
  nav: "docs"
---

Introduction to the examples. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Table of Contents

- [**Example 1.01**](#example-101): Lennard–Jones solid with 8 atoms
- [**Example 1.02**](#example-102): Lennard–Jones cluster with 13 atoms
- [**Example 1.03**](#example-103): Lennard–Jones cluster with 2–13 atoms
- [**Example 1.04**](#example-104): Lennard–Jones cluster with 38 atoms and symmetry
- [**Example 1.05**](#example-105): Lennard–Jones cluster with 38 atoms and relax-and-shake (RASH)
- [**Example 1.06**](#example-106): Lennard–Jones cluster LJ56, using LJ55 icosahedral core
- [**Example 1.07**](#example-107): Lennard–Jones surface with adatom
- [**Example 1.08**](#example-108): Binary Lennard–Jones with fixed composition
- [**Example 1.09**](#example-109): Binary Lennard–Jones with variable composition
- [**Example 1.10**](#example-110): Ternary Lennard–Jones with variable composition
- [**Example 1.11**](#example-111): Binary Lennard–Jones defect calculation
- [**Example 1.12**](#example-112): Binary Lennard–Jones nanowire in a nanotube
- [**Example 1.13**](#example-113): Binary Lennard–Jones interface
- [**Example 2.01**](#example-201): CASTEP free search for 2 atoms of carbon at 100 GPa
- [**Example 2.02**](#example-202): CASTEP free search for 8 atoms of hydrogen at 100 GPa, followed by molecular units
- [**Example 2.03**](#example-203): CASTEP fixed cell search for γ-B28
- [**Example 2.04**](#example-204): CASTEP fixed cell search for γ-B28 with units
- [**Example 3.01**](#example-301): Gulp free search for 2 atoms of carbon at 100 GPa, using a Tersoff potential
- [**Example 3.02**](#example-302): Gulp coordination constrained search, 8 atoms of carbon, using a Tersoff potential
- [**Example 3.03**](#example-303): Gulp coordination constrained cluster search, 20 atoms of carbon, using a Tersoff potential
- [**Example 3.04**](#example-304): Gulp symmetry unconstrained search for SiO₂ polymorphs, using a Vashishta potential
- [**Example 3.05**](#example-305): Gulp symmetry unconstrained search for layered SiO₂ structures, using a Vashishta potential
- [**Example 3.06**](#example-306): Gulp symmetry unconstrained search for small SiO₂ clusters, using a Vashishta potential
- [**Example 3.07**](#example-307): Gulp symmetry constrained search for CH₄ molecular crystals, using a Tersoff potential
- [**Example 4.01**](#example-401): VASP free search for 2 atoms of carbon at 100 GPa

## Example 1.01

In this example we will use random searching to find the ground state of a Lennard–Jones solid.

```shell
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
