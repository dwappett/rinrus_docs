---
title: xyz_to_pdb
layout: default
parent: Scripts
---

```
usage: xyz_to_pdb.py [-h] [-xyz XYZ] [-pdb PDBF] [-frame FRAME] [-name NAME]

generate pdbfiles from orca xyz file

options:
  -h, --help    show this help message and exit
  -xyz XYZ      xyz structure file
  -pdb PDBF     template pdb file
  -frame FRAME  select frame: -1 (final=default) or integer (count starts at 0) or "all"
  -name NAME    filename for output pdb
  ```