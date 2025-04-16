---
title: Driver
---

# How to make QM-cluster models and input files with RINRUS

The RINRUS workflow can be run all in one go with the driver or done step by step with the individual scripts. Both ways of using RINRUS are described in this document.

# Using the driver
The driver runs the entire RINRUS model building procedure from initial PDB structure to input file with one command (and a few interactive inputs along the way). The structure pre-processing described below in part 1 of the step-by-step usage instructions needs to be done before using the driver (except for protonation with reduce).

```bash
# Usage of the RINRUS driver
python $HOME/git/RINRUS/bin/RINRUS_driver.py -i driver_input

# Arguments:
-i FILE    rinrus driver input file (default: driver_input)
``` 

### Input file
The `driver_input` file should contain the following details:
```
Path_to_scripts: [path to RINRUS bin directory]
PDB: [starting pdb file name]
Protonate_initial: [true/t/y or false/f/n]
Seed: [seed residues]
RIN_program: [probe/arpeggio/distance/manual]
Model(s): [model number or maximal or all]
Seed_charge: [overall charge of seed]
Multiplicity: [model multiplicity]
Computational_program: [gaussian/gau-xtb/orca/qchem/none]
# optional lines:
Must_include: [any non-seed fragments (SC, N-term. and/or C-term. of residue) that need to be in model, e.g. A:6:C,A:7:S+C+N,A:8:N]
RIN_info_file: [file to use with manual RIN input, default 'res_atoms.dat']
input_template_path: [path to input file template if not using standard templates]
Gaussian_basis_intmp: [true/t/y to use basis set info from Gaussian template rather than default basis sets]
```

### Log file
The driver writes a log file called `rinrus_log_[date].out` with the details of the run/commands called. These can include:

```bash
# If reduce protonation selected
$HOME/git/RINRUS/bin/reduce -NOFLIP -Quiet PDB.pdb > PDB_h.pdb 

# If probe RIN selected:
$HOME/git/RINRUS/bin/probe -unformated -MC -self "all" -Quiet PDB_h.pdb > PDB.probe
$HOME/git/RINRUS/bin/probe2rins.py -f PDB.probe -s [seed residues]

# If arpeggio RIN selected
$HOME/git/RINRUS/bin/arpeggio/arpeggio.py PDB_h.pdb
$HOME/git/RINRUS/bin/arpeggio2rins.py -f PDB.contacts -s [seed residues]

# If distance RIN selected
$HOME/git/RINRUS/bin/pdb_dist_rank.py -pdb PDB_h.pdb -s [seed residues] -cut [cutoff] -type [avg/mass]

# Model trimming and capping
$HOME/git/RINRUS/bin/rinrus_trim2_pdb.py -s [seed residues] -pdb PDB_h.pdb -model N -c [res_atoms.dat/contact_counts.dat/res_atom-X.dat depending on RIN program] -mustadd [must_include fragments]
$HOME/git/RINRUS/bin/pymol_protonate.py -pdb res_N.pdb (if specified: -ignore_ids residues) (if specified: -ignore_atoms atoms) (if specified: -ignore_atnames atomtypes)
$HOME/git/RINRUS/bin/make_template_pdb.py -name res_N

# Input file creation (if computational program not set to 'none')
$HOME/git/RINRUS/bin/write_input.py -format computational_program -c seed_charge -type hopt -pdb model_N_template.pdb (if specified: -intmp input_template_path) (if specified: -basisinfo intmp)
```