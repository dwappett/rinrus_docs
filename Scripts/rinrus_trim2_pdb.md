---
title: rinrus_trim2_pdb
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
```

how code works:
```
- read args
- process non-canonical res info
- process unfrozen atoms
- read pdb
- process seed
- read in res_atoms, remove comment lines
- loop through pdb create pdb_res_name and pdb_res_atom dictionaries - pdb_res_atom contains all atoms in pdb for given resid
- get max and min size of models based on no. seed fragments, no. must_add groups, length of res_atoms
- run trimming function on model(s)
  - for seed lines of res_atoms, add atoms to res_part_list dictionary - dict is like {(A,203): ['C1','O1','C2','O2']} etc for each resid
  - add must_add groups to res_part_list
  - add lines of res_atoms to res_part_list until size of res_part_list is desired model size
  - check atoms in each entry of res_part_list
    - if seed or water, add all atoms from that group in original pdb to model atom info. frozen atom list is empty.
    - otherwise:
      - run check_sc function
        - if res is proline, include all atoms from standard rinrus res atom dict
        - if res is in standard res dict and has SC atoms, add all SC atoms
        - if res is in non-canonical res dict and has SC atoms, add all SC atoms
        - if res is not recognized, use original atom list from res_atoms and print warning
      - if any side chain atoms, make sure CA/HA in there and freeze CA
      - if no SC atoms, complete relevant part of MC and make sure CA/HA in there
  - complete peptide bonds
    - checks that group is not seed and not water and res+1/res-1 are not water either
    - if N and H in there, make sure C,O,CA,HA of previous residue are in there
    - if C and O in there, make sure N,H,CA,HA of next residue are in there
      - if next residue is proline, make sure to add whole proline and add N,H,CA,HA of res+2 to complete that peptide bond too
  - check for disconnected adjacent CAs, if found then add peptide bond between
  - if residue has only CA/HA or is alanine side chain
    - add peptide bond on both sides up to CAs
    - check if this has made more disconnected adjacent CAs, if yes then connect
  - check for any partial prolines (from adding peptide bonds to connect CA-CA or anchor CAs/ala SC)
    - if found, add SC and MC if necessary.
    - again check if this has made more disconnected adjacent CAs and connect if needed
    - repeat this check until no new groups added and everything completed/connected as they should be
  - check frozen info
    - make sure all CAs are frozen
    - freeze CB for ['ARG','LYS','GLU','GLN','MET','TRP','TYR','PHE']
    - unfreeze any atoms specified to be unfrozen
  - write final atom lists to res_N_atom_info.dat
  - write final frozen lists to res_N_froz_info.dat
  - write res_N.pdb
```
