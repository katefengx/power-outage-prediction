# Power Outage Prediction
by Kate Feng and Tracy Vu

# Introduction

This project explores how power outage data can be leveraged to predict the number of affected customers during an outage across different regions in the United States. In particular, we are investigating: **How can we predict the number of affected customers based on factors like outage duration, region, and cause?** 

The dataset we are using is Purdue University’s Major Power Outage Risks in the U.S. dataset, which consists of major power outages, defined by the Department of Energy as those affecting over 50,000 customers or resulting in a loss of over 300 megawatts of energy. The data spans from January 2000 to July 2016 across the United States. Each of the 1,534 rows represents a single power outage occurrence, with 57 total columns containing information such as the duration of the outage, the state and region in which it occurred, and the category of the outage’s cause.

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
- `'CAUSE.CATEGORY.DETAIL'` - This column gives some additional details to the categories causing the major power outage. 
- `'OUTAGE.DURATION'` - This column represents the length of time (in minutes) that an outage persisted, providing a quantitative measure of outage severity.
- `'CUSTOMERS.AFFECTED'` - This column represents the number of customers affected by the outage.
- `'DEMAND.LOSS.MW'` - This column represents the amount of energy (in megawatts) that the customers affected by the outage demanded during the duration of the outage. This column, as well as the `'CUSTOMERS.AFFECTED'` column, provide an examination into whether public pressure affects the speed of outage repair.
- `'POPDEN_URBAN'` - This column represents the persons per square mile (population density) in the urban areas.
- `'TOTAL.SALES'`- This column represents the total megawatts of electricity consumed per hour in the respective state.
- `'TOTAL.CUSTOMERS'` - This column represents the annual total number of customers served in the state the outage occurred in.
- `'POPULATION'` - This column represents the yearly population of the state the outage occurred in.


# Data Cleaning and Exploratory Data Analysis

## Data Cleaning
In order to effectively analyze our dataset, we must first clean it. To do this, we first dropped columns that we knew we wouldn’t be using.
The dropped columns include: `"OBS"`, `"variables"`, `"RES.PRICE"`, `"COM.PRICE"`, `"IND.PRICE"`, `"TOTAL.PRICE"`, `"RES.SALES"`, `"COM.SALES"`, `"IND.SALES"`, `"RES.PERCEN"`, `"COM.PERCEN"`, `"IND.PERCEN"`, `"RES.CUSTOMERS"`, `"COM.CUSTOMERS"`, `"IND.CUSTOMERS"`, `"RES.CUST.PCT"`, `"COM.CUST.PCT"`, `"IND.CUST.PCT"`, `"PC.REALGSP.STATE"`, `"PC.REALGSP.USA"`, `"PC.REALGSP.REL"`, `"PC.REALGSP.CHANGE"`, `"UTIL.REALGSP"`, `"TOTAL.REALGSP"`, `"UTIL.CONTRI"`, `"PI.UTIL.OFUSA"`, `'POPPCT_URBAN'`, `'POPPCT_UC'`, `'POPDEN_UC'`, `'POPDEN_RURAL'`, `'AREAPCT_UC'`, `'PCT_LAND'`, `'PCT_WATER_TOT'`, `'PCT_WATER_INLAND'`, `'ANOMALY.LEVEL'`, `'OUTAGE.START.DATE'`, `'OUTAGE.START.TIME'`, `'OUTAGE.RESTORATION.DATE'`, `'OUTAGE.RESTORATION.TIME'`, `'HURRICANE.NAMES'`, `'NERC.REGION'`, `'AREAPCT_URBAN'`.

We converted the `'MONTH'` column from numerical values to text values and renamed columns (`'U.S._STATE'` to `'STATE'`, `'POSTAL.CODE'` to `'STATE.ABBV'`) for readability. Furthermore, in order to predict the size of the customer base affected, we created another column called `'AFFECTED_BUCKET'` with lables based off of `'CUSTOMERS.AFFECTED'` with these bins:

| **Bin Range (Customers Affected)** | **Label**                 |
|------------------------------------|---------------------------|
| 0 - 10,000                         | Small Customer Base       |
| 10,000 - 100,000                   | Medium Customer Base      |
| 100,000 - 500,000                  | Large Customer Base       |
| 500,000+                           | Very Large Customer Base  |

Below are a sample of rows from our cleaned dataset.

| YEAR | MONTH   | STATE         | STATE.ABBV | CLIMATE.REGION | CLIMATE.CATEGORY | CAUSE.CATEGORY      | CAUSE.CATEGORY.DETAIL | OUTAGE.DURATION | DEMAND.LOSS.MW | CUSTOMERS.AFFECTED | TOTAL.SALES | TOTAL.CUSTOMERS | POPULATION | POPDEN_URBAN | AFFECTED_BUCKET     |
|:-----|:--------|:--------------|:-----------|:---------------|:-----------------|:--------------------|:----------------------|-----------------|----------------|--------------------|-------------|-----------------|------------|--------------|---------------------|
| 766  | 2000.0  | NaN           | North Carolina | NC             | Southeast        | NaN                 | severe weather       | thunderstorm     | NaN             | 5.00e+04           | NaN         | 4.11e+06        | 8.08e+06  | 1367.2       | Medium Customer Base |
| 1049 | 2008.0  | February      | Florida    | FL             | Southeast        | cold                | system operability disruption | NaN       | 91             | 318                | 5.40e+04   | 16515248        | 9.63e+06  | 1.85e+07     | Medium Customer Base |
| 575  | 2005.0  | July          | Maryland   | MD             | Northeast        | normal              | severe weather       | thunderstorm     | 4517            | NaN                | 6.49e+04   | 6782930         | 2.36e+06  | 5.59e+06     | Medium Customer Base |
| 331  | 2010.0  | June          | Indiana    | IN             | Central          | normal              | severe weather       | thunderstorm     | 513             | NaN                | 5.30e+04   | 9121925         | 3.10e+06  | 6.49e+06     | Medium Customer Base |
| 705  | 2003.0  | August        | Ohio       | OH             | Central          | normal              | system operability disruption | NaN       | 3137            | 7000               | 1.20e+06   | 14199635        | 5.40e+06  | 1.14e+07     | Very Large Customer Base |


## Exploratory Data Analysis

### Univariate Analysis: **Customer Base Size Histogram and Chloropleth Map**
For our first univariate plot, we created a histogram to show the distribution of the customer base sizes. We found that most outages affected a medium to large customer base, with 371 outages affecting 10,000 to 100,000 customers and 385 outages affecting 100,000 to 500,000 customers. 
<iframe
  src="assets/affected_hist.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

We also examined how the number of power outages varied by state. To visualize this, we created a choropleth map showing the distribution of power outages across different states. We grouped the data by `'STATE.ABBV'`, counted the number of rows per group, and added labels for the number of outages in states with over 50 outages. The major states here included California with 210 outages, Texas with 127 outages, and Washington with 97 outages over the 15.5 years of data.
<iframe
  src="assets/outages_map.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

### Bivariate Analysis: **Customer Base vs. Electricity Use and Duration Over Time**
We wanted to know if there was a relationship between the customer base categories `'AFFECTED_BUCKET'`, and its total electricity consumption, `'TOTAL.SALES'`. To explore this, we calculated the average total electricity consumption for each customer base category and visualized it using a bar chart. Outage electricity consumption grew as the customer base size increased, which was expected. The average total electricity consumption across outages serving a "Very Large Customer Base" was around 16.5MWh compared to around 9.8MWh for the "Small Customer Base"
<iframe
  src="assets/bar_region.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

We also wanted to find out if there was a relationship between the `'YEAR'` and `'OUTAGE.DURATION'` columns, to see if there was a correlation between the year and the average duration of power outages. We found that there is a negative relationship between year and energy consumption, with the number of major outages peaking in 2005 before slowly decreasing.
<iframe
  src="assets/line_year.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

