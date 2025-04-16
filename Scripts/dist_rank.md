---
title: dist_rank
layout: default
parent: Scripts
---

```
usage: dist_rank.py [-h] [-pdb PDBF] [-s SEED] [-satom SEEDATOM] [-type TYPEF] [-max COFF] [-noH]

Distance based selection scheme with both residue and functional group partitioning

options:
  -h, --help       show this help message and exit
  -pdb PDBF        pdb file to use
  -s, -seed SEED   seed, examples: A:300,A:301,A:302
  -satom SEEDATOM  atoms to use as center, examples: A:300:CA
  -type TYPEF      "closest" or "mass" or "avg"
  -max COFF        cutoff/max distance from ligand, default 5A
  -noH, -noh       ignore hydrogens
```