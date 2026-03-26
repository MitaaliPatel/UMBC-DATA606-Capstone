# UMBC-DATA606-Capstone

# Disaster Resource Allocation Using Data Analytics

**Prepared for:** UMBC Data Science Master Degree Capstone by Dr. Chaojie (Jay) Wang

**Author:** Mitaali Patel

**Links:**
- [GitHub Repository](https://github.com/MitaaliPatel/UMBC-DATA606-Capstone)
- [LinkedIn Profile](https://www.linkedin.com/in/mitaali-patel)
- PowerPoint Presentation: *Coming soon*
- YouTube Video: *Coming soon*

---

## Overview

This project analyzes FEMA disaster assistance patterns across US counties from 2019 to 2022 and examines how community-level socioeconomic and demographic factors relate to disaster vulnerability and resource demand. The goal is to build predictive models that can help estimate housing and financial assistance needs based on historical disaster data and Census indicators, and to present findings through an interactive Streamlit dashboard.

---

## Research Questions

1. How do disaster type, location, and severity relate to the amount of assistance requested and approved?
2. Which demographic and socioeconomic factors are linked to higher disaster vulnerability and resource demand?
3. Can historical disaster and population data predict housing assistance needs?
4. What features best explain disaster impact and emergency resource allocation?

---

## Repository Structure

```
UMBC-DATA606-Capstone/
    app/                        Streamlit web application (app.py)
    notebooks/                  Jupyter notebooks for EDA and analysis
    docs/                       Project documentation and reports
    dataset/                    See note below on data files
    models/                     Model training scripts (train_models.py)
    requirements.txt            Python dependencies
    README.md                   This file
    proposal.md                 Project proposal
    resume.md                   Author resume
```

---

## Data

### Note on Dataset Files

The raw datasets are too large to store on GitHub. They are stored locally and available upon request. The datasets used in this project are:

| File | Source | Description |
|------|--------|-------------|
| `fema_assistance_yearly_summary.csv` | [FEMA OpenFEMA](https://www.fema.gov/about/openfema/data-sets) | Yearly FEMA disaster assistance data by county (2019-2022) |
| `acs_county_social_economic.csv` | [US Census Bureau ACS](https://data.census.gov/) | County-level socioeconomic indicators (2019-2022) |
| `master_disaster_data.csv` | Merged dataset | Combined FEMA and Census data at county-year level |

### Data Summary

| Dataset | Rows | Columns | Time Period |
|---------|------|---------|-------------|
| FEMA Assistance | 3,794 | 6 | 2019-2022 |
| ACS Socioeconomic | 16,106 | 8 | 2019-2022 |
| Master (Merged) | 3,611 | 12 | 2019-2022 |

Each row in the master dataset represents one **county in one year**.

### Key Variables

| Column | Type | Description |
|--------|------|-------------|
| `state_clean` | Categorical | US state abbreviation |
| `county_clean` | Categorical | County name |
| `Year` | Integer | Year of record (2019-2022) |
| `total_population` | Integer | Total county population |
| `median_income` | Integer | Median household income ($) |
| `poverty_rate` | Float | Percentage of population below poverty line |
| `poverty_count` | Integer | Number of residents below poverty line |
| `total_applicants` | Integer | Number of FEMA assistance applicants |
| `housing_assistance_amount` | Float | Total housing assistance disbursed ($) |
| `total_assistance_amount` | Float | Total FEMA assistance disbursed ($) |

**Target variable:** `total_assistance_amount`

**Features used for modeling:** `total_population`, `median_income`, `poverty_rate`, `poverty_count`, `total_applicants`, `housing_assistance_amount`, and engineered features including `assistance_per_capita`, `vulnerability_index`, and `log` transforms.

---

## Exploratory Data Analysis

EDA was performed in Jupyter Notebook and covers:

- Correlation analysis between socioeconomic factors and FEMA assistance amounts
- Geographic analysis of top states by total assistance
- Temporal trends in applicant volume and assistance disbursement (2019-2022)
- Poverty tier segmentation and its relationship to per-capita assistance
- Identification of outlier counties with highest assistance per capita
- Feature engineering including vulnerability index, assistance per capita, and income brackets

Key finding: 2020 accounts for the majority of records driven by COVID-19 disaster declarations and an active hurricane season. Louisiana counties dominate the top per-capita assistance list due to repeated hurricane impacts.

See the full notebook: [EDAandVisualization.ipynb](./notebooks/EDAandVisualization.ipynb)

---

## Model Training

Three regression models were trained to predict `log(total_assistance_amount)`:

| Model | Cross-Val R2 | Test R2 | Test MAE |
|-------|-------------|---------|----------|
| Ridge Regression | 0.790 | 0.829 | 0.423 |
| Random Forest | **0.853** | **0.895** | **0.329** |
| Gradient Boosting | 0.854 | 0.867 | 0.328 |

**Best model: Random Forest (Test R2 = 0.895)**

- Train/test split: 80/20
- Python packages: scikit-learn, pandas, numpy
- Development environment: Local machine with virtual environment

---

## Streamlit Web Application

An interactive dashboard was built using Streamlit and Plotly with five main sections:

- **Geographic Analysis** — US choropleth map with state-level metrics and top county rankings
- **Trends Over Time** — Year-over-year assistance and applicant volume trends
- **Socioeconomic Relationships** — Interactive scatter plots, poverty tier and income bracket analysis, correlation heatmap
- **Predictive Model** — Model performance metrics and interactive prediction tool
- **Data Explorer** — Sortable, filterable data table with summary statistics

To run the app locally:

```bash
pip install -r requirements.txt
python models/train_models.py
streamlit run app/app.py
```

---

## References

- FEMA OpenFEMA Data Sets: https://www.fema.gov/about/openfema/data-sets
- US Census Bureau American Community Survey: https://data.census.gov/
- Scikit-learn documentation: https://scikit-learn.org/
- Streamlit documentation: https://docs.streamlit.io/
- Plotly Express documentation: https://plotly.com/python/plotly-express/
