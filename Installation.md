---
title: Installation
layout: default
nav_order: 2
---

# Installation

Clone the repository, then add `bin` to your `PATH` and library code under `lib3` to your `PYTHONPATH`. For example, to install in `~/git`:
``` bash
cd ~/git
git clone https://github.com/natedey/RINRUS.git
export PATH="~/git/RINRUS/bin:$PATH"
export PYTHONPATH="~/git/RINRUS/lib3:$PYTHONPATH"
```

# Python dependencies

- Python >= 3.x
- NumPy
- Pandas
- PyMOL
  - If installing via conda, it's under `-c conda-forge pymol-open-source`.
- OpenBabel (required for Arpeggio)
- BioPython (required for Arpeggio)

# External dependencies

Precompiled binaries of Probe, Reduce, and Arpeggio are present in `bin/`.

- [Probe](https://github.com/rlabduke/probe) - version 2.16.130520 is packaged with RINRUS
- [Reduce](https://github.com/rlabduke/reduce) - version 3.23 is packaged with RINRUS
- [Arpeggio](https://github.com/harryjubb/arpeggio) - packaged with RINRUS

These require:
- obabel version = openbabel/2.4.1
- CMake >= 3.10
- Any C/C++ compiler suite with C++11 support

PDB HET groups (some ligands and noncanonical amino acids) can be protonated by Reduce. 
The connectivity table file is included with RINRUS: `bin/reduce_wwPDB_het_dict.txt`
You must set a shell environment variable to allow reduce to use the reduce_wwPDB_het_dict file, as the default location is /local:

```bash
setenv REDUCE_HET_DICT /home/$USER/git/RINRUS/bin/reduce_wwPDB_het_dict.txt 
```
or
```bash
export REDUCE_HET_DICT=/home/$USER/git/RINRUS/bin/reduce_wwPDB_het_dict.txt
```