## Interesting Aggregates
To further investigate the sales across climate regions, we created a pivot table. We grouped our first pivot table by the column, `CLIMATE.REGION`, and used the `mean()` function to determine the average `TOTAL.SALES` across the months. This allowed us to identify patterns in energy consumption based on climate regions over time.

| CLIMATE.REGION       | January      | February     | March        | April        | May          | June         | July         | August       | September    | October      | November     | December     |
|:---------------------|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|------------:|
| Central             | 9999134.88   | 9704433.5   | 8803031.0   | 8296968.53  | 8610485.55  | 10035573.94 | 12987285.21 | 12126452.0  | 10827517.43 | 9321714.12  | 8800424.8   | 9975352.22  |
| East North Central  | 7115169.25   | 6362546.8   | 6440860.5   | 6914944.64  | 7823724.9   | 7681555.95  | 9408858.55  | 8709003.54  | 8017706.78  | 7431004.14  | 7656789.67  | 8894578.7   |
| Northeast           | 6633505.48   | 7301159.0   | 5800729.46  | 2866264.26  | 5711189.12  | 6769895.62  | 8426713.92  | 9024923.71  | 7462276.09  | 6897058.72  | 3261755.5   | 7670454.57  |
| Northwest          | 7440878.07   | 6698315.31  | 6297616.08  | 7013575.4   | 6016778.71  | 5466119.36  | 7277678.33  | 6321413.79  | 6469954.43  | 6061274.0   | 7188666.57  | 7085207.0   |
| South              | 16044959.81  | 23281753.27 | 21709001.46 | 19045943.3  | 15341953.13 | 20628203.52 | 22035386.1  | 20993897.56 | 22508896.89 | 17128871.44 | 14301226.0  | 14798184.21 |
| Southeast          | 11478360.27  | 11885612.56 | 11779881.22 | 11096123.0  | 11940065.4  | 11730216.44 | 11820852.62 | 16842852.41 | 15557131.0  | 15695574.0  | 8904418.33  | 10545208.33 |
| Southwest          | 3759643.0    | 3374356.93  | 2951417.83  | 2976632.44  | 3276493.33  | 5019965.11  | 5089792.33  | 4101329.0   | 2492126.0   | 3672241.25  | 2226672.0   | 4280192.57  |
| West              | 18323967.46  | 18861674.5  | 19788192.33 | 19451781.43 | 19400558.89 | 21078384.76 | 24839961.07 | 25358698.5  | 24603592.5  | 21710812.95 | 19410953.92 | 20674018.24 |
| West North Central | NaN          | NaN         | 1242527.0   | NaN         | 1433063.0   | 1592024.4   | NaN         | 1563340.75  | NaN         | 1416356.0   | 1490885.5   | 1798136.0   |

Similar to the pivot table above, we also wanted to investigate energy consumption patterns across different temperatures, `CLIMATE.CATEGORY`, for each state. We applied the mean() aggregate function to `TOTAL.SALES`. Below are the first few rows of the table:

| STATE                 | cold        | normal       | warm        |
|:----------------------|------------:|------------:|------------:|
| Alabama              | 7647656.0   | 7193314.5   | 8934377.0   |
| Arizona              | 5425664.57  | 6438748.0   | 5547223.57  |
| Arkansas             | 3880604.86  | 4163933.6   | 3706199.33  |
| California           | 21693834.79 | 22031934.17 | 21332614.62 |
| Colorado             | 4030931.67  | 4421043.75  | 4289402.0   |

# Assessment of Missingness
## NMAR Analysis
The column `'CAUSE.CATEGORY.DETAIL'` is likely to be NMAR because its missingness does not depend on other columns. For example, rows that had severe weather in ‘CAUSE.CATEGORY’ had both missing and non-missing data in `'CAUSE.CATEGORY.DETAIL'`, so it is not dependent on other columns.

