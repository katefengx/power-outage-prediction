
# Predicting Power Outage Severity
by Kate Feng and Tracy Vu

# Introduction

This project explores how power outage data can be leveraged to predict the number of affected customers during an outage across different regions in the United States. In particular, we are investigating: **How can we predict the number of affected customers based on factors like outage duration, region, and cause?** 

The dataset we are using is Purdue University’s Major Power Outage Risks in the U.S. dataset, which consists of major power outages from January 2000 to July 2016 across the United States. Each of the 1,534 rows represents a single power outage occurrence, with 57 total columns containing information such as the duration of the outage, the state and region in which it occurred, and the category of the outage’s cause.

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

We converted the `'MONTH'` column from numerical values to text values and renamed columns (`'U.S._STATE'` to `'STATE'`, `'POSTAL.CODE'` to `'STATE.ABBV'`) for readability. 

Furthermore, in order to predict the size of the customer base affected, we created another column called `'AFFECTED_BUCKET'` with labels based off of `'CUSTOMERS.AFFECTED'` with these bins:

| **Bin Range (Customers Affected)** | **Label**                 |
|------------------------------------|---------------------------|
| 0 - 10,000                         | Small Customer Base       |
| 10,000 - 100,000                   | Medium Customer Base      |
| 100,000 - 500,000                  | Large Customer Base       |
| 500,000+                           | Very Large Customer Base  |

Below are a sample of rows from our cleaned dataset, with a portion of columns selected.

| STATE.ABBV | CLIMATE.CATEGORY | OUTAGE.DURATION | DEMAND.LOSS.MW | AFFECTED_BUCKET     |
|:--------------:|:----------------:|:---------------:|:---------------:|:-------------------:|
| IN             | cold             | NaN             | 15             | Large Customer Base |
| WI             | cold             | 388             | 30             | Small Customer Base |
| TX             | warm             | 1320            | NaN            | Medium Customer Base |
| OR             | normal           | 989             | NaN            | NaN                 |
| PA             | normal           | 3189            | NaN            | Medium Customer Base |



## Exploratory Data Analysis

### Univariate Analysis

**Customer Base Size Histogram**

For our first univariate plot, we created a histogram to show the distribution of the customer base sizes. We found that most outages affected a **medium to large customer base**, with 371 outages affecting 10,000 to 100,000 customers and 385 outages affecting 100,000 to 500,000 customers. 
<iframe
  src="assets/affected_hist.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

**Chloropleth Map**

We also examined how the number of power outages varied by state. To visualize this, we created a choropleth map showing the distribution of power outages across different states. We grouped the data by `'STATE.ABBV'`, counted the number of rows per group, and added labels for the number of outages in states with over 50 outages. The major states here included **California with 210 outages**, **Texas with 127 outages**, and **Washington with 97 outages** over the 15.5 years of data.
<iframe
  src="assets/outages_map.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

### Bivariate Analysis
#### **Customer Base vs. Duration**
We wanted to know if there was a relationship between the customer base categories `'AFFECTED_BUCKET'`, and the duration of power outages, `'OUTAGE.DURATION'`. To explore this, we calculated the average outage duration for each customer base category and visualized it using a bar chart. Average outage duration grew as the customer base size increased, meaning that **longer power outages affected greater groups of customers**. 

<iframe
  src="assets/bar_region.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

The average outage duration across outages serving a "Very Large Customer Base" was close to 7,000 minutes compared to around 1,000 minutes for the "Small Customer Base". This large difference is likely due to the **North American Cold Wave in January-March 2014** that caused power outages affecting large numbers of customers due to the frigid weather.

#### **Electricity Consumption Over Time**

We also wanted to find out if there was a relationship between the `'YEAR'` and `'TOTAL.SALES'` columns, to see if there was a correlation between the year and the average electricity consumption.
<iframe
  src="assets/line_year.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

