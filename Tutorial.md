---
title: Quickstart tutorial
layout: default
nav_order: 2
---

# RINRUS quickstart tutorial

This tutorial is a simplified version of the [instructions pages](Instructions/index.md) to get you started with RINRUS. It shows how to run the RINRUS model building procedure in two ways: with the driver (recommended) or with the scripts individually. 

The pre-processed PDB:2CHT structure `2cht_h_corrected_new.clean.pdb` can be downloaded from [the examples directory in the GitHub repo](https://github.com/natedey/RINRUS/blob/master/examples/2025-NEW-EXAMPLES/2cht_h_corrected_new.clean.pdb)

# Using the driver

Create a text file called `rinrus.inp` containing one of the following sets of input options:
- To build models from the probe RIN:
    ```
    pdb: 2cht_h_corrected_new.clean.pdb
    seed: A:203
    rin_program: probe
    model: max
    model_prot_ignore_ids: A:203
    qm_input_format: orca
    ```
- To build models from the arpeggio RIN:
    ```
    pdb: 2cht_h_corrected_new.clean.pdb
    seed: A:203
    rin_program: arpeggio
    arpeggio_rank: counts
    model: max
    model_prot_ignore_ids: A:203
    qm_input_format: orca
    ```
- To build models based on distance (e.g. atoms within 5A of the closest seed atom):
    ```
    pdb: 2cht_h_corrected_new.clean.pdb
    seed: A:203
    rin_program: distance
    dist_type: closest
    dist_max: 5
    model: max
    model_prot_ignore_ids: A:203
    qm_input_format: orca
    ```

Then run the driver with your input file:
```
python3 ~/git/RINRUS/bin/RINRUS_driver.py -i rinrus.inp
```

# Running the scripts individually

## Step 1: Selecting the active site/generating the active site RIN

The active site RIN can be determined from the seed fragments based on probe contacts, arpeggio contacts or distance. Pick one of these options:

**Probe:**
- Run `probe` on the (modified) PDB file to generate a *.probe file of all contacts in the enzyme. Then extract the active site RIN from the probe contacts with `probe2rins.py`. 
    ```
    ~/git/RINRUS/bin/probe -unformated -MC -self "all" 2cht_h_corrected_new.clean.pdb > 2cht_h_corrected_new.clean.probe

    python3 ~/git/RINRUS/bin/probe2rins.py -f 2cht_h_corrected_new.clean.probe -s A:203
    ```
**Arpeggio:**
- Run `arpeggio` on the (modified) PDB file to generate a *.contacts file of all contacts in the enzyme. Then extract the active site RIN from the arpeggio contacts with `arpeggio2rins.py`. 
    ```
    python3 ~/git/RINRUS/bin/arpeggio/arpeggio.py 2cht_h_corrected_new.clean.pdb

    python3 ~/git/RINRUS/bin/arpeggio2rins.py -f 2cht_h_corrected_new.clean.contacts -s A:203
    ```
**Distance:**
- Use `dist_rank.py` to select all fragments with any atoms within a cutoff radius of the seed (here doing atoms within 5A of the closest seed atom)
    ```
    python3 ~/git/RINRUS/bin/dist_rank.py -pdb 2cht_h_corrected_new.clean.pdb -s A:203 -max 5 -type closest
    ```


## Step 2: Structure trimming and capping

Run `rinrus_trim2_pdb.py` to trim the protein structure based on the atoms/functional groups selected in `res_atoms.dat`. Here we are making the "maximal" model with all selected groups included. 

```bash
python3 ~/git/RINRUS/bin/rinrus_trim2_pdb.py -pdb 2cht_h_corrected_new.clean.pdb -s A:203 -model maximal
```

Then use `pymol_protonate.py` to cap the truncation sites with hydrogens. You must have a local copy of PyMOL installed! If you used arpeggio or distance ranking, you might have to change the model PDB filename in this command to match the size of those models.
```bash
python3 ~/git/RINRUS/bin/pymol_protonate.py -pdb res_14.pdb -ignore_ids A:203 
```

Make the template model PDB file with `make_template_pdb.py`. The template PDB file is the capped model with the atom constraints encoded into it. Again here the model number might need to be changed for arpeggio/distance-based models.
```
python3 ~/git/RINRUS/bin/make_template_pdb.py -model 14
```

## Step 3: Preparing input files for QM computations

Use `write_input.py` to generate input files for quantum chemistry packages. Again, make sure that the correct model PDB filename is used. 

- Create a gaussian input file:
    ```
    python3 ~/git/RINRUS/bin/write_input.py -pdb model_14_template.pdb -c -2 -format gaussian -inpn gau.inp
    ```
- Create an ORCA input file:
    ```
    python3 ~/git/RINRUS/bin/write_input.py -pdb model_14_template.pdb -c -2 -format orca -inpn orca.inp
    ```
- Create a Psi4 F-SAPT input file:
    ```
    python3 ~/git/RINRUS/bin/write_input.py -pdb model_14_template.pdb -c -2 -format psi4-fsapt -seed A:203 -inpn psi4-input.dat
    ```

