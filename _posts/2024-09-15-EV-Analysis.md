---
layout: post
title: Analysis of EV Trends in State of Washington
image: "/posts/credit_card.png"
tags: [Data Analysis, Vizualization, Seaborn, Geopandas, Python]
---

# Analysis of Electric Vehicle Adoption in State of Washington


Please refer to the github repository for notebook [GitHub](https://github.com/paiatul5/credit_card_delinquency)

This project is based on Electric Vehicles Data published on Government of Washington's website: https://data.wa.gov/Transportation/Electric-Vehicle-Population-Data/f6w7-q2d2/about_data
Below are the findings based on the Analysis

## Table of Contents
1. [Dataset](#dataset)
2. [Broad Overview](#overview)
3. [Dataset](#dataset)
4. [Metric of Consideration](#metric-of-consideration)
5. [Models](#models)
6. [Results](#results)
7. [Final Model: XGBoost with Oversampling](#final-model-xgboost-with-oversampling)
8. [Local Interpretability (LIME)](#local-interpretability-lime)


---

## <a id="dataset"></a> Dataset

- Registration data is as of 03-31-2024. All comparisons are done keeping in mind that we only have 3 months of data for 2024.
- This is a snapshot of the data, there are 181458 vehicle registrations in the registry.

- Below is an overview of the key columns:

- **VIN (1-10):** First 10 digits of the Vehicle Identification Number.
- **County, City, State, Postal Code:** Geographic details where the vehicle is registered.
- **Model Year:** Year of the vehicle's model.
- **Make, Model:** The manufacturer (e.g., Tesla, Audi) and model of the vehicle.
- **Electric Vehicle Type:** Indicates whether the vehicle is a:
  - **Battery Electric Vehicle (BEV)**
  - **Plug-in Hybrid Electric Vehicle (PHEV)**
- **CAFV Eligibility:** Specifies if the vehicle qualifies as a Clean Alternative Fuel Vehicle.
- **Electric Range:** The vehicle's range on electric power alone (in miles).
- **Base MSRP:** Manufacturer’s Suggested Retail Price.
- **Legislative District:** Legislative district where the vehicle is registered.
- **DOL Vehicle ID:** Unique ID for the vehicle from the Department of Licensing.
- **Vehicle Location:** Geographical coordinates (longitude, latitude) of the vehicle's location.
- **Electric Utility:** The electric utility provider servicing the vehicle’s location.
- **2020 Census Tract:** The census tract where the vehicle is registered.
<br>
## <a id="overview"></a> Broad Overview
- Key takeaways are as follows
  
   - <ins>**Top Left**</ins> : Number of Electric Vehicles registered are increasing. Just for the **first 3 months of 2024**, we see number of cars of 2024 model year are at 
                    **par with full year's data for 2020!**. This signals a widespread adoption of Electric Vehicles in the State of Washington.
   - <ins>**Top Right**</ins>  : **Tesla** by far is the **most popular** Electric Vehicle registered in Washington. It is followed by the traditional automalers like **Nissan, 
                     Chevrolet, Ford and Kia**. We will consider them to be most popular makers from here on. These numbers are based on models from 1997 to 2024.
   - <ins>**Bottom Left**</ins>  : Market Share of **Tesla** has been fairly consistent over the years, but there is a **dip in 2024 models**(at least for the first 3 months). We see 
                    **Ford, Nissan, and Chevrolet lose** market share as well!. **Kia and BMW** are **picking up** market share, however there seem to be a few **new entrants**
                    that are capitalizing on Tesla's dip.
   - <ins>**Bottom Right**</ins>  : We see the most popular EV manufacturers as a group have **increased their offerings over the years**. We see **BMW and Kia** have started to push 
                        **more models from 2015**, while Nissan, Ford, and Chevrolet have been more or less consistent in the number of offerings. Tesla too has 
                        increased it's offerings. So it is **not the case of lesser models that is causing the dip in their market share**.
![alt text](/img/posts/chart_1.png "Overview of Electric Vehicles")


## <a id="metric-of-consideration"></a>Metric of Consideration:
- Optimized for **Class 1 recall** to capture as many delinquencies as possible, minimizing false negatives.
- **Drawback**: Higher false positives, leading to additional work.

## <a id="models"></a>Models:
### Baseline:
- **XGBoost** with no oversampling.

### Oversampling Models:
1. **Logistic Regression**: Optimized using oversampling.
2. **Random Forest**: Used oversampling with tuning of hyperparameters.
3. **Stacked Model : Logistic Regression & Random Forest**
4. **XGBoost**: Employed oversampling and hyperparameter tuning.

## <a id="final-model-xgboost-with-oversampling"></a>Final Model: XGBoost with Oversampling
- **Recall Score**: 78% for Class 1 (delinquent accounts).
- **Drawback**: 21% of flagged accounts are false positives.

## <a id="results"></a>Results:
- The final **XGBoost model** achieved a **recall score of 78%** for predicting delinquent accounts.
- This means that out of all delinquent accounts, the model successfully identified 78%.
- However, **21% of flagged accounts** were false positives, meaning that the model predicted them to be delinquent, but they were not.
- The model flagged **25% of the total records** as potentially delinquent. Focusing on these flagged records helps to catch 78% of delinquencies.

## <a id="local-interpretability-lime"></a>Local Interpretability (LIME):
- Analyzed instances where the model was correct and incorrect in predicting delinquency.