We found that there is a negative relationship between year and energy consumption, with the number of major outages peaking in 2001 before slowly decreasing.

## Interesting Aggregates
To further investigate the sales across climate regions, we created a pivot table. We grouped our first pivot table by the column, `CLIMATE.REGION`, and used the `mean()` function to determine the average `TOTAL.SALES` across the months. This allowed us to **identify patterns in energy consumption based on climate regions over time.** 

In general, the Southwest has less total energy consumption throughout the year. Below are a few months of the pivot table.

| MONTH       | January     | May         | August      | December    |
| **CLIMATE.REGION** |             |             |             |             |
|:------------|:-----------:|:-----------:|:-----------:|:-----------:|
| Central              | 9999134.88  | 8610485.55  | 12126452.0  | 9975352.22  |
| East North Central   | 7115169.25  | 7823724.9   | 8709003.54  | 8894578.7   |
| Northeast            | 6633505.48  | 5711189.12  | 9024923.71  | 7670454.57  |
| Southwest            | 3759643.0   | 3276493.33  | 4101329.0   | 4280192.57  |
| West                 | 18323967.46 | 19400558.89 | 25358698.5  | 20674018.24 |
| West North Central   | NaN         | 1433063.0   | 1563340.75  | 1798136.0   |


Similar to the pivot table above, we also wanted to investigate energy consumption patterns across different temperatures, `CLIMATE.CATEGORY`, for each state. We applied the mean() aggregate function to `TOTAL.SALES`. Below are the first few rows of the table.

| CLIMATE.CATEGORY   | cold        | normal      | warm        |
| **STATE**    |         |       |         |
|:------------|:-----------:|:-----------:|:-----------:|:-----------:|
| Alabama      | 7647656.0   | 7193314.5   | 8934377.0   |
| Arizona      | 5425664.57  | 6438748.0   | 5547223.57  |
| Arkansas     | 3880604.86  | 4163933.6   | 3706199.33  |
| California   | 21693834.79 | 22031934.17 | 21332614.62 |
| Colorado     | 4030931.67  | 4421043.75  | 4289402.0   |

# Assessment of Missingness
## NMAR Analysis

The column `'CUSTOMERS.AFFECTED'` is likely NMAR because its missingness does not depend on other columns. Instead, the missingness of this column depends on how each company records the number of customers affected, as **this column aggregates data from multiple companies.** Variations in reporting standards, such as differing thresholds for what constitutes an affected customer or inconsistencies in data collection methods, could lead to systematic patterns of missingness.

However, if we had access to each company's recording standards, the missingness might be MAR rather than NMAR.

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

Our test statistic was the difference in means between average `'TOTAL.SALES'` for data without missing `'DEMAND.LOSS.MW'` and the average `'TOTAL.SALES'` for data with missing `'DEMAND.LOSS.MW'`. We got a low p-value of 0.001, which is below the standard 0.05 significance threshold. We obtained a low p-value of 0.001, which is below the standard 0.05 significance threshold. 

Therefore, we **reject the null hypothesis** and conclude that the missingness of `'DEMAND.LOSS.MW'` is dependent on `'TOTAL.SALES'`: when `'TOTAL.SALES'` is lower, `'DEMAND.LOSS.MW'` is more likely to be missing. 

This dependency is possibly due to reporting bias, where locations with less electricity consumption do not have record of the demand loss because it is seen as less impactful. 

<iframe
  src="assets/dependent_hist.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

Next, we wanted to see if the missingness of the electricity demand loss column, `'DEMAND.LOSS.MW'`, depended on the duration of the power outage, `'OUTAGE.DURATION'`. 

Our set of hypotheses for this test were:

**Null Hypothesis:** The missingness of the data in the `'DEMAND.LOSS.MW'` column does not depend on the length of the power outage, `'OUTAGE.DURATION'`.

