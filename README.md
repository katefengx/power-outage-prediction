# Power Outage Prediction
by Kate Feng and Tracy Vu

# Introduction

This project examines the regional impact on the severity of power outages in the United States. In particular, we are investigating: **Do power outages last longer in the Northeast Midwest region compared to other regions of the U.S.?** The dataset we are using is Purdue University’s Major Power Outage Risks in the U.S. dataset, which consists of major power outages, defined by the Department of Energy as those affecting over 50,000 customers or resulting in a loss of over 300 megawatts of energy. The data spans from January 2000 to July 2016 across the United States. Each of the 1,534 rows represents a single power outage occurrence, with 57 total columns containing information such as the duration of the outage, the state and region in which it occurred, and the category of the outage’s cause.

The columns we included in our analysis are:
- `'YEAR'` - This column labels the year that the power outage occurred.
- `'MONTH'` - This column labels the month that the power outage occurred.  
- `'U.S._STATE'` - This column labels the state that the power outage was located. Column renamed to `'STATE'` for readability.
- `'POSTAL.CODE'` - This column labels the abbreviation for the state the power outage was located. Column renamed to `'STATE.ABBV'` for readability.
- `'CLIMATE.REGION'` - This column categorizes the region where the outage occurred. The possible values include: 
    - East North Central (Northeast Midwest)
    - Central
    - Northeast
    - Northwest
    - South
    - Southeast
    - Southwest
    - West
    - West North Central
- `'CLIMATE.CATEGORY'` - This column categorizes the climate of the region where the outage occurred. The possible values include:
    - Normal
    - Cold
    - Warm
- `'CAUSE.CATEGORY'` - This column categorizes the overall cause of the outage. The possible values include:
    - Equipment failure
    - Severe weather
    - Public appeal
    - Intentional attack
    - Fuel supply emergency
    - Islanding
    - System operability
- `'OUTAGE.DURATION'` - This column represents the length of time (in minutes) that an outage persisted, providing a quantitative measure of outage severity.
- `'CUSTOMERS.AFFECTED'` - This column represents the number of customers affected by the outage.
- `'DEMAND.LOSS.MW'` - This column represents the amount of energy (in megawatts) that the customers affected by the outage demanded during the duration of the outage. This column, as well as the `'CUSTOMERS.AFFECTED'` column, provide an examination into whether public pressure affects the speed of outage repair.

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning
In order to effectively analyze our dataset, we must first clean it. To do this, we first dropped columns that we knew we wouldn’t be using.
The dropped columns include: `"OBS"`, `"variables"`, `"RES.PRICE`", `"COM.PRICE"`, `"IND.PRICE"`, `"TOTAL.PRICE"`, `"RES.SALES"`, `"COM.SALES"`, `"IND.SALES"`, `"TOTAL.SALES"`, `"RES.PERCEN"`, `"COM.PERCEN"`, `"IND.PERCEN"`, `"RES.CUSTOMERS"`, `"COM.CUSTOMERS"`, `"IND.CUSTOMERS"`, `"TOTAL.CUSTOMERS"`, `"RES.CUST.PCT"`, `"COM.CUST.PCT"`, `"IND.CUST.PCT"`, `"PC.REALGSP.STATE"`, `"PC.REALGSP.USA"`, `"PC.REALGSP.REL"`, `"PC.REALGSP.CHANGE"`, `"UTIL.REALGSP"`, `"TOTAL.REALGSP"`, `"UTIL.CONTRI"`, `"PI.UTIL.OFUSA"`, `'POPPCT_URBAN'`, `'POPPCT_UC'`, `'POPDEN_URBAN'`, `'POPDEN_UC'`, `'POPDEN_RURAL'`, `'AREAPCT_URBAN'`, `'AREAPCT_UC'`, `'PCT_LAND'`, `'PCT_WATER_TOT'`, `'PCT_WATER_INLAND'`, `'ANOMALY.LEVEL'`, `'OUTAGE.START.DATE'`, `'OUTAGE.START.TIME'`, `'OUTAGE.RESTORATION.DATE'`, `'OUTAGE.RESTORATION.TIME'`, `'CAUSE.CATEGORY.DETAIL'`, `'HURRICANE.NAMES'`, `'POPULATION'`.

We converted the `'MONTH'` column from numerical values to text values and renamed columns (`'U.S._STATE'` to `'STATE'`, `'POSTAL.CODE'` to `'STATE.ABBV'`) for readability.

## Exploratory Data Analysis

### Univariate Analysis: Outage Duration Histogram and Chloropleth Map
For our first univariate plot, we created a histogram to show the distribution of power outage durations. To further clean our `'OUTAGE.DURATION'` data, we removed rows containing `'np.nan'` values, excluded the max value (extreme outlier), then calculated our standard deviation. We used the SD to further exclude outliers—values beyond three standard deviations from the mean—for a clearer visual representation. We found that outages between 0 and 499 minutes dominate the distribution, with a 0.47 probability. 
<iframe
  src="assets/duration_histogram.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

We also examined how the number of power outages varied by state. To visualize this, we created a choropleth map showing the distribution of power outages across different states. We grouped the data by `'STATE.ABBV'`, counted the number of rows per group, and added labels for the number of outages in states with over 50 outages. The major states here included California with 210 outages, Texas with 127 outages, and Washington with 97 outages over the 15.5 years of data.
<iframe
  src="assets/outages_map.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Bivariate Analysis: Outage Duration Against Region and Year
We wanted to know if there was a relationship between the region where the outage occurred, `'CLIMATE.REGION'`, and its duration, `'OUTAGE.DURATION'`. To find out, we calculated the average duration per region and plotted it on a bar chart against the region name. Outage durations peaked in the East North Central region, with an average duration of 5,352 minutes, which was well above the average outage durations of the other regions, which peaked at around 3,000 minutes. 
<iframe
  src="assets/bar_region.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>


We also wanted to find out if there was a relationship between the `'YEAR'` and `'OUTAGE.DURATION'` columns, to see if there was a correlation between the year and the length of power outages. We found that there is a negative relationship between year and outage duration, with the number of major outages peaking in 2005 before slowly decreasing.
<iframe
  src="assets/line_year.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>


# Assessment of Missingness

## NMAR Analysis
## Missingness Dependency


# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis

