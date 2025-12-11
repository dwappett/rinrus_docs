---
title: Reduce
layout: default
parent: Scripts
---

# Reduce

Reduce ([DOI:10.1006/jmbi.1998.2401](https://doi.org/10.1006/jmbi.1998.2401)) is a protonation software made for use with Probe. Packaged with RINRUS because that's what we use for protonating initial PDB structures.

Reduce can flip orientation of certain residues when protonating. We avoid this with NOFLIP option.

Reduce uses the PDB HET ligand dictionary (provided with RINRUS as reduce_wwPDB_het_dict.txt) to protonate non-canonical residues, small molecules, ligands etc.

## Usage:

```bash
$HOME/git/RINRUS/bin/reduce -NOFLIP 3bwm.pdb > 3bwm_h.pdb
```

## Inputs:

- Unprotonated PDB file

## Outputs:

- Protonated PDB file