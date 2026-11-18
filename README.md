# Toronto Road Safety — Predictive & Prescriptive Analytics

A data-driven framework for identifying high-risk intersections in Toronto and recommending targeted safety interventions under a user-defined budget constraint.

## Overview

This project combines machine learning and combinatorial optimization to support road safety planning for the City of Toronto Transportation Services. A composite risk score is computed for each intersection using historical collision data. Literature-backed eligibility rules then assign each intersection its most appropriate safety intervention. A binary knapsack optimization model selects the set of interventions that maximizes expected crash reductions within a given budget.

The results are presented in an interactive dashboard with a map, budget slider, and filterable priority table.

## Data

Download the Toronto Traffic Collision dataset from:

**https://data.tps.ca/datasets/bc4c72a793014a55a674984ef175a6f3_0/explore?location=43.700975%2C-79.333447%2C10**

1. Click **Download** and select **CSV**
2. Rename the file to `Traffic_Collisions_Open_Data.csv`
3. Place it in the root of this repository

## Setup

```bash
pip install -r requirements.txt
```

## Usage

Run the scripts in order:

```bash
python 1_data_cleaning.py
python 2_model.py
python 3_optimization.py
python 4_dashboard.py
```

This generates `dashboard.html` and `optimization_results.json`.

To view the dashboard locally:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000/dashboard.html` in your browser.

## Scripts

| Script | Description | Output |
|---|---|---|
| `1_data_cleaning.py` | Cleans raw collision data, engineers time-of-day features, aggregates to intersection level | `intersections_cleaned.csv` |
| `2_model.py` | Computes composite risk scores, assigns risk bands, trains Random Forest for feature importance | `intersections_with_risk.csv`, `risk_thresholds.json` |
| `3_optimization.py` | Assigns literature-based interventions, runs binary knapsack optimization | `optimization_results.json`, `optimization_results.csv` |
| `4_dashboard.py` | Generates interactive HTML dashboard | `dashboard.html` |

## Interventions

| Intervention | Crash Modification Factor (CMF) | Crash Reduction | Cost Range | Source |
|---|---|---|---|---|
| Crosswalk Visibility Enhancement | 0.60 | 40% | $10K–$25K | Chen et al., 2012 |
| Street Lighting Upgrade | 0.82 | 18% | $20K–$50K | Runyan et al., 2024 |
| High Friction Surface Treatment | 0.80 | 20% | $15K–$40K | NCHRP Report 617; Essa et al., 2025 |
| Bike Lane Installation | 0.734 | 26.6% | $50K–$100K | Avelar et al., 2021 |
| Adaptive Signal Control | 0.87 | 13% | $30K–$65K | Khattak et al., 2018 |
| Raised Median / Pedestrian Refuge | 0.685 | 31.5% | $30K–$80K | Zegeer et al., 2017 |

## Key References

- Chen et al. (2012). *Relative Effectiveness of Pedestrian Safety Countermeasures at Urban Intersections.* Crash Modification Factor ID: 4123
- Runyan et al. (2024). Crash Modification Factor (CMF) Clearinghouse — Street Lighting
- NCHRP Report 617 (2008). *Accident Modification Factors (CMFs) for Traffic Engineering and ITS Improvements.* Crash Modification Factor ID: 2259
- Essa et al. (2025). *Safety Effectiveness of HFST at Signalized Intersections in British Columbia.*
- Avelar et al. (2021). Crash Modification Factor (CMF) Clearinghouse — Bike Lane Installation
- Khattak et al. (2018). *Estimating Safety Effects of Adaptive Signal Control Technology using Crash Modification Factors.*
- Zegeer et al. (2017). *Development of Crash Modification Factors (CMFs) for Uncontrolled Pedestrian Crossing Treatments.* Crash Modification Factor ID: 9017
- FHWA Proven Safety Countermeasures (2021). highways.dot.gov/safety/proven-safety-countermeasures
- AASHTO Highway Safety Manual (2010)
