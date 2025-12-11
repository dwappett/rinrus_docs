---
title: arpeggio2rins
layout: default
parent: Scripts
---

# arpeggio2rins.py

`arpeggio2rins.py` analyses an Arpeggio output file, extracts all contacts involving any seed atoms, and prepares `res_atoms.dat` files for the model trimming.

```
usage: arpeggio2rins.py [-h] [-p OPT] [-f CF] [-s SEED]

Get contact information from Arpeggio output

options:
  -h, --help  show this help message and exit
  -p OPT      ignore proximal or not
  -f CF       Arpeggio contact output file
  -s SEED     Seed residues, in the format "A:601,A:602"
```