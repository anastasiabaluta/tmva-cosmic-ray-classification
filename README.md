# TMVA Multiclass Cosmic Ray Primary Classification

Multivariate classification of ultra-high-energy cosmic ray shower primaries
at the **Pierre Auger Observatory**, using the TMVA framework via PyROOT.
Four MVA methods are compared on a 4-class classification task.

---

## Physics context

Ultra-high-energy cosmic rays initiate extensive air showers in the atmosphere.
Identifying the primary particle type (the mass composition) is a central
open problem in cosmic ray physics. This project classifies air shower events
into four classes using reconstructed shower observables measured by the
hybrid detector surface detector (SD) array of the Pierre Auger Observatory.

---

## Classes and input variables

**4 primary classes:**

| Label | Description |
|---|---|
| `unconvphoton` | Unconverted photon-induced shower |
| `convphoton` | Converted photon-induced shower |
| `proton` | Proton-induced shower |
| `iron` | Iron nucleus-induced shower |

**5 input observables:**

| Variable | Description |
|---|---|
| `zen_rec` | Reconstructed zenith angle |
| `log10_sb` | Log₁₀ of the SD signal parameter Sᵦ |
| `beta` | LDF fitting parameter β |
| `stationnumber` | Number of triggered SD stations |
| `angle_B` | Angle between the reconstructed shower axis and geomagnetic field |

---

## Methods compared

All four methods are run in **multiclass mode** (`AnalysisType=multiclass`)
using ROOT 6 + PyROOT (Python 3.10). Each method was tuned independently
via hyperparameter scans before final comparison.

| Method | TMVA type | Key tuned parameters |
|---|---|---|
| BDTG | `kBDT`, `BoostType=Grad` | NTrees, MaxDepth, Shrinkage, BaggedSampleFraction |
| PDEFoam | `kPDEFoam` | nActiveCells, nSampl |
| MLP | `kMLP` | NCycles, HiddenLayers, LearningRate, EstimatorType, UseRegulator |
| DNN | `kDL` | Layout (architecture × activation), Optimizer, MaxEpochs, DropConfig |

### Best configuration found for each method

| Method | Key settings | Mean test AUC |
|---|---|---|
| BDTG | NTrees=850, MaxDepth=3, Shrinkage=0.10, BaggedSampleFraction=0.5 | 0.949 |
| PDEFoam | nActiveCells=750, nSampl=2000 | — |
| MLP | NCycles=1000, HiddenLayers=N+5,N, TrainingMethod=BP, EstimatorType=CE | 0.917 |
| **DNN** | **Layout=DENSE\|64\|RELU ×3, Optimizer=ADAM, MaxEpochs=200** | **0.912** |

**Result:** The DNN produced the most precise confusion matrix across all four
classes, with the fewest off-diagonal misclassifications.

---

## Environment

```
Python    3.10
ROOT      6.x  (with PyROOT and TMVA enabled)
pandas    ≥ 1.5
numpy     ≥ 1.23
matplotlib≥ 3.6
```