**Alternative Hypothesis:** The missingness of the data in the `'DEMAND.LOSS.MW'` column does depend on the length of the power outage, `'OUTAGE.DURATION'`.

<iframe
  src="assets/duration_missing.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

We performed a permutation test by shuffling the "Missing/Not Missing" labels for the `'DEMAND.LOSS.MW'` column, and calculated the difference in average outage durations between both groups.

Our observed difference in duration was 128.76 minutes and our p-value for this permutation test was 0.34. Using a significance level of 5%, we determine that this is a high p-value and therefore **fail to reject the null.** The difference in mean durations between missing and non-missing demand loss data was likely due to chance.

<iframe
  src="assets/duration_missing_hist.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

# Hypothesis Testing
We conducted a test to investigate whether there is a significant difference in the average `'OUTAGE.DURATION'` between the 'Very Large Customer Base' group and the other groups in the `'AFFECTED_BUCKET'` column.

Recall from the first univariate plot that the average outage duration, `'OUTAGE.DURATION'`, was the **highest in the 'Very Large Customer Base' group.** We aim to test if this significant difference is statistically significant or due to random chance.

**Null Hypothesis:** The mean `'OUTAGE.DURATION'` in the 'Very Large Customer Base' group is not longer than the mean `'OUTAGE.DURATION'` in other groups.

**Alternative Hypothesis:** The mean `'OUTAGE.DURATION'` in the 'Very Large Customer Base' group is longer than the mean `'OUTAGE.DURATION'` in other groups.

**Test Statistic**: The difference between the observed mean of the 'Very Large Customer Base' group and the mean from the permutation distribution.

<iframe
  src="assets/hypothesis_test.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

The p-value came out to be extremely small (essentially 0.0), meaning we can **reject the null hypothesis at a 0.01 significance level.** This provides strong evidence that outages affecting 'Very Large Customer Base' group, on average, are longer compared to outages affecting other groups.

# Framing a Prediction Problem

Our model will aim to **predict the number of customers affected by a power outage**. We binned our `'CUSTOMERS.AFFECTED'` column, with the buckets being Small Customer Base (0 - 10,000), Medium Customer Base (10,000 - 100,000), Large Customer Base (100,000 - 500,000), and Very Large Customer Base (500,000+). 

This will be a **multiclass classification** model since there are various buckets that an outage could fall into. We chose to predict this variable because we believe that it would be useful in future efforts of preparation and prioritization regarding power outages.

The metric used will be an **F1-score** since there are multiple classes, meaning that there may be class imbalances, which is why it is better than using accuracy as a metric. We wanted to ensure that our model had both high precision (avoid false positives) and high recall (avoid false negatives).

At the time of prediction, we would know the **outage duration, climate region, cause category, climate category, urban population density, total customers of the state** (annually), **population of the state, total sales of the state** (total electricity consumption), and **demand loss** (peak demand lost during the outage). This provides us with information to predict the size of the customer base that was affected.

# Baseline Model

For our baseline model, we are using a Random Forest Classifier with these four features: `'CLIMATE.REGION'`, `'CLIMATE.CATEGORY'`, `'CAUSE.CATEGORY'`, `'OUTAGE.DURATION'`. The first three variables, `'CLIMATE.REGION'`, `'CLIMATE.CATEGORY'`, `'CAUSE.CATEGORY'`, contain nominal values while the last variable, `'OUTAGE.DURATION'`, contains continuous quantitative values of minutes. 

These features were chosen since we believed they helped utility companies understand which outages are likely to have a larger impact, based on our previous analyses. 

A higher duration value could represent higher severity in an outage. Cause category reveals the outage cause and climate category reveals the climate type, both of which may impact duration and severity. Climate region shows the geographic region, which provides information for severity and customers affected. 
These features provide key insights for affected customers.

