---
title: write_input
layout: default
parent: Scripts
---

```
usage: write_input.py [-h] [-type STEP] [-m MULTIPLICITY] [-c LIGAND_CHARGE] [-format FMAT] [-intmp INPUT_TMP] 
                    [-inpn INP_NAME] [-basisinfo BASISINFO] [-wdir OUTPUT_DIR] [-pdb NEW_PDB] [-noh NO_H_PDB] [-adh H_ADD_PDB] 
                    [-tmp TMP_PDB] [-outf GAU_OUT] [-ckp CHECK_POINT] [-pdb1 PDB1] [-pdb2 PDB2] [-parts PARTS] [-seed SEED]

Prepare template PDB files, write input files, save output PDB files in working directory

options:
  -h, --help            show this help message and exit
  -type STEP            hopt: read noh, addh pdbs, write_final_pdb and read input_template write_first_inp,
                        gauout: read outputwrite_modred_inp, input_template, write_second_inp,
                        pdb: read new pdb file, input_template, write_new_inp
                        replacecoords: read pdb1 and pdb2 for replacing the fragment in pdb1 with pdb2 coordinates and write new input
                        fsapt: F-SAPT0 calculation (psi4 only, ignores format specification)
  -m MULTIPLICITY       multiplicity
  -c LIGAND_CHARGE      charge_of_ligand
  -format FMAT          input_file_format eg.'gaussian','qchem','gau-xtb'
  -intmp INPUT_TMP      template_for_write_input
  -inpn INP_NAME        input_name
  -basisinfo BASISINFO  'intmp' if in template file, use dictionary otherwise
  -wdir OUTPUT_DIR      working dir
  -pdb NEW_PDB          new_pdb_file
  -noh NO_H_PDB         trimmed_pdb_file
  -adh H_ADD_PDB        hadded_pdb_file
  -tmp TMP_PDB          template_pdb_file
  -outf GAU_OUT         output_name
  -ckp CHECK_POINT      check_file_frame
  -pdb1 PDB1            minima_pdb_file
  -pdb2 PDB2            ts_pdb_file
  -parts PARTS          ts_frag_indo
  -seed SEED            seed
```