---
title: Driver
layout: default
nav_order: 2
---

# Using the RINRUS driver

The driver runs the entire RINRUS model building procedure from initial PDB structure to input file with one command (and potentially a few interactive inputs along the way). The structure pre-processing needs to be done before using the driver (except for protonation with reduce, although we strongly recommend doing that beforehand as well so the results can be checked).

## Calling the driver:

```bash
# Usage of the RINRUS driver
python $HOME/git/RINRUS/bin/RINRUS_driver.py 

# Optional arguments:
-i FILE    rinrus driver input file (default: rinrus.inp)
``` 

## Input file

A template `rinrus.inp` file is provided [here](https://github.com/natedey/RINRUS/blob/master/template_files/rinrus.inp).

The `rinrus.inp` file must contain the following options:

| keyword | value | function argument defined |
|:--------|:------|:---------|
| PDB | starting PDB filename | `-pdb` for most functions |
| seed | ch:ID identifiers | `-s` for most functions |
| RIN_program | probe/arpeggio/distance/manual | |
| model | all/max/maximal/[integer] | `-model` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.md) |
| qm_input_format | gaussian/gau-xtb/orca/qchem/psi4-fsapt/none | `-format` for [write_input.py](../Scripts/write_input.md) |

<br />

The following optional keywords can also be read from `rinrus.inp`:

| keyword | value | function argument defined |
|:--------|:------|:---------|
| path_to_scripts | path to RINRUS bin directory | |
| protonate_initial | true/t/y or false/f/n | |
| res_atoms_file | res_atoms file to use with "RIN_program: manual" | `-ra` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.md) |
| arpeggio_proximal | true/t/y or false/f/n | `-prox` for [arpeggio2rins.py](../Scripts/arpeggio2rins.md) |
| arpeggio_rank | counts/types | `-ra` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.md) |
| dist_type | closest/mass/avg | `-type` for [dist_rank.py](../Scripts/dist_rank.md)
| dist_satom | seed atoms for distance calculations | `-satom` for [dist_rank.py](../Scripts/dist_rank.md) |
| dist_max| distance cutoff in Angstroms | `-max` for [dist_rank.py](../Scripts/dist_rank.md)
| dist_noH | true/t/y or false/f/n | `-noH` for [dist_rank.py](../Scripts/dist_rank.md)
| must_add | ch:ID[:S/C/N] of non-seed fragments that need to be in model | `-mustadd` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.md) |
| unfrozen | ch:ID[:CA/CB] of atoms to avoid constraining | `-unfrozen` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.md) |
| nc_res_info | non-canonical residue info file | `-ncres` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.md) |
| model_prot_ignore_ids | ch:ID identifiers | `-ignore_ids` for [pymol_protonate.py](../Scripts/pymol_protonate.md) |
| model_prot_ignore_atoms | ch:ID:atom identifiers | `-ignore_atoms` for [pymol_protonate.py](../Scripts/pymol_protonate.md) |
| model_prot_ignore_atnames | atom names | `-ignore_atnames` for [pymol_protonate.py](../Scripts/pymol_protonate.md) |
| QM_input_template | path to input template | `-intmp` for [write_input.py](../Scripts/write_input.md) |
| Gaussian_basis_intmp | true/t/y or false/f/n | `-basisinfo` for [write_input.py](../Scripts/write_input.md)
| QM_calc_hopt | true/t/y or false/f/n | `-type hopt` for [write_input.py](../Scripts/write_input.md)
| seed_charge | charge of seed fragment(s) | `-c` for [write_input.py](../Scripts/write_input.md)
| multiplicity | total spin multiplicity | `-m` for [write_input.py](../Scripts/write_input.md)
| fsapt_fA | ch:ID identifiers | `-seed` for [write_input.py](../Scripts/write_input.md)



## Log file
The driver writes a log file called `rinrus_log_[date].out` which contains the details of the run:
- Header containing the github commit tag so that building procedure can be reproduced even if code is later changed
- Python executable being used (for troubleshooting errors with python packages)
- The raw contents of the driver input file
- The options selected from parsing the driver input file
- The commands run by the driver in each step of the model building procedure