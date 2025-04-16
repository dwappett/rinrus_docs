---
title: Debugging
layout: home
nav_order: 5
---

# Main to-do
1. PSI4 templates can only set up SAPT/FSAPT/ISAPT input files. Geometry optimization templates need to make and tested
2. An approach that uses building schemes while including charged residues within a certain distance to seed
3. Interface with MBE codes
4. Interface with EnzyHTP
5. Formatting, plotting, and nomenclature standards

## problem cases where RINRUS may not behave correctly
1. Canonical residues in seed/ligand needs to be checked (C_alpha atoms should not be frozen, polypeptides and termini should be trimmed properly)
2. Salt bridges are not automatically added
3. Coding the automatic and correct treatment of cysteine S-S bridges is in progress
4. N or C-terminus residue could have a mis-named proton on carboxylic acid (MOE names is H which clashes with the N-H proton name)
5. RINRUS cannot automatically unfold symmetry of old multimeric PDB files - user may be on the hook for this anyway, since it's a preprocessing thing. TJ wrote a script to do this. I don't see it in the repo though
6. RINRUS may not correctly calculate seed charge if charged canonical amino acids are included in the seed
7. If # of waters (from MD simulation) exceeds 10,000, there can be issues with RINRUS. VMD will start printing the res# values in hexadecimal format, which will crash RINRUS. We have two scripts in /preprocessing to deal with this

## things in my list that Domi may already have fixed
1. Recognizing or ignoring duplicate atom types or res id #s in the pdb file
2. Canonical amino acids with invalid atom types
3. Listing residues/fragments that aren’t in seed but must be included in all models (possible both manually and with the driver)
4. Poorly justified element names truncating 2nd character of 2-character elements
5. Improved documentation for frozen atom algorithm I wrote this a long time ago, but I don't remember the context: "frozen atom code has a slightly complicated interaction if MC or SC or both are in the model for carbon betas"
6. Documentation for manually creating res_atoms.dat
