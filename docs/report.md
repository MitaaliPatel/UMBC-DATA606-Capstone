# Disaster Resource Allocation Using Data Analytics

**Author:** Mitaali Patel  
**Program:** UMBC Data Science Master Degree Capstone — DATA606  
**Advisor:** Dr. Chaojie (Jay) Wang  
**Semester:** Spring 2026  

**Links:**
- [GitHub Repository](https://github.com/MitaaliPatel/UMBC-DATA606-Capstone)
- [LinkedIn Profile](https://www.linkedin.com/in/mitaali-patel)
- [PowerPoint Presentation](https://canva.link/qqr4e475ihcar61)
- YouTube Video: *Coming soon*

---

## 1. Background

### What is this project about?

Every year, the Federal Emergency Management Agency (FEMA) distributes billions of dollars in disaster assistance to counties across the United States following declared disasters. This assistance covers housing damage, personal property loss, and other immediate recovery needs for affected residents. Despite the scale of this program, resource allocation decisions are often reactive — funds are disbursed in response to documented demand rather than proactive estimates of community need.

This project analyzes FEMA Individual Assistance data from 2019 to 2022 alongside US Census socioeconomic indicators to understand what drives disaster assistance distribution at the county level. By merging these two data sources, the project examines how population characteristics, poverty levels, income, and geographic factors relate to the amount and type of assistance counties receive.

### Why does it matter?

Understanding disaster assistance patterns has meaningful implications for emergency management policy. If socioeconomic vulnerability does not reliably predict assistance levels, it raises questions about whether the most at-risk communities are being adequately served. Conversely, if demand volume — measured by applicant count — is the primary driver of disbursements, then communities with lower disaster awareness or fewer resources to navigate the application process may be systematically underserved even when their need is high.

Beyond the policy implications, predictive models that estimate likely assistance needs can help emergency managers pre-position resources, set budgetary expectations, and prioritize outreach in vulnerable communities before or immediately after a disaster event.

### Research Questions

1. How do disaster location and severity relate to the amount of assistance requested and approved?
2. Which demographic and socioeconomic factors are linked to higher disaster vulnerability and resource demand?
3. Can historical disaster and population data predict housing assistance needs?
4. What features best explain disaster impact and emergency resource allocation?

---

## 2. Data

### Data Sources

This project uses two publicly available datasets:

**FEMA OpenFEMA Individual Assistance Data**
Available at: https://www.fema.gov/about/openfema/data-sets

This dataset contains yearly summaries of FEMA Individual Assistance by county, including the number of applicants, total assistance disbursed, and housing-specific assistance amounts. It covers disaster declarations from 2019 through 2022.

**US Census Bureau American Community Survey (ACS)**
Available at: https://data.census.gov

The ACS provides county-level socioeconomic indicators including total population, median household income, poverty count, and poverty rate. Five-year ACS estimates were used for years 2019 through 2022.

### Dataset Overview

| Dataset | Rows | Columns | Time Period |
|---------|------|---------|-------------|
| FEMA Assistance | 3,794 | 6 | 2019-2022 |
| ACS Socioeconomic | 16,106 | 8 | 2019-2022 |
| Master (Merged) | 3,611 | 12 | 2019-2022 |

The two datasets were merged at the county-year level using standardized state abbreviations and cleaned county names. Each row in the master dataset represents one county in one calendar year.

### Data Dictionary

| Column | Type | Description |
|--------|------|-------------|
| `state_clean` | Categorical | US state abbreviation (e.g. LA, TX) |
| `county_clean` | Categorical | Standardized county name |
| `Year` | Integer | Year of record (2019-2022) |
| `total_population` | Integer | Total county population from ACS |
| `median_income` | Integer | Median household income in dollars |
| `poverty_rate` | Float | Percentage of population below the federal poverty line |
| `poverty_count` | Integer | Number of residents below the federal poverty line |
| `total_applicants` | Integer | Number of FEMA Individual Assistance applicants |
| `housing_assistance_amount` | Float | Total housing assistance disbursed in dollars |
| `total_assistance_amount` | Float | Total FEMA assistance disbursed in dollars |

**Target Variable:** `total_assistance_amount`

**Predictor Features:** `total_population`, `median_income`, `poverty_rate`, `poverty_count`, `total_applicants`, `housing_assistance_amount`, and engineered features described in Section 4.

### Data Quality Notes

One row in the master dataset contained a corrupted `median_income` value of -666,666,666. This row was removed from all analyses using the filter `df[df['median_income'] > 0]`. No other missing values were found in the merged dataset.

The dataset is not evenly distributed across years. The year 2020 accounts for 3,068 of the 3,611 records (approximately 85%) due to the combination of nationwide COVID-19 disaster declarations and one of the most active Atlantic hurricane seasons on record. This concentration is an important caveat when interpreting temporal patterns.

---

## 3. Exploratory Data Analysis

All EDA was performed in Jupyter Notebook. The full notebook is available at: [EDAandVisualization.ipynb](./notebooks/EDAandVisualization.ipynb) 

### Summary Statistics

The target variable `total_assistance_amount` has a heavily right-skewed distribution, with a mean of approximately $1.85 million per county-year but a median of only $204,000. The maximum value is $244 million, corresponding to Louisiana counties affected by Hurricane Ida in 2021. This extreme skew motivated the use of log transformation in the modeling phase.

The average poverty rate across all county-year records is 14.3%, ranging from 1.7% to 57.9%. The average median household income is approximately $52,000, though one corrupted value was removed as noted above.

### Correlation Analysis

A Pearson correlation analysis between all numeric variables and `total_assistance_amount` revealed the following key findings:

- `total_applicants` has the strongest correlation with total assistance at **r = 0.87**
- `housing_assistance_amount` correlates at **r = 0.93** (expected given it is a component of total assistance)
- `poverty_count` correlates at **r = 0.42**
- `poverty_rate` correlates at only **r = 0.02** — surprisingly weak
- `median_income` correlates at essentially zero (**r = 0.00**)

The near-zero correlation of poverty rate with total assistance is the most notable finding from this analysis. It suggests that FEMA disbursements respond primarily to documented demand volume rather than to pre-existing socioeconomic vulnerability. Counties with higher poverty rates do not automatically receive more assistance.

### Geographic Analysis

Louisiana received more total FEMA assistance than any other state over the 2019-2022 period, with a total exceeding $1.6 billion. This reflects the impact of Hurricanes Laura (2020), Delta (2020), and Ida (2021). Texas and New York rank second and third respectively.

When assistance is normalized by county population, Louisiana counties dominate the top 10 list. Cameron Parish, Louisiana received approximately $2,151 per resident in 2020 alone. Kentucky counties (Knott, Breathitt, Letcher) also appear prominently due to severe flooding events in 2022.

New Jersey and Michigan rank higher than expected in total assistance figures, which reflects COVID-19 disaster declarations rather than natural disasters. These declarations were nationwide in scope and contributed significantly to 2020 totals across many non-hurricane states.

### Temporal Analysis

Applicant volume grew steadily from approximately 160,000 in 2019 to a peak of over 1.6 million in 2021, before dropping sharply to approximately 160,000 in 2022. The 2020-2021 spike is driven by the overlap of COVID-19 declarations and major hurricane activity. The sharp decline in 2022 reflects the end of COVID-related disaster declarations.

This temporal imbalance means that any model trained on this dataset will have learned heavily from 2020 and 2021 patterns. Results should be interpreted with this in mind.

### Poverty Tier Analysis

Counties were grouped into four poverty tiers: Low (less than 10%), Moderate (10-20%), High (20-30%), and Extreme (30%+). Boxplots of assistance per applicant across these tiers on a log scale show nearly identical median values across all four groups. This reinforces the correlation finding — poverty level does not predict how much assistance each applicant receives. Geography and disaster exposure are more important determinants.

### Key EDA Findings Summary

- Demand volume (applicant count) is the dominant driver of FEMA assistance amounts
- Poverty rate has essentially no predictive relationship with total assistance
- Louisiana and Kentucky counties receive the highest per-capita assistance due to repeated disaster impacts
- 2020 and 2021 account for the vast majority of assistance and applicant volume
- The distribution of assistance amounts is extremely right-skewed, requiring log transformation for modeling

---

## 4. Feature Engineering

Prior to modeling, the following features were engineered from the raw dataset:

| Feature | Formula | Purpose |
|---------|---------|---------|
| `assistance_per_capita` | total_assistance / total_population | Normalizes assistance by county size |
| `assistance_per_applicant` | total_assistance / total_applicants | Average aid received per applicant |
| `applicants_per_capita` | total_applicants / total_population | Normalized disaster demand signal |
| `housing_share` | housing_assistance / total_assistance | Proportion of aid allocated to housing |
| `vulnerability_index` | (z_poverty + z_income) / 2 | Composite socioeconomic risk score |
| `log_total_assistance` | log1p(total_assistance_amount) | Log-transformed target variable |
| `log_applicants` | log1p(total_applicants) | Log-transformed applicant count |
| `log_population` | log1p(total_population) | Log-transformed population |
| `log_income` | log1p(median_income) | Log-transformed income |
| `state_mean_log_assistance` | Mean of log_total_assistance by state | State-level historical encoding |

The vulnerability index combines z-scores of poverty rate (positive direction) and median income (negative direction) into a single composite risk measure, where higher values indicate greater socioeconomic vulnerability.

Log transformations were applied to all right-skewed variables to stabilize variance and improve model performance. The target variable `log(total_assistance_amount)` was predicted by the model and back-transformed using `expm1()` to obtain dollar estimates.

Income brackets and poverty tiers were created as categorical variables for segmented analysis in the EDA phase but were not used as model features.

---

## 5. Model Training

### Model Selection

Three regression models were evaluated to predict `log(total_assistance_amount)`:

- **Ridge Regression** — linear model with L2 regularization, serves as a baseline
- **Random Forest** — ensemble of decision trees, captures non-linear relationships
- **Gradient Boosting** — sequential ensemble method, strong on structured tabular data

### Feature Set

Ten features were used as model inputs:

`log_population`, `log_income`, `poverty_rate`, `poverty_count`, `vulnerability_index`, `applicants_per_capita`, `log_applicants`, `housing_share`, `Year`, `state_mean_log_assistance`

### Training Configuration

- Train/test split: 80/20, random_state=42
- Cross-validation: 5-fold on training set
- Evaluation metrics: R2 score, Mean Absolute Error (MAE), Root Mean Squared Error (RMSE)
- Development environment: Local machine with Python 3.14 virtual environment
- Python packages: scikit-learn, pandas, numpy

### Model Hyperparameters

**Random Forest:** n_estimators=200, max_depth=8, min_samples_leaf=5, random_state=42

**Gradient Boosting:** n_estimators=200, max_depth=4, learning_rate=0.05, random_state=42

**Ridge Regression:** alpha=1.0

### Results

| Model | CV R2 | Test R2 | Test MAE | Test RMSE |
|-------|-------|---------|----------|-----------|
| Ridge Regression | 0.790 | 0.829 | 0.423 | 0.870 |
| Random Forest | **0.853** | **0.895** | **0.329** | **0.680** |
| Gradient Boosting | 0.854 | 0.867 | 0.328 | 0.768 |

**Best model: Random Forest with Test R2 = 0.895**

The Random Forest model explains approximately 89.5% of the variance in log-transformed assistance amounts on held-out test data. All metrics are reported in log-scale units. When back-transformed to dollar amounts, the model produces predictions with a typical error range of roughly one order of magnitude, which is reasonable given the extreme variance in the target variable.

### Feature Importance

The Random Forest feature importance analysis reveals a striking result: `log_applicants` has an importance score above 0.85, far exceeding all other features combined. The remaining features — Year, housing_share, applicants_per_capita, poverty_count — each contribute less than 0.05. Poverty rate and vulnerability index have importance scores near zero.

This confirms the EDA finding that applicant volume is the primary determinant of FEMA assistance amounts. The implication is that FEMA's disbursement system is fundamentally demand-responsive: assistance flows to where people apply, not necessarily where socioeconomic need is highest.

### Model Performance on Actual vs Predicted

Scatter plots of actual versus predicted values show that all three models cluster tightly around the perfect fit line for the bulk of the dataset. All models show some underprediction for extreme outlier counties — those affected by catastrophic single events like major hurricanes. These outliers are difficult to model from socioeconomic features alone since their assistance levels reflect the scale of physical destruction rather than demographic characteristics.

---

## 6. Streamlit Web Application

An interactive dashboard was built using Streamlit and Plotly to make the project findings accessible and explorable. The application loads the trained Random Forest model and the engineered dataset to power all visualizations and predictions.

### Running the Application

```bash
pip install -r requirements.txt
python models/train_models.py
streamlit run app.py
```

### Dashboard Features

**Geographic Analysis Tab**

An interactive US choropleth map displays state-level metrics with radio button controls to toggle between total assistance, assistance per capita, total applicants, poverty rate, and vulnerability index. State abbreviations are overlaid directly on the map. Supporting charts show the top 15 states by total assistance and the top 10 counties by assistance per capita. The state-year heatmap output image is also displayed with an explanatory caption.

**Trends Over Time Tab**

Line charts show total and housing assistance trends from 2019 to 2022 with data labels. Bar charts show applicant volume by year. An area chart shows the number of counties receiving aid each year. Distribution boxplots on a log scale show the spread of county-level assistance amounts across years.

**Socioeconomic Relationships Tab**

An interactive scatter plot allows users to select any combination of X and Y variables from the dataset, with an OLS trendline and color coding by state. A correlation bar chart shows Pearson correlations of all key variables with total assistance. Poverty tier and income bracket analyses are shown as grouped bar charts. The vulnerability vs assistance and income bracket output images are displayed with captions.

**Model Results Tab**

Model performance metrics for all three models are displayed as styled cards with the best model highlighted. The model comparison, actual vs predicted, and feature importance output images are shown with plain-language explanations of what each chart means.

**Predict Assistance Tab**

Users can enter county characteristics including population, median income, poverty rate, estimated applicant count, year, state, and expected housing assistance share. The Random Forest model generates a prediction displayed as a gauge chart with color zones (green for low, orange for moderate, red for high) alongside styled cards showing the predicted amount and a confidence range based on test set RMSE.

---

## 7. Conclusion

### Summary

This project demonstrates that FEMA disaster assistance distribution is primarily driven by applicant demand volume rather than pre-existing socioeconomic vulnerability. A Random Forest model trained on county-level FEMA and Census data achieves a Test R2 of 0.895, explaining approximately 90% of the variance in assistance amounts using only 10 features. The dominant feature — log-transformed applicant count — accounts for over 85% of the model's predictive power.

The geographic analysis reveals significant concentration of assistance in disaster-prone states, particularly Louisiana, while the temporal analysis shows the outsized influence of 2020 and 2021 driven by COVID-19 declarations and an active hurricane season. The finding that poverty rate has near-zero predictive power for total assistance raises important questions about equity in disaster relief allocation.

### Limitations

**Temporal range:** The dataset covers only four years (2019-2022). This limits the ability to capture long-term disaster patterns and makes the model heavily influenced by the 2020-2021 anomaly.

**No disaster type variable:** The FEMA dataset does not include disaster type (hurricane, flood, tornado, wildfire, etc.). Incorporating disaster type would likely improve both the analysis and model performance significantly.

**Demand-driven bias:** Because applicant count is the strongest predictor, the model essentially predicts assistance based on how many people applied. This means it may not accurately estimate needs for communities with low application rates due to barriers such as language, awareness, or internet access.

**Static predictions:** The model predicts assistance given that a disaster has already occurred and applicants have registered. It does not predict whether or when a disaster will occur.

**Geographic scope:** The model was trained on US county-level data and is not generalizable to other countries or sub-county geographies.

### Future Research Directions

- Incorporate FEMA disaster declaration records to add disaster type and severity classifications as features
- Build a separate predictive model specifically targeting `housing_assistance_amount` to address Research Question 3 more directly
- Extend the dataset to include 2023 and beyond as FEMA releases updated OpenFEMA data
- Develop a county-level pre-disaster risk scoring system that estimates likely assistance needs without requiring applicant count as an input
- Investigate the relationship between application barriers (internet access, language, immigration status) and assistance gaps in high-vulnerability counties
- Explore time-series modeling approaches that can capture disaster recurrence patterns at the state and regional level

---

## 8. References

- FEMA OpenFEMA Data Sets: https://www.fema.gov/about/openfema/data-sets
- US Census Bureau American Community Survey: https://data.census.gov/
- Pedregosa et al. (2011). Scikit-learn: Machine Learning in Python. Journal of Machine Learning Research, 12, 2825-2830. https://scikit-learn.org/
- Streamlit Documentation: https://docs.streamlit.io/
- Plotly Express Documentation: https://plotly.com/python/plotly-express/
- pandas Documentation: https://pandas.pydata.org/docs/
- NumPy Documentation: https://numpy.org/doc/
- Breiman, L. (2001). Random Forests. Machine Learning, 45(1), 5-32.
- Friedman, J.H. (2001). Greedy Function Approximation: A Gradient Boosting Machine. Annals of Statistics, 29(5), 1189-1232.
- Hoerl, A.E. and Kennard, R.W. (1970). Ridge Regression: Biased Estimation for Nonorthogonal Problems. Technometrics, 12(1), 55-67.
