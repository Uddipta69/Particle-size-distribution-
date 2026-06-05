# Soil Classification – USCS (Unified Soil Classification System)

A Jupyter notebook that classifies soil according to **ASTM D2487 (USCS)**
using sieve analysis data and Atterberg limits.

---

## What does this notebook do?

Given lab measurements, the notebook:

1. Computes the **grain-size distribution** (% passing each sieve)
2. Plots the **Grain-Size Distribution Curve** (semi-log scale)
3. Interpolates **D10, D30, D60** and computes **Cu** and **Cc**
4. Reads **Liquid Limit (LL)** and **Plastic Limit (PL)**, computes **PI**
5. Plots the **Casagrande Plasticity Chart** with A-line and U-line
6. Applies the full **USCS Decision Tree** and prints the classification symbol

---

## USCS Classification Overview

```
% Fines (passing 0.075 mm sieve)
│
├── < 50 %  →  COARSE-GRAINED
│   │
│   ├── > 50 % of coarse on #4 (4.75 mm)  →  GRAVEL (G)
│   │   ├── Fines < 5 %   →  Cu ≥ 4 and 1 ≤ Cc ≤ 3  →  GW  (Well-Graded Gravel)
│   │   │                    otherwise                →  GP  (Poorly-Graded Gravel)
│   │   ├── Fines 5–12 %  →  Dual symbol  (GW-GM, GP-GC, …)
│   │   └── Fines > 12 %  →  Above A-line & PI ≥ 7   →  GC  (Clayey Gravel)
│   │                         otherwise               →  GM  (Silty Gravel)
│   │
│   └── ≤ 50 % of coarse on #4  →  SAND (S)
│       ├── Fines < 5 %   →  Cu ≥ 6 and 1 ≤ Cc ≤ 3  →  SW  (Well-Graded Sand)
│       │                    otherwise                →  SP  (Poorly-Graded Sand)
│       ├── Fines 5–12 %  →  Dual symbol  (SW-SM, SP-SC, …)
│       └── Fines > 12 %  →  Above A-line & PI ≥ 7   →  SC  (Clayey Sand)
│                             otherwise               →  SM  (Silty Sand)
│
└── ≥ 50 %  →  FINE-GRAINED
    ├── LL < 50  →  Above A-line & PI ≥ 7  →  CL   (Lean Clay)
    │             │  PI 4–7 (borderline)  →  CL-ML
    │             └  otherwise            →  ML   (Silt)
    └── LL ≥ 50  →  Above A-line          →  CH   (Fat Clay)
                    otherwise             →  MH   (Elastic Silt)
```

---

## Project Structure

```
soil-classifier/
│
├── data/
│   ├── soil.xlsx           ← Sieve analysis input
│   └── atterberg.xlsx      ← Atterberg limits input
│
├── soil_classifier.ipynb   ← Main notebook
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Input Files

### `data/soil.xlsx` – Sieve Analysis

| Column | Unit | Description |
|---|---|---|
| `Sieve Size (mm)` | mm | Opening size of sieve (use 0 for the pan) |
| `Mass Retained (g)` | g | Mass of soil retained on this sieve |

**Standard USCS sieves used:**

| Sieve | Size (mm) |
|---|---|
| #4  | 4.750 |
| #10 | 2.000 |
| #40 | 0.425 |
| #200 | 0.075 |
| Pan | 0.000 |

### `data/atterberg.xlsx` – Atterberg Limits

| Parameter | Value | Unit |
|---|---|---|
| Liquid Limit (LL) | 45 | % |
| Plastic Limit (PL) | 25 | % |
| Total Initial Mass | 1000 | g |

---

## Getting Started

### Option A – Google Colab (no installation)

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. **File → Upload notebook** → upload `soil_classifier.ipynb`
3. Upload both `.xlsx` files to the Colab files panel
4. Update `SIEVE_PATH` and `ATTERBERG_PATH` in the notebook to match
5. **Runtime → Run all**

### Option B – Run Locally

```bash
git clone https://github.com/<your-username>/soil-classifier.git
cd soil-classifier

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

pip install -r requirements.txt
jupyter notebook soil_classifier.ipynb
```

---

## Output

| File | Description |
|---|---|
| `grain_size_curve.png` | Semi-log grain-size distribution with D10/D30/D60 marked |
| `plasticity_chart.png` | Casagrande chart with A-line, U-line, and sample point |
| Printed table | Full sieve analysis with % retained and % passing |
| Printed result | USCS symbol + description + all key parameters |

---

## Requirements

| Package | Purpose |
|---|---|
| `pandas` | Reading Excel and tabular calculations |
| `openpyxl` | Excel engine for pandas |
| `numpy` | Numerical interpolation |
| `matplotlib` | Plotting |
| `scipy` | (optional – for future curve fitting) |

---

## Limitations

- Designed for **rectangular / standard sieve sets** only.
- Does not handle **organic soils** (OL / OH) – requires odour/colour tests not
  available from the data alone.
- Hydrometer analysis (for particle sizes < 0.075 mm) is not included.

---

## Author

*Your Name Here* – open an issue or pull request for improvements.
