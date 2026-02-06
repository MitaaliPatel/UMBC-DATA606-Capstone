# 1. Title and Author

## Project Title

Disaster Resource Allocation Using Data Analytics

Prepared for UMBC Data Science Master Degree Capstone by Dr Chaojie (Jay) Wang

Author Name
Mitaali Patel

Link to the author's GitHub repo of the project
[https://github.com/MitaaliPatel/UMBC-DATA606-Capstone](https://github.com/MitaaliPatel/UMBC-DATA606-Capstone)

Link to the author's LinkedIn profile
[https://www.linkedin.com/in/mitaali-patel/](https://www.linkedin.com/in/mitaali-patel/)

Link to the PowerPoint presentation file
(To be added)

Link to the YouTube video
(To be added)

---

# 2. Background

Natural disasters such as hurricanes, floods, wildfires, and severe storms cause major damage to communities each year. These events disrupt infrastructure, displace residents, and create urgent demand for emergency resources such as housing assistance, food, and medical support. Agencies like the Federal Emergency Management Agency (FEMA) rely on damage reports and assistance applications to estimate disaster impact and distribute limited resources.

Disaster response decisions are often made under time pressure and incomplete information. Historical disaster data combined with demographic and socioeconomic indicators can be used to improve estimates of disaster severity and resource needs. A data-driven approach can support more effective planning and allocation of emergency assistance.

This project develops a Disaster Resource Allocation System using real-world datasets to analyze disaster impact and predict resource needs at the county or state level. Exploratory data analysis and predictive modeling will be used to identify key factors associated with assistance demand and damage severity. The final outcome will be an interactive Streamlit web application for exploring disaster risk and estimated resource requirements.

### Research Questions

1. How do disaster type, location, and severity relate to the amount of assistance requested and approved?
2. Which demographic and socioeconomic factors are linked to higher disaster vulnerability and resource demand?
3. Can historical disaster and population data predict housing assistance needs?
4. What features best explain disaster impact and emergency resource allocation?

---

# 3. Data

This project uses public datasets related to disaster events, disaster assistance, and population vulnerability.

## 3.1 Data Sources

1. FEMA Individual Assistance Housing Registrants Data
   Source: [https://www.fema.gov/about/openfema/data-sets](https://www.fema.gov/about/openfema/data-sets)
   Records of households and individuals who applied for disaster assistance.

2. NOAA Storm Events Database
   Source: [https://www.ncdc.noaa.gov/stormevents/](https://www.ncdc.noaa.gov/stormevents/)
   Information on weather-related disasters such as floods, hurricanes, and tornadoes.

3. U.S. Census American Community Survey (ACS)
   Source: [https://www.census.gov/programs-surveys/acs](https://www.census.gov/programs-surveys/acs)
   County- and state-level demographic and socioeconomic indicators.

All datasets will be downloaded and stored in the `data` subfolder.

---

## 3.2 Data Size and Shape (Estimated)

### FEMA Individual Assistance Dataset

* Size: ~500–800 MB
* Shape: ~3–5 million rows, 20–30 columns
* Time period: 2010–2023
* Each row represents one disaster assistance application

### NOAA Storm Events Dataset

* Size: ~200 MB
* Shape: ~1 million rows, 15–20 columns
* Time period: 2010–2023
* Each row represents one disaster event

### Census ACS Dataset

* Size: ~50 MB
* Shape: ~3,000 rows, 10–20 columns
* Time period: 2019–2023
* Each row represents one geographic unit (county or state)

---

## 3.3 What Each Row Represents

| Dataset           | Row Represents                        |
| ----------------- | ------------------------------------- |
| FEMA IA           | One disaster assistance application   |
| NOAA Storm Events | One disaster event                    |
| Census ACS        | One geographic unit (county or state) |

---

## 3.4 Data Dictionary (Selected Columns)

### FEMA Individual Assistance Dataset

| Column Name             | Data Type   | Definition             | Potential Values       |
| ----------------------- | ----------- | ---------------------- | ---------------------- |
| disasterNumber          | Integer     | FEMA disaster ID       | Numeric                |
| state                   | Categorical | U.S. state code        | TX, FL, CA, etc.       |
| county                  | Categorical | County name            | Text                   |
| incidentType            | Categorical | Disaster type          | Flood, Hurricane, Fire |
| totalDamageAmount       | Float       | Estimated damage       | Continuous             |
| housingAssistanceAmount | Float       | Approved housing aid   | Continuous             |
| personalPropertyLoss    | Float       | Reported property loss | Continuous             |

### NOAA Storm Events Dataset

| Column Name     | Data Type   | Definition       | Potential Values          |
| --------------- | ----------- | ---------------- | ------------------------- |
| EVENT_TYPE      | Categorical | Event type       | Flood, Tornado, Hurricane |
| BEGIN_DATE      | Date        | Event start date | YYYY-MM-DD                |
| DAMAGE_PROPERTY | Float       | Property damage  | Continuous                |
| DAMAGE_CROPS    | Float       | Crop damage      | Continuous                |
| STATE           | Categorical | State            | TX, FL, CA                |

### Census ACS Dataset

| Column Name            | Data Type | Definition                        | Potential Values |
| ---------------------- | --------- | --------------------------------- | ---------------- |
| median_income          | Float     | Median household income           | Continuous       |
| poverty_rate           | Float     | Population below poverty line (%) | Continuous       |
| population             | Integer   | Total population                  | Continuous       |
| elderly_population_pct | Float     | Population aged 65+ (%)           | Continuous       |

---

## 3.5 Target Variable

The primary target variable is:

**housingAssistanceAmount** (FEMA dataset)

This represents approved housing assistance and serves as a proxy for disaster resource needs.

Alternative targets for secondary analysis:

* totalDamageAmount
* number of applications per county
* high-impact disaster indicator (binary)

---

## 3.6 Feature Variables (Predictors)

Potential predictors include:

From FEMA:

* incidentType
* state
* county
* personalPropertyLoss
* number of applicants per disaster

From NOAA:

* EVENT_TYPE
* DAMAGE_PROPERTY
* DAMAGE_CROPS
* event frequency

From Census:

* population
* median_income
* poverty_rate
* elderly_population_pct

Datasets will be merged using geographic identifiers (state and county) and time period.

---

## 3.7 Suitability of the Dataset

These datasets are appropriate because they:

* Are real-world, public, and authoritative.
* Support feature engineering and predictive modeling.
* Include both disaster impact measures and vulnerability indicators.
* Enable geographic and temporal analysis.
* Can be integrated into an interactive Streamlit application.

The final dataset will be transformed into a tidy format where each row represents disaster impact at a geographic level with associated vulnerability and assistance outcomes. Exploratory analysis will be conducted in a Jupyter Notebook stored in the `notebooks` folder.