To make this column MAR, we would need to visit external databases to see if some utility companies provide `'CAUSE.CATEGORY.DETAIL'` more consistently than others.

## Missingness Dependency
There are various columns where there is missing data. We wanted to find out if the total electricity consumption, `'TOTAL.SALES'`, is dependent on the missingness in the electricity demand loss column, `'DEMAND.LOSS.MW'`. 

<iframe
  src="assets/sales_missing.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

Our set of hypotheses were:

**Null Hypothesis:** The missingness of the data in the `'DEMAND.LOSS.MW'` column does not depend on the total electricity consumption, `'TOTAL.SALES'`.

**Alternative Hypothesis:** The missingness of the data in the `'DEMAND.LOSS.MW'` column does depend on the total electricity consumption, `'TOTAL.SALES'`.

Our test statistic was the difference in means between average `'TOTAL.SALES'` for data without missing `'DEMAND.LOSS.MW'` and the average `'TOTAL.SALES'` for data with missing `'DEMAND.LOSS.MW'`. We got a low p-value of 0.001, which is below the standard 0.05 significance threshold. We obtained a low p-value of 0.001, which is below the standard 0.05 significance threshold. Therefore, we reject the null hypothesis and conclude that the missingness of `'DEMAND.LOSS.MW'` is dependent on `'TOTAL.SALES'`: when `'TOTAL.SALES'` is lower, `'DEMAND.LOSS.MW'` is more likely to be missing. This dependency is possibly due to reporting bias, where locations with less electricity consumption do not have record of the demand loss because it is seen as less impactful. 

<iframe
  src="assets/dependent_hist.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

Next, we wanted to see if the missingness of the electricity demand loss column, `'DEMAND.LOSS.MW'`, depended on the duration of the power outage, `'OUTAGE.DURATION'`. Our set of hypotheses were:

**Null Hypothesis:** The missingness of the data in the `'DEMAND.LOSS.MW'` column does not depend on the length of the power outage, `'OUTAGE.DURATION'`.

**Alternative Hypothesis:** The missingness of the data in the `'DEMAND.LOSS.MW'` column does depend on the length of the power outage, `'OUTAGE.DURATION'`.

<iframe
  src="assets/duration_missing.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

We performed a permutation test by shuffling the "Missing/Not Missing" labels for the `'DEMAND.LOSS.MW'` column, and calculated the difference in average outage durations between both groups.

Our observed difference in duration was 128.76 minutes and our p-value for this permutation test was 0.34. Using a significance level of 1%, we determine that this is a high p-value and therefore fail to reject the null. The difference in mean durations between missing and non-missing demand loss data was due to chance.

<iframe
  src="assets/duration_missing_hist.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

# Hypothesis Testing
We conducted a test to investigate whether there is a significant difference in the average `'TOTAL.SALES'` between the 'West' region and the other regions based on the `'CLIMATE.REGION'` column.

Recall that the average electricity consumption, `'TOTAL.SALES'`, was the highest in the 'West' region. We aim to test if there is a significant difference in the average electricity consumption, `'TOTAL.SALES'`, between the 'West' region and the other regions.

**Null Hypothesis:** The mean `'TOTAL.SALES'` in the Western region is not longer than the mean `'TOTAL.SALES'` in other regions.

**Alternative Hypothesis:** The mean `'TOTAL.SALES'` of power outages in the Western region is longer than the mean `'TOTAL.SALES'` in other regions.

**Test Statistic**: The difference between the observed mean of the 'West' region and the mean from the permutation distribution.

<iframe
  src="assets/hypothesis_test.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

The p-value came out to be extremely small (essentially 0.0), meaning we can reject the null hypothesis. This provides strong evidence that the Western region, on average, has a higher amount of electricity consumption compared to the other regions.

# Framing a Prediction Problem
From our previous section, we found out that regional factors can give a good idea of the total electricity consumption of a state, `'TOTAL.SALES'`. NOOOOOOO

Predicting the region of the outage based off of 

# Baseline Model

# Final Model

# Fairness Analysis