We one hot encoded the nominal variables, `'CLIMATE.REGION'`, `'CLIMATE.CATEGORY'`, `'CAUSE.CATEGORY'` to ensure that they were properly interpreted by the classifier. We passed `'OUTAGE.DURATION'` through as-is because applying transformations like Standard Scaler would not improve the accuracy since the Random Forest model splits data based on thresholds in each feature.

The metric, F1 score, of this model is 0.55. The F1 score for each customer base bucket is shown below:

| Class                         | Precision | Recall | F1-Score | Support |
|-------------------------------|-----------|--------|----------|---------|
| Large Customer Base            | 0.48      | 0.53   | 0.50     | 73      |
| Medium Customer Base           | 0.42      | 0.45   | 0.43     | 67      |
| Small Customer Base            | 0.92      | 0.85   | 0.89     | 55      |
| Very Large Customer Base       | 0.00      | 0.00   | 0.00     | 16      |
| **Accuracy**                   |           |        | **0.55** | **211** |
| **Macro avg**                  | 0.45      | 0.46   | 0.46     | 211     |
| **Weighted avg**               | 0.54      | 0.55   | 0.54     | 211     |

This model best predicts 'Small Customer Base', likely because there are clear patterns (such as shorter outage duration or easily-fixed outage cause) to predict an outage affecting less people. 

The accuracy for the 'Very Large Customer Base' was 0, meaning this model was not able to predict outages affecting over 500,000 customers at all. This is likely due to the small number of examples in the dataset for this category, with 16 examples. This model ended up having a mediocre F1-score, so there is a lot of room for improvement.

# Final Model

Since our accuracy for the last model was low, we wanted to analyze **which features we should continue using for our final model** and which features were decreasing the accuracy. 

<iframe
  src="assets/first_importance.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

Upon plotting the feature importances, we found that the variables `'CLIMATE.REGION'` and `'CLIMATE.CATEGORY'` contributed very little to predicting customer base size. In contrast, certain values in `'CAUSE.CATEGORY'`, such as 'severe weather' and 'intentional attack', had higher importance. Notably, `'OUTAGE.DURATION'` had the highest importance, indicating that the model relied heavily on this feature when making splits.

For the final model, we **removed the two non-contributing features and added five more new features,** bringing the total to seven features: `'CAUSE.CATEGORY'`, `'POPDEN_URBAN'`, `'TOTAL.CUSTOMERS'`, `'OUTAGE.DURATION'`, `'POPULATION'`, `'TOTAL.SALES'`, and `'DEMAND.LOSS.MW'`.

The additional features included:
- **Population** (discrete), representing the size of the area impacted by the outage.
- **Total Sales** (continuous), reflecting the energy consumption in a region and helping to express the severity of the outage.
- **Total Customers** (discrete), indicating the number of customers in the affected state, which can give a better sense of the potential impact.
- **Demand Loss** (continuous), which relates to the economic impact of the outage, highlighting its severity.
- **Urban Population Density** (discrete), which provides insight into how densely populated an area is, and potentially how many people would be affected by the outage.

We included these additional features because they seemed to represent critical factors affecting the impact of outages on customers. Features like population and urban density help to capture the size of the affected area, while total sales and demand loss give a better gauge of the outage’s economic and energy impact. Together, these factors enhance our model’s ability to predict the severity of outages on customers.

We used GridSearchCV to find the optimized hyperparameters for the Random Forest Classifier:
- max_features: 'sqrt'
- min_samples_leaf: 2
- min_samples_split': 5
- n_estimators': 300

However, when we looked at the classification report, we found that the F1-scores across the report **were lower compared to the default parameters**:

| Class                         | Precision | Recall | F1-Score | Support |
|-------------------------------|-----------|--------|----------|---------|
| Large Customer Base            | 0.53      | 0.75   | 0.62     | 32      |
| Medium Customer Base           | 0.74      | 0.60   | 0.67     | 48      |
| Small Customer Base            | 0.98      | 0.96   | 0.97     | 52      |
| Very Large Customer Base       | 0.67      | 0.33   | 0.44     | 6       |
| **Accuracy**                   |           |        | **0.76** | **138** |
| **Macro avg**                  | 0.73      | 0.66   | 0.68     | 138     |
| **Weighted avg**               | 0.78      | 0.76   | 0.76     | 138     |

We can compare these scores to the scores using these default parameters: 
- max_features: 'auto' (model will consider all features)
- min_samples_leaf: 1
- min_samples_split: 2
- n_estimators: 100

In the default parameters, the F1 score of this improved model is 0.80. The F1 score for each customer base bucket is shown below:

| Class                         | Precision | Recall | F1-Score | Support |
|-------------------------------|-----------|--------|----------|---------|
| Large Customer Base            | 0.60      | 0.75   | 0.67     | 32      |
| Medium Customer Base           | 0.77      | 0.62   | 0.69     | 48      |
| Small Customer Base            | 0.96      | 0.98   | 0.97     | 52      |
| Very Large Customer Base       | 0.83      | 0.83   | 0.83     | 6       |
| **Accuracy**                   |           |        | **0.80** | **138** |
| **Macro avg**                  | 0.79      | 0.80   | 0.79     | 138     |
| **Weighted avg**               | 0.81      | 0.80   | 0.80     | 138     |

The decreased accuracy in the hyperparameter tuning is likely due to overfitting of the training data or class imbalance (the model becomes more sensitive to majority classes). Therefore, we will use the default parameters instead of the hyperparameters found with GridSearchCV.

The **overall F1-Score improved to 0.80**, reflecting an increase in accuracy across all customer base categories. Notably, the 'Very Large Customer Base' saw a significant improvement, with its **F1-Score rising from 0.0 to 0.83**. Despite the sample size being small (only 6 samples), the model performed much better due to the inclusion of more features, which allowed the 'Very Large Customer Base' to be more adequately represented.

Additionally, the accuracy for other categories also showed improvement. For example, the 'Small Customer Base' now has an **impressive F1-Score of 0.97**, demonstrating that the model can effectively predict this category. 

These improvements suggest that with more features and better representation, the model has become more capable of distinguishing between the various customer base categories.

We can visualize the prediction accuracy in a confusion matrix:
<iframe
  src="assets/confusion_matrix.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

# Fairness Analysis

Our groups to test the model’s fairness are whether the cause of the outage, `'CAUSE.CATEGORY'`, was **system operability disruption** or not. We used the accuracy as the evaluation metric since it represents the proportion of correct predictions made for both groups. This metric suits the classification problem as we are trying to see whether the model is accurate in distinguishing between severe weather and other outage causes.

We decided to test the model’s fairness on these groups because we found that the **false negative rates of outages caused by system operability disruption were the highest out of all the cause categories.** This raised our concerns about whether the model was underperforming for this cause.

<iframe
  src="assets/fnr_cause.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

**Null Hypothesis:** The model is fair. Its accuracy for system operability disruption and non-system operability disruption cases are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis:** The model is unfair. Its accuracy for system operability disruption and non-system operability disruption cases are significantly different.

**Test Statistic:** The difference in accuracy between "system operability disruption" and the rest of the categories.

We conducted a permutation test with 5,000 trials to assess whether the accuracy of our model differs when predicting power outages caused by "system operability disruption" compared to all other causes. Our observed accuracy difference was -0.21, and we obtained a p-value of 0.079. Since this p-value exceeds the standard significance threshold of 0.05, we **fail to reject the null hypothesis.** 

These results suggest that the observed lower accuracy for system operability disruption is not statistically significant and **may be due to random variation rather than a true effect.** While there appears to be an association between system operability disruption and reduced accuracy, the evidence is insufficient to conclude that this difference is meaningful.

<iframe
  src="assets/fairness_perm.html"
  width="800"
  height="420"
  frameborder="0"
></iframe>

