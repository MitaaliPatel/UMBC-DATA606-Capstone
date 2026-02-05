# 1. Title and Author

## Project Title  
Disaster Resource Allocation System: Data-Driven Estimation of Disaster Impact and Emergency Resource Needs

Prepared for UMBC Data Science Master Degree Capstone by Dr Chaojie (Jay) Wang

Author Name  
Mitaali Patel  

Link to the author's GitHub repo of the project  
https://github.com/MitaaliPatel/UMBC-DATA606-Capstone 

Link to the author's LinkedIn profile  
https://www.linkedin.com/in/mitaali-patel/  

Link to the PowerPoint presentation file  
(To be added)

Link to the YouTube video  
(To be added)

---

# 2. Background

Natural disasters such as hurricanes, floods, wildfires, and severe storms cause significant damage to communities each year. These events disrupt infrastructure, displace populations, and require rapid and effective deployment of emergency resources such as shelter assistance, housing aid, food, and medical services. Government agencies such as the Federal Emergency Management Agency (FEMA) rely on damage assessments and applications for assistance to estimate the severity of disaster impact and allocate limited resources accordingly.

However, disaster response decisions are often made under conditions of uncertainty, incomplete data, and time pressure. Historical disaster data, when combined with demographic and socioeconomic indicators, can be used to build predictive models that estimate disaster severity and resource needs more accurately. A data-driven system that integrates disaster event information, population vulnerability, and past assistance outcomes has the potential to improve disaster preparedness and optimize emergency resource allocation.

This project aims to develop a Disaster Resource Allocation System using real-world datasets to analyze disaster impact and predict emergency resource needs at a geographic level (e.g., county or state). The project will use exploratory data analysis and predictive modeling techniques to identify key factors influencing assistance demand and damage severity. The final outcome will be an interactive Streamlit web application that allows users to explore disaster risk and estimated resource needs based on historical data.

### Research Questions

1. How do disaster characteristics (type, severity, and location) relate to the amount of assistance requested and approved?
2. What demographic and socioeconomic factors are associated with higher disaster vulnerability and resource demand?
3. Can historical disaster and population data be used to predict disaster-related housing or assistance needs?
4. Which features are the strongest predictors of disaster impact and emergency resource allocation?

---

# 3. Data

This project will use a combination of public datasets related to disaster events, disaster assistance, and population vulnerability.

## 3.1 Data Sources

1. FEMA Individual Assistance Housing Registrants Data  
   Source: https://www.fema.gov/openfema-data-page/individual-assistance-housing-registrants-large-disasters  
   This dataset contains records of individuals and households who applied for disaster assistance following major disasters.

2. NOAA Storm Events Database  
   Source: https://www.ncdc.noaa.gov/stormevents/  
   This dataset provides detailed information about weather-related disaster events including hurricanes, floods, tornadoes, and storms.

3. U.S. Census American Community Survey (ACS)  
   Source: https://www.census.gov/programs-surveys/acs  
   This dataset provides demographic and socioeconomic indicators at the county and state level.

The datasets will be downloaded and stored in the `data` subfolder.

---

## 3.2 Data Size and Shape (Estimated)

### FEMA Individual Assistance Dataset
- Size: Approximately 500–800 MB (depending on years selected)
- Shape: Approximately 3–5 million rows and 20–30 columns
- Time period: 2010–2023
- Each row represents one household or individual assistance application

### NOAA Storm Events Dataset
- Size: Approximately 200 MB
- Shape: Approximately 1 million rows and 15–20 columns
- Time period: 2010–2023
- Each row represents one recorded disaster event

### Census ACS Dataset
- Size: Approximately 50 MB
- Shape: Approximately 3,000 rows (county-level) and 10–20 columns
- Time period: 2019–2023
- Each row represents one geographic unit (county or state)

---

## 3.3 What Each Row Represents

| Dataset | Row Represents |
|---------|----------------|
| FEMA IA | One disaster assistance application |
| NOAA Storm Events | One disaster event (e.g., flood, hurricane) |
| Census ACS | One geographic unit (county or state) |

---

## 3.4 Data Dictionary (Selected Columns)

### FEMA Individual Assistance Dataset

| Column Name | Data Type | Definition | Potential Values |
|-------------|----------|------------|------------------|
| disasterNumber | Integer | FEMA disaster identifier | Numeric |
| state | Categorical | U.S. state code | TX, FL, CA, etc. |
| county | Categorical | County name | Text |
| incidentType | Categorical | Type of disaster | Flood, Hurricane, Fire, Storm |
| totalDamageAmount | Float | Estimated total damage | Continuous |
| housingAssistanceAmount | Float | Approved housing assistance | Continuous |
| personalPropertyLoss | Float | Reported property loss | Continuous |

### NOAA Storm Events Dataset

| Column Name | Data Type | Definition | Potential Values |
|-------------|----------|------------|------------------|
| EVENT_TYPE | Categorical | Type of weather event | Flood, Tornado, Hurricane |
| BEGIN_DATE | Date | Event start date | YYYY-MM-DD |
| DAMAGE_PROPERTY | Float | Estimated property damage | Continuous |
| DAMAGE_CROPS | Float | Estimated crop damage | Continuous |
| STATE | Categorical | State where event occurred | TX, FL, CA |

### Census ACS Dataset

| Column Name | Data Type | Definition | Potential Values |
|-------------|----------|------------|------------------|
| median_income | Float | Median household income | Continuous |
| poverty_rate | Float | Percent of population below poverty line | Continuous |
| population | Integer | Total population | Continuous |
| elderly_population_pct | Float | Percent aged 65+ | Continuous |

---

## 3.5 Target Variable

The target (label) variable for the machine learning model will be:

**housingAssistanceAmount** (from FEMA dataset)

This represents the amount of approved housing assistance and serves as a proxy for disaster resource needs.

Alternative target variables (for secondary analysis):
- totalDamageAmount  
- number of applications per county  
- binary indicator of high-impact disaster (above threshold damage)

---

## 3.6 Feature Variables (Predictors)

Potential feature variables include:

From FEMA:
- incidentType  
- state  
- county  
- personalPropertyLoss  
- number of applicants per disaster  

From NOAA:
- EVENT_TYPE  
- DAMAGE_PROPERTY  
- DAMAGE_CROPS  
- event frequency  

From Census:
- population  
- median_income  
- poverty_rate  
- elderly_population_pct  

These variables will be merged using geographic identifiers (state and county) and time period.

---

## 3.7 Suitability of the Dataset

These datasets are suitable for this project because:
- They are real-world, authoritative, and publicly available.
- They support feature engineering and predictive modeling.
- They contain both impact variables (damage, assistance) and vulnerability indicators (income, population).
- They enable geographic and temporal analysis.
- They can be visualized and deployed in an interactive Streamlit application.

The resulting dataset will be transformed into a tidy format where each row represents a disaster impact observation at a geographic level with corresponding vulnerability indicators and resource outcomes.

The exploratory data analysis will be conducted using a Jupyter Notebook saved in the `notebooks` folder.
