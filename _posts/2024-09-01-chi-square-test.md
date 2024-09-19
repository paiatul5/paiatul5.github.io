---
layout: post
title: Assessing Campaign Performance Using Chi-Square Test For Independence
image: "/posts/ab-testing-title-img.png"
tags: [AB Testing, Hypothesis Testing, Chi-Square, Python]
---

In this project we apply Chi-Square Test For Independence (a Hypothesis Test) to assess the performance of two types of mailers that were sent out to promote a new service! 

# Table of contents

- [00. Project Overview](#overview-main)
    - [Context](#overview-context)
    - [Actions](#overview-actions)
    - [Results & Discussion](#overview-results)
- [01. Concept Overview](#concept-overview)
- [02. Data Overview & Preparation](#data-overview)
- [03. Applying Chi-Square Test For Independence](#chi-square-application)
- [04. Analysing The Results](#chi-square-results)
- [05. Discussion](#discussion)

___

# Project Overview  <a name="overview-main"></a>

### Context <a name="overview-context"></a>

Earlier in the year, our client, a grocery retailer, ran a campaign to promote their new "Delivery Club" - an initiative that costs a customer $100 per year for membership, but offers free grocery deliveries rather than the normal cost of $10 per delivery.

For the campaign promoting the club, customers were put randomly into three groups - the first group received a low quality, low cost mailer, the second group received a high quality, high cost mailer, and the third group were a control group, receiving no mailer at all.

The client knows that customers who were contacted, signed up for the Delivery Club at a far higher rate than the control group, but now want to understand if there is a significant difference in signup rate between the cheap mailer and the expensive mailer.  This will allow them to make more informed decisions in the future, with the overall aim of optimising campaign ROI!

<br>
<br>
### Actions <a name="overview-actions"></a>

For this test, as it is focused on comparing the *rates* of two groups - we applied the Chi-Square Test For Independence.  Full details of this test can be found in the dedicated section below.

**Note:** Another option when comparing "rates" is a test known as the *Z-Test For Proportions*.  While, we could absolutely use this test here, we have chosen the Chi-Square Test For Independence because:

* The resulting test statistic for both tests will be the same
* The Chi-Square Test can be represented using 2x2 tables of data - meaning it can be easier to explain to stakeholders
* The Chi-Square Test can extend out to more than 2 groups - meaning the client can have one consistent approach to measuring signficance

From the *campaign_data* table in the client database, we isolated customers that received "Mailer 1" (low cost) and "Mailer 2" (high cost) for this campaign, and excluded customers who were in the control group.

We set out our hypotheses and Acceptance Criteria for the test, as follows:

**Null Hypothesis:** There is no relationship between mailer type and signup rate. They are independent.
**Alternate Hypothesis:** There is a relationship between mailer type and signup rate. They are not independent.
**Acceptance Criteria:** 0.05

As a requirement of the Chi-Square Test For Independence, we aggregated this data down to a 2x2 matrix for *signup_flag* by *mailer_type* and fed this into the algorithm (using the *scipy* library) to calculate the Chi-Square Statistic, p-value, Degrees of Freedom, and expected values

<br>
<br>

### Results & Discussion <a name="overview-results"></a>

Based upon our observed values, we can give this all some context with the sign-up rate of each group.  We get:

* Mailer 1 (Low Cost): **32.8%** signup rate
* Mailer 2 (High Cost): **37.8%** signup rate

However, the Chi-Square Test gives us the following statistics:

* Chi-Square Statistic: **1.94**
* p-value: **0.16**

The Critical Value for our specified Acceptance Criteria of 0.05 is **3.84**

Based upon these statistics, we retain the null hypothesis, and conclude that there is no relationship between mailer type and signup rate.

In other words - while we saw that the higher cost Mailer 2 had a higher signup rate (37.8%) than the lower cost Mailer 1 (32.8%) it appears that this difference is not significant, at least at our Acceptance Criteria of 0.05.

Without running this Hypothesis Test, the client may have concluded that they should always look to go with higher cost mailers - and from what we've seen in this test, that may not be a great decision.  It would result in them spending more, but not *necessarily* gaining any extra revenue as a result

Our results here also do not say that there *definitely isn't a difference between the two mailers* - we are only advising that we should not make any rigid conclusions *at this point*.  

Running more A/B Tests like this, gathering more data, and then re-running this test may provide us, and the client more insight!

<br>
<br>

___

# Concept Overview  <a name="concept-overview"></a>

<br>
#### A/B Testing

An A/B Test can be described as a randomised experiment containing two groups, A & B, that receive different experiences. Within an A/B Test, we look to understand and measure the response of each group - and the information from this helps drive future business decisions.

Application of A/B testing can range from testing different online ad strategies, different email subject lines when contacting customers, or testing the effect of mailing customers a coupon, vs a control group.  Companies like Amazon are running these tests in an almost never-ending cycle, testing new website features on randomised groups of customers...all with the aim of finding what works best so they can stay ahead of their competition.  Reportedly, Netflix will even test different images for the same movie or show, to different segments of their customer base to see if certain images pull more viewers in.

<br>
#### Hypothesis Testing

A Hypothesis Test is used to assess the plausibility, or likelihood of an assumed viewpoint based on sample data - in other words, a it helps us assess whether a certain view we have about some data is likely to be true or not.

There are many different scenarios we can run Hypothesis Tests on, and they all have slightly different techniques and formulas - however they all have some shared, fundamental steps & logic that underpin how they work.

<br>
**The Null Hypothesis**

In any Hypothesis Test, we start with the Null Hypothesis. The Null Hypothesis is where we state our initial viewpoint, and in statistics, and specifically Hypothesis Testing, our initial viewpoint is always that the result is purely by chance or that there is no relationship or association between two outcomes or groups

<br>
**The Alternate Hypothesis**

The aim of the Hypothesis Test is to look for evidence to support or reject the Null Hypothesis.  If we reject the Null Hypothesis, that would mean we’d be supporting the Alternate Hypothesis.  The Alternate Hypothesis is essentially the opposite viewpoint to the Null Hypothesis - that the result is *not* by chance, or that there *is* a relationship between two outcomes or groups

<br>
**The Acceptance Criteria**

In a Hypothesis Test, before we collect any data or run any numbers - we specify an Acceptance Criteria.  This is a p-value threshold at which we’ll decide to reject or support the null hypothesis.  It is essentially a line we draw in the sand saying "if I was to run this test many many times, what proportion of those times would I want to see different results come out, in order to feel comfortable, or confident that my results are not just some unusual occurrence"

Conventionally, we set our Acceptance Criteria to 0.05 - but this does not have to be the case.  If we need to be more confident that something did not occur through chance alone, we could lower this value down to something much smaller, meaning that we only come to the conclusion that the outcome was special or rare if it’s extremely rare.

So to summarise, in a Hypothesis Test, we test the Null Hypothesis using a p-value and then decide it’s fate based on the Acceptance Criteria.

<br>
**Types Of Hypothesis Test**

There are many different types of Hypothesis Tests, each of which is appropriate for use in differing scenarios - depending on a) the type of data that you’re looking to test and b) the question that you’re asking of that data.

In the case of our task here, where we are looking to understand the difference in sign-up *rate* between two groups - we will utilise the Chi-Square Test For Independence.

<br>
#### Chi-Square Test For Independence

The Chi-Square Test For Independence is a type of Hypothesis Test that assumes observed frequencies for categorical variables will match the expected frequencies.

The *assumption* is the Null Hypothesis, which as discussed above is always the viewpoint that the two groups will be equal.  With the Chi-Square Test For Independence we look to calculate a statistic which, based on the specified Acceptance Criteria will mean we either reject or support this initial assumption.

The *observed frequencies* are the true values that we’ve seen.

The *expected frequencies* are essentially what we would *expect* to see based on all of the data.

**Note:** Another option when comparing "rates" is a test known as the *Z-Test For Proportions*.  While, we could absolutely use this test here, we have chosen the Chi-Square Test For Independence because:

* The resulting test statistic for both tests will be the same
* The Chi-Square Test can be represented using 2x2 tables of data - meaning it can be easier to explain to stakeholders
* The Chi-Square Test can extend out to more than 2 groups - meaning the business can have one consistent approach to measuring signficance

___

<br>
# Data Overview & Preparation  <a name="data-overview"></a>

In the client database, we have a *campaign_data* table which shows us which customers received each type of "Delivery Club" mailer, which customers were in the control group, and which customers joined the club as a result.

For this task, we are looking to find evidence that the Delivery Club signup rate for customers that received "Mailer 1" (low cost) was different to those who received "Mailer 2" (high cost) and thus from the *campaign_data* table we will just extract customers in those two groups, and exclude customers who were in the control group.

In the code below, we:

* Load in the Python libraries we require for importing the data and performing the chi-square test (using scipy)
* Import the required data from the *campaign_data* table
* Exclude customers in the control group, giving us a dataset with Mailer 1 & Mailer 2 customers only

<br>
```python

# install the required python libraries
import pandas as pd
from scipy.stats import chi2_contingency, chi2

# import campaign data
campaign_data = ...

# remove customers who were in the control group
campaign_data = campaign_data.loc[campaign_data["mailer_type"] != "Control"]

```
<br>
A sample of this data (the first 10 rows) can be seen below:
<br>
<br>

| **customer_id** | **campaign_name** | **mailer_type** | **signup_flag** |
|---|---|---|---|
| 74 | delivery_club | Mailer1 | 1 |
| 524 | delivery_club | Mailer1 | 1 |
| 607 | delivery_club | Mailer2 | 1 |
| 343 | delivery_club | Mailer1 | 0 |
| 322 | delivery_club | Mailer2 | 1 |
| 115 | delivery_club | Mailer2 | 0 |
| 1 | delivery_club | Mailer2 | 1 |
| 120 | delivery_club | Mailer1 | 1 |
| 52 | delivery_club | Mailer1 | 1 |
| 405 | delivery_club | Mailer1 | 0 |
| 435 | delivery_club | Mailer2 | 0 |

<br>
In the DataFrame we have:

* customer_id
* campaign name
* mailer_type (either Mailer1 or Mailer2)
* signup_flag (either 1 or 0)

___

<br>
# Applying Chi-Square Test For Independence <a name="chi-square-application"></a>

<br>
#### State Hypotheses & Acceptance Criteria For Test

The very first thing we need to do in any form of Hypothesis Test is state our Null Hypothesis, our Alternate Hypothesis, and the Acceptance Criteria (more details on these in the section above)

In the code below we code these in explcitly & clearly so we can utilise them later to explain the results.  We specify the common Acceptance Criteria value of 0.05.

```python

# specify hypotheses & acceptance criteria for test
null_hypothesis = "There is no relationship between mailer type and signup rate.  They are independent"
alternate_hypothesis = "There is a relationship between mailer type and signup rate.  They are not independent"
acceptance_criteria = 0.05

```

<br>
#### Calculate Observed Frequencies & Expected Frequencies

As mentioned in the section above, in a Chi-Square Test For Independence, the *observed frequencies* are the true values that we’ve seen, in other words the actual rates per group in the data itself.  The *expected frequencies* are what we would *expect* to see based on *all* of the data combined.

The below code:

* Summarises our dataset to a 2x2 matrix for *signup_flag* by *mailer_type*
* Based on this, calculates the:
    * Chi-Square Statistic
    * p-value
    * Degrees of Freedom
    * Expected Values
* Prints out the Chi-Square Statistic & p-value from the test
* Calculates the Critical Value based upon our Acceptance Criteria & the Degrees Of Freedom
* Prints out the Critical Value

```python

# aggregate our data to get observed values
observed_values = pd.crosstab(campaign_data["mailer_type"], campaign_data["signup_flag"]).values

# run the chi-square test
chi2_statistic, p_value, dof, expected_values = chi2_contingency(observed_values, correction = False)

# print chi-square statistic
print(chi2_statistic)
>> 1.94

# print p-value
print(p_value)
>> 0.16

# find the critical value for our test
critical_value = chi2.ppf(1 - acceptance_criteria, dof)

# print critical value
print(critical_value)
>> 3.84

```
<br>
Based upon our observed values, we can give this all some context with the sign-up rate of each group.  We get:

* Mailer 1 (Low Cost): **32.8%** signup rate
* Mailer 2 (High Cost): **37.8%** signup rate

From this, we can see that the higher cost mailer does lead to a higher signup rate.  The results from our Chi-Square Test will provide us more information about how confident we can be that this difference is robust, or if it might have occured by chance.

We have a Chi-Square Statistic of **1.94** and a p-value of **0.16**.  The critical value for our specified Acceptance Criteria of 0.05 is **3.84**

**Note** When applying the Chi-Square Test above, we use the parameter *correction = False* which means we are applying what is known as the *Yate's Correction* which is applied when your Degrees of Freedom is equal to one.  This correction helps to prevent overestimation of statistical signficance in this case.

___

<br>
# Analysing The Results <a name="chi-square-results"></a>

At this point we have everything we need to understand the results of our Chi-Square test - and just from the results above we can see that, since our resulting p-value of **0.16** is *greater* than our Acceptance Criteria of 0.05 then we will _retain_ the Null Hypothesis and conclude that there is no significant difference between the signup rates of Mailer 1 and Mailer 2.

We can make the same conclusion based upon our resulting Chi-Square statistic of **1.94** being _lower_ than our Critical Value of **3.84**

To make this script more dynamic, we can create code to automatically interpret the results and explain the outcome to us...

```python

# print the results (based upon p-value)
if p_value <= acceptance_criteria:
    print(f"As our p-value of {p_value} is lower than our acceptance_criteria of {acceptance_criteria} - we reject the null hypothesis, and conclude that: {alternate_hypothesis}")
else:
    print(f"As our p-value of {p_value} is higher than our acceptance_criteria of {acceptance_criteria} - we retain the null hypothesis, and conclude that: {null_hypothesis}")

>> As our p-value of 0.16351 is higher than our acceptance_criteria of 0.05 - we retain the null hypothesis, and conclude that: There is no relationship between mailer type and signup rate.  They are independent


# print the results (based upon p-value)
if chi2_statistic >= critical_value:
    print(f"As our chi-square statistic of {chi2_statistic} is higher than our critical value of {critical_value} - we reject the null hypothesis, and conclude that: {alternate_hypothesis}")
else:
    print(f"As our chi-square statistic of {chi2_statistic} is lower than our critical value of {critical_value} - we retain the null hypothesis, and conclude that: {null_hypothesis}")
    
>> As our chi-square statistic of 1.9414 is lower than our critical value of 3.841458820694124 - we retain the null hypothesis, and conclude that: There is no relationship between mailer type and signup rate.  They are independent

```
<br>
As we can see from the outputs of these print statements, we do indeed retain the null hypothesis.  We could not find enough evidence that the signup rates for Mailer 1 and Mailer 2 were different - and thus conclude that there was no significant difference.

___

<br>
# Discussion <a name="discussion"></a>

While we saw that the higher cost Mailer 2 had a higher signup rate (37.8%) than the lower cost Mailer 1 (32.8%) it appears that this difference is not significant, at least at our Acceptance Criteria of 0.05.

Without running this Hypothesis Test, the client may have concluded that they should always look to go with higher cost mailers - and from what we've seen in this test, that may not be a great decision.  It would result in them spending more, but not *necessarily* gaining any extra revenue as a result

Our results here also do not say that there *definitely isn't a difference between the two mailers* - we are only advising that we should not make any rigid conclusions *at this point*.  

Running more A/B Tests like this, gathering more data, and then re-running this test may provide us, and the client more insight!

{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "source": [
        "#**Analyzing Signup Rates for Mailer Types for ABC Grocery Store**"
      ],
      "metadata": {
        "id": "3it7GWntP3pp"
      }
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "id": "ONcUCpWbDVw9"
      },
      "outputs": [],
      "source": [
        "import pandas as pd\n",
        "import scipy as sp\n",
        "from scipy.stats import chi2_contingency, chi\n",
        "import seaborn as sns\n",
        "import matplotlib.pyplot as plt"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "## import the mailer data\n",
        "mailer_df = pd.read_excel('/content/grocery_database.xlsx', sheet_name = 'campaign_data' )"
      ],
      "metadata": {
        "id": "saNUw-PUDukk"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "mailer_df.head()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "GsFtZ4z0EQvR",
        "outputId": "45b994f4-3e05-418c-ce8d-ba00ee607205"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "   customer_id  campaign_name campaign_date mailer_type  signup_flag\n",
              "0           74  delivery_club    2020-07-01     Mailer1            1\n",
              "1          524  delivery_club    2020-07-01     Mailer1            1\n",
              "2          607  delivery_club    2020-07-01     Mailer2            1\n",
              "3          343  delivery_club    2020-07-01     Mailer1            0\n",
              "4          322  delivery_club    2020-07-01     Mailer2            1"
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-5383d81f-11fc-4722-92af-1d72bada159a\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>customer_id</th>\n",
              "      <th>campaign_name</th>\n",
              "      <th>campaign_date</th>\n",
              "      <th>mailer_type</th>\n",
              "      <th>signup_flag</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>74</td>\n",
              "      <td>delivery_club</td>\n",
              "      <td>2020-07-01</td>\n",
              "      <td>Mailer1</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>524</td>\n",
              "      <td>delivery_club</td>\n",
              "      <td>2020-07-01</td>\n",
              "      <td>Mailer1</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>607</td>\n",
              "      <td>delivery_club</td>\n",
              "      <td>2020-07-01</td>\n",
              "      <td>Mailer2</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>343</td>\n",
              "      <td>delivery_club</td>\n",
              "      <td>2020-07-01</td>\n",
              "      <td>Mailer1</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>322</td>\n",
              "      <td>delivery_club</td>\n",
              "      <td>2020-07-01</td>\n",
              "      <td>Mailer2</td>\n",
              "      <td>1</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-5383d81f-11fc-4722-92af-1d72bada159a')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-5383d81f-11fc-4722-92af-1d72bada159a button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-5383d81f-11fc-4722-92af-1d72bada159a');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "<div id=\"df-4791f01d-9d05-49b4-905d-d3b6ff65542c\">\n",
              "  <button class=\"colab-df-quickchart\" onclick=\"quickchart('df-4791f01d-9d05-49b4-905d-d3b6ff65542c')\"\n",
              "            title=\"Suggest charts\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "<svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "     width=\"24px\">\n",
              "    <g>\n",
              "        <path d=\"M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z\"/>\n",
              "    </g>\n",
              "</svg>\n",
              "  </button>\n",
              "\n",
              "<style>\n",
              "  .colab-df-quickchart {\n",
              "      --bg-color: #E8F0FE;\n",
              "      --fill-color: #1967D2;\n",
              "      --hover-bg-color: #E2EBFA;\n",
              "      --hover-fill-color: #174EA6;\n",
              "      --disabled-fill-color: #AAA;\n",
              "      --disabled-bg-color: #DDD;\n",
              "  }\n",
              "\n",
              "  [theme=dark] .colab-df-quickchart {\n",
              "      --bg-color: #3B4455;\n",
              "      --fill-color: #D2E3FC;\n",
              "      --hover-bg-color: #434B5C;\n",
              "      --hover-fill-color: #FFFFFF;\n",
              "      --disabled-bg-color: #3B4455;\n",
              "      --disabled-fill-color: #666;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart {\n",
              "    background-color: var(--bg-color);\n",
              "    border: none;\n",
              "    border-radius: 50%;\n",
              "    cursor: pointer;\n",
              "    display: none;\n",
              "    fill: var(--fill-color);\n",
              "    height: 32px;\n",
              "    padding: 0;\n",
              "    width: 32px;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart:hover {\n",
              "    background-color: var(--hover-bg-color);\n",
              "    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "    fill: var(--button-hover-fill-color);\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart-complete:disabled,\n",
              "  .colab-df-quickchart-complete:disabled:hover {\n",
              "    background-color: var(--disabled-bg-color);\n",
              "    fill: var(--disabled-fill-color);\n",
              "    box-shadow: none;\n",
              "  }\n",
              "\n",
              "  .colab-df-spinner {\n",
              "    border: 2px solid var(--fill-color);\n",
              "    border-color: transparent;\n",
              "    border-bottom-color: var(--fill-color);\n",
              "    animation:\n",
              "      spin 1s steps(1) infinite;\n",
              "  }\n",
              "\n",
              "  @keyframes spin {\n",
              "    0% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "      border-left-color: var(--fill-color);\n",
              "    }\n",
              "    20% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    30% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    40% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    60% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    80% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "    90% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "  }\n",
              "</style>\n",
              "\n",
              "  <script>\n",
              "    async function quickchart(key) {\n",
              "      const quickchartButtonEl =\n",
              "        document.querySelector('#' + key + ' button');\n",
              "      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.\n",
              "      quickchartButtonEl.classList.add('colab-df-spinner');\n",
              "      try {\n",
              "        const charts = await google.colab.kernel.invokeFunction(\n",
              "            'suggestCharts', [key], {});\n",
              "      } catch (error) {\n",
              "        console.error('Error during call to suggestCharts:', error);\n",
              "      }\n",
              "      quickchartButtonEl.classList.remove('colab-df-spinner');\n",
              "      quickchartButtonEl.classList.add('colab-df-quickchart-complete');\n",
              "    }\n",
              "    (() => {\n",
              "      let quickchartButtonEl =\n",
              "        document.querySelector('#df-4791f01d-9d05-49b4-905d-d3b6ff65542c button');\n",
              "      quickchartButtonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "    })();\n",
              "  </script>\n",
              "</div>\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ],
            "application/vnd.google.colaboratory.intrinsic+json": {
              "type": "dataframe",
              "variable_name": "mailer_df",
              "summary": "{\n  \"name\": \"mailer_df\",\n  \"rows\": 870,\n  \"fields\": [\n    {\n      \"column\": \"customer_id\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 251,\n        \"min\": 1,\n        \"max\": 870,\n        \"num_unique_values\": 870,\n        \"samples\": [\n          597,\n          349,\n          766\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"campaign_name\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 1,\n        \"samples\": [\n          \"delivery_club\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"campaign_date\",\n      \"properties\": {\n        \"dtype\": \"date\",\n        \"min\": \"2020-07-01 00:00:00\",\n        \"max\": \"2020-07-01 00:00:00\",\n        \"num_unique_values\": 1,\n        \"samples\": [\n          \"2020-07-01 00:00:00\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"mailer_type\",\n      \"properties\": {\n        \"dtype\": \"category\",\n        \"num_unique_values\": 3,\n        \"samples\": [\n          \"Mailer1\"\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"signup_flag\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0,\n        \"min\": 0,\n        \"max\": 1,\n        \"num_unique_values\": 2,\n        \"samples\": [\n          0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}"
            }
          },
          "metadata": {},
          "execution_count": 15
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [],
      "metadata": {
        "id": "h35vzboxP0gX"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "## excluding the control group for the analysis\n",
        "campaign_data = mailer_df.loc[mailer_df['mailer_type'] != 'Control']\n"
      ],
      "metadata": {
        "id": "rsVsikNqFCZv"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "##**Analyzing Mailer Type Effectiveness**\n",
        "\n"
      ],
      "metadata": {
        "id": "aOS5GBQyJonj"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "sns.heatmap(pd.crosstab(campaign_data['mailer_type'], campaign_data['signup_flag']),  annot=True, fmt=\".2f\", annot_kws={\"size\": 10, \"color\": \"black\"}, cmap=\"Set1\",cbar= False)\n",
        "plt.title(\"Mailer Type vs Signup Counts\", fontsize=16, fontweight='bold')\n",
        "plt.show()"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 474
        },
        "id": "A9XirM7tEpxG",
        "outputId": "037f8070-4a9f-49d0-d3ce-9177b89b23bf"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "display_data",
          "data": {
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ],
            "image/png": "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Vex9vz7//HMNGDDAfWPIxo0bm67I87aiNw9NTEyskvt4ARcbd5UGqtgbb7yhEydOaP369aa7Tj/88MMlBhdUH+vXr1fv3r3VpUsXNWnSRLGxscrIyNCmTZv0zTffmJadPn26l6oELi98cgJVbNq0aR4Df3Xp0kUPPPCAlypCeeXn52v58uUedxEuEBAQoBdffFHDhg27yJUBlyfCC3CROBwO1atXTyNGjNDjjz9err4K8J5mzZrpqaee0rfffqtdu3bp+PHjys/PV3h4uJo0aaKePXtq9OjRZR4fBcCFo88LAACwFK42AgAAlkJ4AQAAlkJ4AQAAlnJJdtidM2eOt0sAUEUGTP2bt0sAUEXqHNhXpuU48gIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACyF8AIAACylWoeXrKwsrVixwttlAACAaqRah5fdu3erR48e3i4DAABUI9U6vAAAABTl682d16hRo9R2p9N5kSoBAABW4dXwkpOTo7Fjx6pFixbFtv/666+aOnXqRa4KAABUZ14NL61bt1bdunWVmJhYbPvPP/9MeAEAACZe7fMyYMAAnT59usT2GjVq6K677rp4BQEAgGrPZhiG4e0iKtucOXO8XQKAKjJg6t+8XQKAKlLnwL4yLef1q43y8vI0evRopaSkeLsUAABgAV4PL35+fvrwww+9XQYA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AFhKlYaX4gayAwAAuBDlDi+GYSg1NVVnz54t07IAAACVqdx3lTYMQ40bN9aWLVsUHx9f6rIZGRkVLuxChH490yv7BQAAVa/cR17sdrvi4+N14sSJqqgHAACgVBXq8/Lcc8/pkUce0ebNmyu7HgAAgFKV+7SRJN11113Kzs5Wq1at5HA4TPc4kqSTJ09WSnEAAABFVSi8vPzyy5VcBgAAQNlUKLwkJiZWdh0AAABlUuFxXvbs2aMpU6botttu09GjRyVJn332mbZs2VJpxQEAABRVofCyfPlytWjRQmvWrNHixYuVmZkpSfr555/15JNPVmqBAAAAhVUovDz++ON65plntHTpUjkcDvf8nj176vvvv6+04gAAAIqqUHjZtGmTbrnlFo/5sbGxOn78+AUXBQAAUJIKhZeIiAgdOnTIY/769etVp06dCy4KAACgJBUKL7feeqsee+wxHT58WDabTS6XSytXrtTDDz+su+66q7JrBAAAcKtQeJk+fbquvvpq1a1bV5mZmWrWrJmuv/56de7cWVOmTKnsGgEAANxsxgXc+jk1NVWbN29WZmamrrnmmvPeqPFiSRrR1NslAKgi13+X6e0SAFSROgf2lWm5Cg1SV6BevXqqV6/ehWwCAACgXMocXiZNmlTmjb700ksVKgYAAOB8yhxe1q9fX6blbDZbhYsBAAA4nzKHl2XLllVlHQAAAGVS4XsbAQAAeEOZj7wMGTJE8+bNU1hYmIYMGVLqsosXL77gwgAAAIpT5vASHh7u7s8SHh5eZQUBAACU5oLGeamuGOcFuHQxzgtw6SrrOC/0eQEAAJZS4UHqPvjgAy1atEipqanKzc01ta1bt+6CCwMAAChOhY68vPLKK7r77rtVs2ZNrV+/Xh06dFBUVJT27t2rfv36VXaNAAAAbhUKL3//+981Z84cvfrqq3I4HHr00Ue1dOlSTZgwQWlpaZVdIwAAgFuFwktqaqo6d+4sSQoMDFRGRoYkaeTIkUpKSqq86gAAAIqoUHipVauWTp48KenczRm///57SVJKSoouwYuXAABANVKh8NKzZ0998sknkqS7775bDz30kHr37q0RI0bolltuqdQCAQAACqvQOC8ul0sul0u+vucuVlq4cKFWrlyp+Ph43X///fLz86v0QsuDcV6ASxfjvACXrrKO81LhQerOnj2rjRs36ujRo3K5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u+++W3feeadq1KjhzZIAAEA159VLpV977TUdOnRIjz76qP7zn/+obt26Gj58uL744guOxAAAgGJ5fZwXf39/3XbbbVq6dKm2bt2qhIQEjRs3Tg0aNFBmJh3zAACAmdfDS2F2u919byPuZwQAAIrj9fCSk5OjpKQk9e7dW1dddZU2bdqk2bNnKzU1VSEhId4uDwAAVDNe7bA7btw4LViwQHXr1tXo0aOVlJSk6Ohob5YEAACqOa8OUme321WvXj1dc801pQ5Gt3jx4nJtl0HqgEsXg9QBly5LDFJ31113VWgEXQAAcPny+iB1AAAA5eH1DrsAAADlQXgBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWQngBAACWYjMMw/B2EUBF5eTk6Nlnn9XkyZPl7+/v7XIAVCLe3ygJ4QWWlp6ervDwcKWlpSksLMzb5QCoRLy/URJOGwEAAEshvAAAAEshvAAAAEshvMDS/P399eSTT9KZD7gE8f5GSeiwCwAALIUjLwAAwFIILwAAwFIILwAAwFIILwAAwFIIL7C01157TQ0aNFBAQICuvfZa/fDDD94uCUAlWLFihQYNGqTatWvLZrNpyZIl3i4J1QjhBZa1cOFCTZo0SU8++aTWrVunVq1aqW/fvjp69Ki3SwNwgbKystSqVSu99tpr3i4F1RCXSsOyrr32WrVv316zZ8+WJLlcLtWtW1fjx4/X448/7uXqAFQWm82mjz76SDfffLO3S0E1wZEXWFJubq5++ukn9erVyz3PbrerV69eWr16tRcrAwBUNcILLOn48eNyOp2qWbOmaX7NmjV1+PBhL1UFALgYCC8AAMBSCC+wpOjoaPn4+OjIkSOm+UeOHFGtWrW8VBUA4GIgvMCSHA6H2rZtq6+//to9z+Vy6euvv1anTp28WBkAoKr5ersAoKImTZqkxMREtWvXTh06dNDLL7+srKws3X333d4uDcAFyszM1O7du93TKSkp2rBhg2rUqKF69ep5sTJUB1wqDUubPXu2XnjhBR0+fFitW7fWK6+8omuvvdbbZQG4QMnJyerRo4fH/MTERM2bN+/iF4RqhfACAAAshT4vAADAUggvAADAUggvAADAUggvAADAUggvAADAUggvAADAUggvAADAUggvAMpl1KhRuvnmm71dRoXMmTNHdevWld1u18svv6ynnnpKrVu39nZZAMqJQeoAlEtaWpoMw1BERIS3SymX9PR0RUdH66WXXtLQoUMVHh6u559/XkuWLNGGDRu8XR6AcuDeRgDKJTw83NslVEhqaqry8vI0YMAAXXHFFd4uB8AF4LQRgGJ98MEHatGihQIDAxUVFaVevXopKyvL47RRRkaG7rjjDgUHB+uKK67QzJkz1b17d/3pT39yL9OgQQNNnz5do0ePVmhoqOrVq6c5c+a425OTk2Wz2XT69Gn3vA0bNshms+mXX36RJM2bN08RERFasmSJ4uPjFRAQoL59+2rfvn3n/V3mzZunFi1aSJIaNmxo2m5ha9euVe/evRUdHa3w8HB169ZN69atMy2zfft2de3aVQEBAWrWrJm++uor2Ww2LVmy5Lx1AKgchBcAHg4dOqTbbrtNo0eP1rZt25ScnKwhQ4aouLPMkyZN0sqVK/XJJ59o6dKl+vbbbz2+8CVpxowZateundavX69x48Zp7Nix2rFjR7nqys7O1rRp0/T2229r5cqVOn36tG699dbzrjdixAh99dVXkqQffvhBhw4dUt26dT2Wy8jIUGJior777jt9//33io+PV//+/ZWRkSFJcjqduvnmmxUUFKQ1a9Zozpw5+stf/lKu3wHAheO0EQAPhw4dUn5+voYMGaL69etLkvvIRWEZGRmaP3++/v3vf+uGG26QJL311luqXbu2x7L9+/fXuHHjJEmPPfaYZs6cqWXLlqlJkyZlrisvL0+zZ8923zl8/vz5atq0qX744Qd16NChxPUKjh5JUkxMjGrVqlXscj179jRNz5kzRxEREVq+fLkGDhyopUuXas+ePUpOTnZvY9q0aerdu3eZfwcAF44jLwA8tGrVSjfccINatGihYcOG6Y033tCpU6c8ltu7d6/y8vJMwSE8PLzYQNKyZUv3zzabTbVq1dLRo0fLVZevr6/at2/vnr766qsVERGhbdu2lWs7JTly5IjuvfdexcfHKzw8XGFhYcrMzFRqaqokaceOHapbt64p/JQWmgBUDcILAA8+Pj5aunSpPvvsMzVr1kyvvvqqmjRpopSUlApv08/PzzRts9nkcrkkSXb7uY+iwqel8vLyKryvikpMTNSGDRs0a9YsrVq1Shs2bFBUVJRyc3Mvei0ASkZ4AVAsm82mLl26aOrUqVq/fr0cDoc++ugj0zINGzaUn5+f1q5d656XlpamnTt3lmtfMTExks6dripQ3OXL+fn5+vHHH93TO3bs0OnTp9W0adNy7a8kK1eu1IQJE9S/f38lJCTI399fx48fd7c3adJE+/bt05EjR9zzCv/uAC4OwgsAD2vWrNH06dP1448/KjU1VYsXL9axY8c8QkJoaKgSExP1yCOPaNmyZdqyZYvGjBkju90um81W5v01btxYdevW1VNPPaVdu3bp008/1YwZMzyW8/Pz0/jx47VmzRr99NNPGjVqlDp27Fhpp27i4+P1zjvvaNu2bVqzZo3uuOMOBQYGutt79+6tRo0aKTExURs3btTKlSs1ZcoUSSrX7wvgwhBeAHgICwvTihUr1L9/f1111VWaMmWKZsyYoX79+nks+9JLL6lTp04aOHCgevXqpS5duqhp06YKCAgo8/78/PyUlJSk7du3q2XLlvp//+//6ZlnnvFYLigoSI899phuv/12denSRSEhIVq4cOEF/a6F/etf/9KpU6fUpk0bjRw5UhMmTFBsbKy73cfHR0uWLFFmZqbat2+ve+65x321UXl+XwAXhhF2AVSqrKws1alTRzNmzNCYMWMqbbvz5s3Tn/70J9NYMNXBypUr1bVrV+3evVuNGjXydjnAZYFLpQFckPXr12v79u3q0KGD0tLS9PTTT0uSbrrpJi9XVjU++ugjhYSEKD4+Xrt379bEiRPVpUsXggtwEXHaCMAFe/HFF9WqVSv3KLzffvutoqOjL2oNCQkJCgkJKfbx3nvvVdp+MjIy9MADD+jqq6/WqFGj1L59e3388ceVtn0A58dpIwCXhF9//bXEy6tr1qyp0NDQi1wRgKpCeAEAAJbCaSMAAGAphBcAAGAphBcAAGAphBcAAGAphBcAAGAphBcAAGAphBcAAGAphBcAAGAp/x8yTJauzrs3lgAAAABJRU5ErkJggg==\n"
          },
          "metadata": {}
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "### At the outset there seems to be a  difference in signup rates amongst the two mailers\n",
        "  ### - For **mailer 1** the signup rate is 123/252 = **0.488**\n",
        "  ### - For **mailer 2** the signup rate is 127/209 = **0.608**"
      ],
      "metadata": {
        "id": "qoPHUpTwJP6C"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "## Checking for statistical difference with Chi_sq test"
      ],
      "metadata": {
        "id": "GO2XDaF2LFn3"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "null_hypothesis = \"There is no relationship between mailer type and signup rate. They are independent\"\n",
        "alternate_hypothesis = \"There is a relationship between mailer type and signup rate. They are not independent\"\n",
        "acceptance_criteria = 0.05\n",
        "\n",
        "chi2_statistic, p_value, dof, expected_values = chi2_contingency(pd.crosstab(campaign_data['mailer_type'], campaign_data['signup_flag']))\n",
        "\n",
        "print(\"Chi-squared Statistic:\", chi2_statistic)\n",
        "print(\"P-value:\", p_value)\n"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "Jy0YAOy-IxRX",
        "outputId": "1432f08a-3370-449f-9ae5-5f3ddcde12f6"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Chi-squared Statistic: 1.728424144871394\n",
            "P-value: 0.1886122739808747\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "print(f\"\"\"Since p value of {p_value} is more than the threshold value of confidence level of {acceptance_criteria},\n",
        "we cannot reject the hypothesis that:\n",
        "{null_hypothesis}\"\"\")"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "1wa-2ZHXNcfa",
        "outputId": "03a605e7-89e3-4464-cd33-0bdc4a37e043"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Since p value of 0.1886122739808747 is more than the threshold value of confidence level of 0.05, \n",
            "we cannot reject the hypothesis that:\n",
            "There is no relationship between mailer type and signup rate. They are independent\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "#**Conclusion**"
      ],
      "metadata": {
        "id": "-U0j4BYQO28U"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "## We cannot statistically say that the signup rate for mailer_2 is higher than mailer_1"
      ],
      "metadata": {
        "id": "UXqtzjT1PhZZ"
      }
    }
  ]
}
