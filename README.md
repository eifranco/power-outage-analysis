# When the Lights Go Out: An Analysis of Major Power Outages

**By:** Adam Hamadene, Emiliano Franco

## Introduction

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

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis