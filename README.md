# rmd17-npz

The **Revised MD17 (rMD17)** dataset in NumPy NPZ format, with pre-made train/test splits.

**Figshare:** https://figshare.com/articles/dataset/Revised_MD17_dataset_rMD17_/12672038

**Citation:**

> Anders S. Christensen and O. Anatole von Lilienfeld (2020)
> "On the role of gradients for machine learning of molecular energies and forces"
> https://arxiv.org/abs/2007.09593

---

## About the dataset

The ten molecules are taken from the original MD17 dataset by Chmiela et al. For each molecule, 100,000 structures are selected and the energies and forces are recalculated at the **PBE/def2-SVP** level of theory using very tight SCF convergence and a very dense DFT integration grid. The dataset is therefore essentially free from numerical noise present in the original MD17.

**Warning:** As the structures come from a molecular dynamics simulation (time series data), they are not guaranteed to be independent samples. **Do not train a model on more than 1,000 samples from this dataset.**

*Note: For azobenzene, only 99,988 samples are available due to 11 failed DFT calculations; the original dataset also only contained 99,999 structures.*

---

## NPZ data keys

Each NPZ file contains the following arrays:

| Key | Shape | dtype | Description |
|---|---|---|---|
| `nuclear_charges` | `(N_atoms,)` | uint8 | Atomic numbers |
| `coords` | `(N_frames, N_atoms, 3)` | float64 | Cartesian coordinates (Å) |
| `energies` | `(N_frames,)` | float64 | Total energy (kcal/mol) |
| `forces` | `(N_frames, N_atoms, 3)` | float64 | Cartesian forces (kcal/mol/Å) |
| `old_indices` | `(N_frames,)` | int64 | Index of each frame in the original MD17 dataset |
| `old_energies` | `(N_frames,)` | float64 | Energy from the original MD17 dataset (kcal/mol) |
| `old_forces` | `(N_frames, N_atoms, 3)` | float64 | Forces from the original MD17 dataset (kcal/mol/Å) |

---

## Train/test splits

The `rmd17-npz/` folder contains pre-split NPZ files for all 10 molecules across 5 independent train/test splits. Each split contains **1,000 training frames** and **1,000 test frames**.

Files follow the naming convention:

```
rmd17-npz/rmd17_<molecule>_<train|test>_<01-05>.npz
```

### Molecules

| Molecule | N_atoms |
|---|---|
| aspirin | 21 |
| azobenzene | 24 |
| benzene | 12 |
| ethanol | 9 |
| malonaldehyde | 9 |
| naphthalene | 18 |
| paracetamol | 20 |
| salicylic | 16 |
| toluene | 15 |
| uracil | 12 |

---

## Download

Individual files can be downloaded directly with `wget` or `curl`. For example, to download the ethanol training split 01:

```bash
wget https://raw.githubusercontent.com/andersx/rmd17-npz/master/rmd17-npz/rmd17_ethanol_train_01.npz
```

or with `curl`:

```bash
curl -L https://raw.githubusercontent.com/andersx/rmd17-npz/master/rmd17-npz/rmd17_ethanol_train_01.npz -o rmd17_ethanol_train_01.npz
```

---

## Usage example

```python
import numpy as np

data = np.load("rmd17_ethanol_train_01.npz")

nuclear_charges = data["nuclear_charges"]          # (9,)
coords          = data["coords"]                   # (1000, 9, 3)
energies        = data["energies"]                 # (1000,)
forces          = data["forces"]                   # (1000, 9, 3)
```

---

## Funding

This work was partly supported by the NCCR MARVEL, funded by the Swiss National Science Foundation. O.A.v.L. acknowledges funding from the Swiss National Science Foundation (407540_167186 NFP 75 Big Data) and from the European Research Council (ERC-CoG grant QML).
