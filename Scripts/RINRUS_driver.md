---
title: RINRUS_driver
layout: default
parent: Scripts
---

```
usage: rinrus_trim2_pdb.py [-h] [-pdb R_PDB] [-s SEED] [-ra R_ATOM] [-ncres CRES] [-unfrozen UFREE] [-model METHOD] [-mustadd MUSTADD]

Trim large PDB file according to res_atoms.dat, write trimmed pdb in working directory

options:
  -h, --help        show this help message and exit
  -pdb R_PDB        protonated pdbfile
  -s, -seed SEED    Chain:Resid,Chain:Resid
  -ra R_ATOM        res_atoms file containing atom info for each residue
  -ncres CRES       Noncanonical residue information
  -unfrozen UFREE   Seed canonical residue unfrozen CA/CB, chain:Resid:CACB,chain:Resid:CA
  -model METHOD     generate one or all trimmed models, if "7" is given, then will generate the 7th model, "max" for only maximal model
  -mustadd MUSTADD  Necessary non-seed fragments ([S]ide chain, [N]-term, [C]-term) e.g. "A:7:S+C,A:8:N"
(rinrus) [dwappett@log02 opts-expanded-models]$ RINRUS_driver.py -h
usage: RINRUS_driver.py [-h] [-i DRIVER_INPUT]

generate output containing script lines from driver_input

options:
  -h, --help       show this help message and exit
  -i DRIVER_INPUT  This is the driver input file
```