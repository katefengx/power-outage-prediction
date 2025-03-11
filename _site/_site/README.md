# Power Outage Prediction
by Kate Feng and Tracy Vu

# Introduction

This project examines the regional impact on the severity of power outages in the United States. In particular, we are investigating: Do power outages last longer in the Northeast Midwest region compared to other regions of the U.S.? The dataset we are using is Purdue University’s Major Power Outage Risks in the U.S. dataset, which consists of major power outages, defined by the Department of Energy as those affecting over 50,000 customers or resulting in a loss of over 300 megawatts of energy. The data spans from January 2000 to July 2016 across the United States. Each of the 1,534 rows represents a single power outage occurrence, with 57 total columns containing information such as the duration of the outage, the state and region in which it occurred, and the category of the outage’s cause.

The columns we included in our analysis are:
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
- `'DEMAND.LOSS.MW'` - This column represents the amount of energy (in megawatts) that the customers affected by the outage demanded during the duration of the outage. This column, as well as the CUSTOMERS.AFFECTED column, provide an examination into whether public pressure affects the speed of outage repair.

## Data Cleaning and Exploratory Data Analysis
this is the info

## Data Cleaning
this is info

## Exploratory Data Analysis

### Univariate Analysis: ____

### Bivariate Analysis: ____ 


# Assessment of Missingness

## NMAR Analysis
## Missingness Dependency


# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis

