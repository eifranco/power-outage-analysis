# When the Lights Go Out: An Analysis of Major Power Outages

**By:** Adam Hamadene, Emiliano Franco


## Introduction

Power outages are more than temporary inconveniences. Major outages can interrupt transportation, communication, business operations, emergency services, and daily life for thousands or even millions of people. In this project, we analyze a dataset of major power outages in the continental United States from January 2000 to July 2016. Each row in the dataset represents one major outage event, with information about where and when the outage happened, what caused it, how long it lasted, and how many customers were affected.

The central question of this project is:

**What characteristics are associated with more severe power outages, especially longer outage durations?**

This question is important because outage severity is not evenly distributed across the country. Some outages last longer because of severe weather, regional infrastructure, demand stress, or other local conditions. By studying outage duration alongside causes, locations, climate information, and customer impact, we can better understand which factors are connected to more disruptive outage events.

After cleaning the dataset, there are **1534 rows**, with each row representing a major power outage. The columns most relevant to this analysis are:

| Column                  | Description                                                                                                      |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `U.S._STATE`            | The U.S. state where the outage occurred.                                                                        |
| `YEAR`                  | The year when the outage occurred.                                                                               |
| `MONTH`                 | The month when the outage occurred.                                                                              |
| `CLIMATE.REGION`        | The climate region associated with the outage location.                                                          |
| `CLIMATE.CATEGORY`      | The climate category at the time of the outage.                                                                  |
| `CAUSE.CATEGORY`        | The broad cause of the outage, such as severe weather, intentional attack, or system operability disruption.     |
| `CAUSE.CATEGORY.DETAIL` | A more specific description of the outage cause.                                                                 |
| `OUTAGE.START`          | A timestamp column created by combining the original outage start date and outage start time.                    |
| `OUTAGE.RESTORATION`    | A timestamp column created by combining the original restoration date and restoration time.                      |
| `OUTAGE.DURATION`       | The length of the outage, measured in minutes. This is the main measure of outage severity used in this project. |
| `CUSTOMERS.AFFECTED`    | The number of customers affected by the outage.                                                                  |
| `DEMAND.LOSS.MW`        | The amount of electricity demand lost during the outage, measured in megawatts.                                  |
| `POPULATION`            | The population of the state where the outage occurred.                                                           |
| `POPPCT_URBAN`          | The percentage of the state population living in urban areas.                                                    |

## Data Cleaning and Exploratory Data Analysis

The first step in this project was cleaning the outage dataset so that it could be used for analysis. Since the data was stored in an Excel file, the spreadsheet included extra rows and formatting information that were not actual outage observations.

### Data Cleaning

We started by loading the Excel file using the correct header row. The first few rows of the spreadsheet contained title information, notes, and unit descriptions, so we skipped the unnecessary rows and removed the row that only contained units. We also dropped the variables column because it was part of the spreadsheet formatting and did not provide useful information for my analysis.

Next, we combined the separate outage start date and start time columns into one timestamp column called OUTAGE.START. We did the same for the restoration date and restoration time columns, creating a new timestamp column called OUTAGE.RESTORATION. This made the dataset easier to work with because each outage now has one clear start timestamp and one clear restoration timestamp.

After creating the timestamp columns, we created additional time-based columns from OUTAGE.START: START.YEAR, START.MONTH, START.HOUR, and START.DAY_OF_WEEK. These columns allow me to analyze whether outage patterns vary by year, month, time of day, or day of the week.

We also created a COMPUTED.DURATION column by subtracting OUTAGE.START from OUTAGE.RESTORATION and converting the result into minutes. We compared this computed duration to the original OUTAGE.DURATION column to check whether the provided duration values were consistent with the timestamp data. Most values matched, but some differed by exactly 60 minutes, which may be due to time recording conventions or daylight saving time. Because OUTAGE.DURATION was provided directly in the original dataset, we kept it as the main duration column for my analysis.

The main columns we focused on for this part of the project include YEAR, MONTH, U.S._STATE, NERC.REGION, CLIMATE.REGION, CLIMATE.CATEGORY, CAUSE.CATEGORY, OUTAGE.DURATION, DEMAND.LOSS.MW, CUSTOMERS.AFFECTED, OUTAGE.START, and OUTAGE.RESTORATION.

The first few rows of the cleaned DataFrame are shown below, with a smaller set of columns selected for readability.

| U.S._STATE   | NERC.REGION   | CLIMATE.REGION     | CAUSE.CATEGORY     |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED | OUTAGE.START        |
|:-------------|:--------------|:-------------------|:-------------------|------------------:|---------------------:|:--------------------|
| Minnesota    | MRO           | East North Central | severe weather     |              3060 |                70000 | 2011-07-01 17:00:00 |
| Minnesota    | MRO           | East North Central | intentional attack |                 1 |                  nan | 2014-05-11 18:38:00 |
| Minnesota    | MRO           | East North Central | severe weather     |              3000 |                70000 | 2010-10-26 20:00:00 |
| Minnesota    | MRO           | East North Central | severe weather     |              2550 |                68200 | 2012-06-19 04:30:00 |
| Minnesota    | MRO           | East North Central | severe weather     |              1740 |               250000 | 2015-07-18 02:00:00 |


### Univariate Analysis
The histogram below shows the distribution of `OUTAGE.DURATION`, measured in minutes. we used a log scale on the y-axis because outage durations are highly right-skewed: most outages are relatively short compared to the longest outages, but a small number of major outages last for much longer. Only 7 outages out of 1534 lasted longer than 40,000 minutes.

<iframe
  src="assets/outage-duration-distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

The bar chart below shows the number of major power outages in each `CAUSE.CATEGORY`. This helps summarize which causes appear most frequently in the dataset. Severe weather is one of the most common categories, which supports looking more closely at whether severe weather outages are also associated with longer outage durations.

<iframe
  src="assets/cause-category-counts.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


The bar chart below shows the top 15 states by average outage duration after removing outages longer than 40,000 minutes. We removed these extremely long outages so that a small number of outliers would not dominate the state averages. This plot shows that average outage duration varies noticeably by state, suggesting that location may be associated with outage severity. However, these averages should be interpreted carefully because some states may have fewer outage records than others.

<iframe
  src="assets/avg-duration-by-state-under-40k.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


The box plot below shows the distribution of `OUTAGE.DURATION` across different `CAUSE.CATEGORY` values after removing outages longer than 40,000 minutes. we excluded these extreme values so that the main patterns across cause categories would be easier to interpret. This plot suggests that outage duration varies noticeably by cause, with some categories—especially severe weather—showing longer typical outage durations and greater variability than others.

<iframe
  src="assets/outage-duration-by-cause-under-40k.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates
The table below groups outages by `CAUSE.CATEGORY` and summarizes several measures of outage severity. For each cause category, we calculated the number of outages, the average outage duration, the median outage duration, the average number of customers affected, and the median number of customers affected. This table is useful because it compares both the frequency and severity of different outage causes. For example, a cause category may not have the most outages overall, but it may still be associated with longer durations or more customers affected.

| CAUSE.CATEGORY                |   outage_count |   avg_duration |   median_duration |   avg_customers_affected |   median_customers_affected |
|:------------------------------|---------------:|---------------:|------------------:|-------------------------:|----------------------------:|
| fuel supply emergency         |             38 |       13484    |            3960   |                     0.14 |                         0   |
| severe weather                |            744 |        3883.99 |            2460   |                188575    |                    110433   |
| equipment failure             |             55 |        1816.91 |             221   |                101936    |                     45451.5 |
| public appeal                 |             69 |        1468.45 |             455   |                  7618.76 |                         0   |
| system operability disruption |            123 |         728.87 |             215   |                211066    |                     69000   |
| intentional attack            |            403 |         429.98 |              56   |                  1790.53 |                         0   |
| islanding                     |             44 |         200.55 |              77.5 |                  6169.09 |                      2342.5 |




## Assessment of Missingness

### NMAR Analysis

Several columns in the dataset contain missing values. One column that could possibly be **NMAR** is `CUSTOMERS.AFFECTED`, which records the number of customers affected by a major power outage. This column could be NMAR because the likelihood of a value being missing may depend on the unreported customer count itself. For example, if the number of affected customers was difficult to estimate, unusually uncertain, or not tracked consistently by the reporting utility, then the value may have been less likely to be reported. Since the dataset combines outage records from different sources, differences in reporting practices could also contribute to missing values.

Additional data that could help determine whether `CUSTOMERS.AFFECTED` is actually MAR would include information about the reporting utility company or agency for each outage. If missingness in `CUSTOMERS.AFFECTED` depends on which company reported the outage, then the missingness could be explained by an observed variable and would be more consistent with MAR. Other useful information would include whether each reporting company was required to report customer impact and whether the outage occurred in an area with reliable customer tracking systems.

### Missingness Dependency

Before choosing a column for the missingness dependency analysis, we looked at the columns with the highest rates of missingness. The table below shows the columns with the most missing values in the cleaned dataset.


| column                  |   missing_count |   missing_percent |
|:------------------------|----------------:|------------------:|
| HURRICANE.NAMES         |            1462 |             95.31 |
| DEMAND.LOSS.MW          |             705 |             45.96 |
| CAUSE.CATEGORY.DETAIL   |             471 |             30.7  |
| CUSTOMERS.AFFECTED      |             443 |             28.88 |
| OUTAGE.RESTORATION.DATE |              58 |              3.78 |
| OUTAGE.RESTORATION      |              58 |              3.78 |
| COMPUTED.DURATION       |              58 |              3.78 |
| OUTAGE.DURATION         |              58 |              3.78 |
| OUTAGE.RESTORATION.TIME |              58 |              3.78 |
| IND.PRICE               |              22 |              1.43 |

To test missingness dependency, we focused on the missingness of `DEMAND.LOSS.MW`, which records the amount of electricity demand lost during an outage in megawatts. We chose this column because it has substantial missingness: 705 values are missing, or about 45.96% of the dataset. This gives us enough missing and non-missing observations to compare patterns across other columns.

We did not use `HURRICANE.NAMES` because its missingness is expected from the data-generating process: most outages are not hurricanes, so most rows should not have a hurricane name. We also did not use `CAUSE.CATEGORY.DETAIL` because it is a more specific follow-up description of `CAUSE.CATEGORY`, and missingness there may simply mean that no detailed cause was recorded. `DEMAND.LOSS.MW` is more directly related to outage severity and is more relevant to our project question, which focuses on what characteristics are associated with more severe outages.

We created a Boolean column indicating whether `DEMAND.LOSS.MW` was missing for each outage. Then, we used permutation tests to determine whether the missingness of this column depends on other observed columns in the dataset.


#### Cause Category

First, we examined whether the missingness of `DEMAND.LOSS.MW` depends on `CAUSE.CATEGORY`.

The plot below compares the distribution of `CAUSE.CATEGORY` for outages where `DEMAND.LOSS.MW` is missing versus outages where it is not missing. The two distributions are visibly different, especially for categories such as intentional attack, severe weather, and system operability disruption. This visual difference motivates the permutation test using total variation distance.

<iframe
  src="assets/missingness-cause-distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Null Hypothesis:** The distribution of `CAUSE.CATEGORY` is the same when `DEMAND.LOSS.MW` is missing and when it is not missing.

**Alternative Hypothesis:** The distribution of `CAUSE.CATEGORY` is different when `DEMAND.LOSS.MW` is missing compared to when it is not missing.

Since `CAUSE.CATEGORY` is categorical, we used total variation distance as the test statistic. The observed TVD was approximately **0.179**, and the p-value from the permutation test was **less than 0.001**. Since this p-value is below the significance level of 0.05, we reject the null hypothesis. This suggests that the distribution of `CAUSE.CATEGORY` is significantly different depending on whether `DEMAND.LOSS.MW` is missing. In other words, the missingness of `DEMAND.LOSS.MW` appears to depend on `CAUSE.CATEGORY`.

This result suggests that the missingness of `DEMAND.LOSS.MW` is not MCAR, since it depends on an observed column in the dataset. Because the missingness is related to an observed variable, this result is more consistent with MAR.

<iframe
  src="assets/missingness-cause-category.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Start Day of Week

Next, we examined whether the missingness of `DEMAND.LOSS.MW` depends on `START.DAY_OF_WEEK`.

**Null Hypothesis:** The distribution of `START.DAY_OF_WEEK` is the same when `DEMAND.LOSS.MW` is missing and when it is not missing.

**Alternative Hypothesis:** The distribution of `START.DAY_OF_WEEK` is different when `DEMAND.LOSS.MW` is missing compared to when it is not missing.

Again, we used total variation distance as the test statistic because `START.DAY_OF_WEEK` is categorical. The observed TVD was **0.056**, and the p-value from the permutation test was **0.29**. Since this p-value is **greater** than 0.05, we **fail to reject** the null hypothesis. This suggests that the missingness of `DEMAND.LOSS.MW` **does not appear to** depend on `START.DAY_OF_WEEK`.

<iframe
  src="assets/missingness-day-of-week.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing

For my hypothesis test, we wanted to investigate whether outages caused by severe weather tend to be more severe than outages caused by other factors. Since my project focuses on outage severity, we used `OUTAGE.DURATION` as the main severity measure.

The two columns used in this hypothesis test were:

| Column            | Description                                                                                                                       |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `CAUSE.CATEGORY`  | The broad cause of the outage. we used this column to separate outages into severe weather outages and non-severe-weather outages. |
| `OUTAGE.DURATION` | The duration of the outage in minutes. we used this column to measure outage severity.                                             |

### Hypotheses

**Null Hypothesis:**
The duration of major power outages caused by severe weather and the duration of major power outages caused by other causes come from the same distribution. Any observed difference in outage duration is due to random chance.

**Alternative Hypothesis:**
Major power outages caused by severe weather tend to last longer than major power outages caused by other causes.

### Test Statistic

The test statistic we used was the difference in mean outage duration between severe weather outages and non-severe-weather outages:

**Mean duration of severe weather outages − mean duration of non-severe-weather outages**

This is a valid test statistic because the alternative hypothesis asks whether severe weather outages tend to have longer durations. Larger positive values of this statistic support the alternative hypothesis.

### Permutation Test

To perform the permutation test, we first removed rows with missing values in either `CAUSE.CATEGORY` or `OUTAGE.DURATION`. Then, we created a Boolean column indicating whether each outage was caused by severe weather. Under the null hypothesis, the severe weather labels should not be meaningfully related to outage duration, so we repeatedly shuffled the severe weather labels and recalculated the difference in mean outage duration.

The observed difference in mean outage duration was **2537 minutes**. This means that, in the observed dataset, severe weather outages lasted about 2537 minutes longer on average than outages caused by other factors.

The p-value from the permutation test was **less than 0.001**. Although the computed p-value appeared as 0, this means that none of the simulated test statistics from the permutation test were as large as the observed statistic. Therefore, it is better to report the p-value as less than 0.001 rather than exactly 0.

Using a significance level of 0.05, we reject the null hypothesis. The data provide evidence that major power outages caused by severe weather tend to last longer than major power outages caused by other causes.

<iframe
  src="assets/hypothesis-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis