---
title: Home
layout: home
has_toc: false
nav_order: 1
---

# RINRUS: the Residue Interaction Network ResidUe Selector

{: .warning }
> Site in early development! Not guaranteed to be correct or useful yet.
> [Go back to the main RINRUS repo](https://github.com/natedey/RINRUS)


## Introduction

Residue Interaction Network-based ResidUe Selector (RINRUS) is a QM-cluster model building tool for biomolecular systems. Starting from a raw PDB file, after running a series of preparation tasks, the tool will
- select important residues for chemical reactions, and
- generate trimmed PDB files and corresponding quantum chemical input files

RINRUS is the first tool available that performs automated and algorithmic trimming and capping of enzyme models. Reproducibility is embedded into the model construction workflow, setting new community standards.

A software review paper is in preparation. For now, the best way to acknowledge RINRUS is to cite:
[DOI: 10.1016/j.bpj.2021.07.029](https://doi.org/10.1016/j.bpj.2021.07.029), [DOI: 10.1016/bs.arcc.2024.10.002](https://doi.org/10.1016/bs.arcc.2024.10.002)
and
[DOI: 10.1039/D3CP06100K](https://doi.org/10.1039/D3CP06100K)

The development of RINRUS has been supported by the National Science Foundation Division of Biological Infrastructure
(CAREER BIO-1846408) and the Department of Energy Basic Energy Sciences (SBIR DE-SC0021568).

## Installation

Clone the repository, then add `bin` to your `PATH` and library code under `lib3` to your `PYTHONPATH`. For example, in `~/git`:
``` bash
cd ~/git
git clone https://github.com/natedey/RINRUS.git
export PATH="~/git/RINRUS/bin:$PATH"
export PYTHONPATH="~/git/RINRUS/lib3:$PYTHONPATH"
```

## Python dependencies

- Python >= 3.x
- NumPy
- pymol
  - If installing via conda, it's under `-c conda-forge pymol-open-source`.
- openbabel (required for Arpeggio)
- BioPython (required for Arpeggio)
- pandas



