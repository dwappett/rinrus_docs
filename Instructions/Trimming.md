---
title: Model building
layout: default
parent: Instructions
nav_order: 3
---

## Trimming and capping the models

### Trimming the models

Use `rinrus_trim2_pdb.py` to generate trimmed cluster models based on the atoms/residues listed in `res_atoms.dat` (or an equivalent file). If any atom from a functional group (amino acid side chain, peptide bond, water molecule, etc) is included in `res_atoms.dat`, the entire functional group will be included; the trimming algorithm only truncates the model at alpha carbons. 

```bash
# Example usage of rinrus_trim2_pdb using a probe "res_atoms.dat" file
python3 ~/git/RINRUS/bin/rinrus_trim2_pdb.py -pdb 3bwm_h.pdb -s A:300,A:301,A:302

# Example usage of rinrus_trim2_pdb using an arpeggio "contact_counts.dat" file and making only the maximal model
python3 ~/git/RINRUS/bin/rinrus_trim2_pdb.py -pdb 3bwm_h.pdb -s A:300,A:301,A:302 -c contact_counts.dat -model max

# All arguments for rinrus_trim2_pdb
-pdb FILE       pre-processed PDB file (e.g. 3bwm_h.pdb)
-s SEED         seed fragment(s)) (e.g. A:300,A:301,A:302)
-c FILE         atom info file (default: res_atoms.dat)
-unfrozen ATS   residues to unfreeze (Chain:resID:<CA/CB/CACB>)
-model N        specify which model to create (number or 'max')
-mustadd	fragment(s) that must be in model (Chain:resID:<S/N/C/S+N/S+C/N+C/S+N+C>)
```

If no model is specified, the script will generate the entire "ladder" of possible models by adding residues based on their order in `res_atoms.dat`, otherwise only the model containing N residues will be created (or the maximal model if 'max' is specified). For each model, the files `res_N.pdb`, `res_N_froz_info.dat` and `res_N_atom_info.dat` are created. 

If canonical residues are included in the seed, you can unfreeze CA and/or CB in these residues with the unfrozen flag. Specify the carbon atoms to unfreeze as CA, CB or CACB.

The mustadd flag is used to define groups that must be in the model but are not part of the seed (e.g. metal coordinating residues or groups covalently bonded to the seed). These are included independently of what is in `res_atoms.dat` so the model numbers might double count some groups. 


### Capping the truncation sites with hydrogens

Use `pymol_protonate.py` to add capping hydrogens to each `res_N.pdb` file where bonds were broken in the trimming procedure. You must have a local copy of PyMOL installed! The script generates a `log.pml` input file containing commands that perform the hydrogen addition and then runs it in PyMOL. 
```bash
# Example usage of pymol_protonate
python3 $HOME/git/RINRUS/bin/pymol_protonate.py -pdb res_N.pdb -ignore_ids "A:300,A:301,A:302" 

# All arguments for pymol_scripts
-pdb FILE               pdb of model to be protonated
-ignore_ids CH:ID       fragment(s) to exclude from protonation (e.g. all seed fragments)
-ignore_atoms CH:ID:AT  specific atom(s) to exclude from protonation (e.g. A:25:C+O,A:25:N) 
-ignore_atnames ATOMS   atom type(s) to exclude (e.g. "ND1,NE2" to avoid changing histidine protonation. Be very careful with this!)
```

The "ignore_ids", "ignore_atoms" and "ignore_atnames" flags are used to specify residue IDs, specific atoms and atom types that should not be protonated. You will most likely want to put the seed fragments in the "ignore_ids" list, otherwise PyMOL might reprotonate your noncanonical amino acids/substrate molecules (and make very poor decisions). By default the script avoids re-protonating the end nitrogens in arginine (NH1 and NH2). If you use "ignore_ats" to avoid any other atom types generally (for example the nitrogens in histidine side chains), check your models carefully before proceeding!

### Making template model pdb files

Use `make_template_pdb.py` to reformat the protonated model pdbs. This makes the file `model_N_template.pdb` which encodes the frozen atom info into the last column of each line. Important for correctly applying constraints in the QM input files made in the next step!
```bash
# Usage of make_template_pdb with standard name formats (res_N.pdb and res_N_h.pdb)
python3 $HOME/git/RINRUS/bin/make_template_pdb.py -name res_N

# Usage of make_template_pdb by specifying both files separately
python3 $HOME/git/RINRUS/bin/make_template_pdb.py -noh res_N.pdb -addh res_N_h.pdb
```

**Next:** [Write input files for QM computations](QM_input.md)