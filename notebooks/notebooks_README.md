# notebooks

This folder contains all Jupyter notebooks for data extraction, merging, and exploratory data analysis.

## Files

| Notebook | Description |
|----------|-------------|
| FEMA data extraction notebook | Pulls and cleans FEMA OpenFEMA Individual Assistance data |
| Census data extraction notebook | Pulls and cleans US Census ACS socioeconomic data |
| Master merge notebook | Merges FEMA and Census data into the master dataset |
| `EDAandVisualization.ipynb` | Full exploratory data analysis with all visualizations |

## Running Order

The notebooks should be run in this order if you are reproducing the dataset from scratch:

1. FEMA extraction
2. Census extraction
3. Master merge
4. EDA and visualization

## Key EDA Findings

- `total_applicants` has the strongest correlation with total assistance at r = 0.87
- `poverty_rate` has near-zero correlation with total assistance at r = 0.02
- 2020 accounts for approximately 85% of all records due to COVID-19 declarations and hurricane activity
- Louisiana counties dominate the top per-capita assistance rankings

## Environment

All notebooks were developed and run in Google Colab. They can also be run locally with the packages listed in `requirements.txt`.
