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

 <div id="df-d71f3f75-a832-49d5-8380-0a9a53fcd151" class="colab-df-container">
    <div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VIN (1-10)</th>
      <th>County</th>
      <th>City</th>
      <th>State</th>
      <th>Postal Code</th>
      <th>Model Year</th>
      <th>Make</th>
      <th>Model</th>
      <th>Electric Vehicle Type</th>
      <th>Clean Alternative Fuel Vehicle (CAFV) Eligibility</th>
      <th>Electric Range</th>
      <th>Base MSRP</th>
      <th>Legislative District</th>
      <th>DOL Vehicle ID</th>
      <th>Vehicle Location</th>
      <th>Electric Utility</th>
      <th>2020 Census Tract</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>WAUTPBFF4H</td>
      <td>King</td>
      <td>Seattle</td>
      <td>WA</td>
      <td>98126.0</td>
      <td>2017</td>
      <td>AUDI</td>
      <td>A3</td>
      <td>Plug-in Hybrid Electric Vehicle (PHEV)</td>
      <td>Not eligible due to low battery range</td>
      <td>16</td>
      <td>0</td>
      <td>34.0</td>
      <td>235085336</td>
      <td>POINT (-122.374105 47.54468)</td>
      <td>CITY OF SEATTLE - (WA)|CITY OF TACOMA - (WA)</td>
      <td>5.303301e+10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>WAUUPBFF2J</td>
      <td>Thurston</td>
      <td>Olympia</td>
      <td>WA</td>
      <td>98502.0</td>
      <td>2018</td>
      <td>AUDI</td>
      <td>A3</td>
      <td>Plug-in Hybrid Electric Vehicle (PHEV)</td>
      <td>Not eligible due to low battery range</td>
      <td>16</td>
      <td>0</td>
      <td>22.0</td>
      <td>237896795</td>
      <td>POINT (-122.943445 47.059252)</td>
      <td>PUGET SOUND ENERGY INC</td>
      <td>5.306701e+10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5YJSA1E22H</td>
      <td>Thurston</td>
      <td>Lacey</td>
      <td>WA</td>
      <td>98516.0</td>
      <td>2017</td>
      <td>TESLA</td>
      <td>MODEL S</td>
      <td>Battery Electric Vehicle (BEV)</td>
      <td>Clean Alternative Fuel Vehicle Eligible</td>
      <td>210</td>
      <td>0</td>
      <td>22.0</td>
      <td>154498865</td>
      <td>POINT (-122.78083 47.083975)</td>
      <td>PUGET SOUND ENERGY INC</td>
      <td>5.306701e+10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1C4JJXP62M</td>
      <td>Thurston</td>
      <td>Tenino</td>
      <td>WA</td>
      <td>98589.0</td>
      <td>2021</td>
      <td>JEEP</td>
      <td>WRANGLER</td>
      <td>Plug-in Hybrid Electric Vehicle (PHEV)</td>
      <td>Not eligible due to low battery range</td>
      <td>25</td>
      <td>0</td>
      <td>20.0</td>
      <td>154525493</td>
      <td>POINT (-122.85403 46.856085)</td>
      <td>PUGET SOUND ENERGY INC</td>
      <td>5.306701e+10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5YJ3E1EC9L</td>
      <td>Yakima</td>
      <td>Yakima</td>
      <td>WA</td>
      <td>98902.0</td>
      <td>2020</td>
      <td>TESLA</td>
      <td>MODEL 3</td>
      <td>Battery Electric Vehicle (BEV)</td>
      <td>Clean Alternative Fuel Vehicle Eligible</td>
      <td>308</td>
      <td>0</td>
      <td>14.0</td>
      <td>225996361</td>
      <td>POINT (-120.524012 46.5973939)</td>
      <td>PACIFICORP</td>
      <td>5.307700e+10</td>
    </tr>
  </tbody>
</table>
</div>

## <a id="overview"></a> Broad Overview
- Key takeaways are as follows
  
   -<ins>**Top Left**</ins> : Number of Electric Vehicles registered are increasing. Just for the **first 3 months of 2024**, we see number of cars of 2024 model year are at 
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
