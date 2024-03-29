Part of this research was completed during [Google Advanced Data Analytics Professional Certificate](https://coursera.org/share/9da4e8e4a88ae4283ab6baea83d094be) course

## Project overview

The objectives of this project are:
* building a model that can predict user churn
* answer questions such as _Who are the users most likely to churn?_ _Why do users churn?_ _When do users churn?_ 

### Business context

> Waze’s free navigation app makes it easier for drivers around the world to get to where they want to go. Waze’s community of map editors, beta testers, translators, partners, and users helps make each drive better and safer. Waze partners with cities, transportation authorities, broadcasters, businesses, and first responders to help as many people as possible travel more efficiently and safely. 

<details>
<summary>Data</summary>

The [dataset](waze_dataset.csv) contains 14,999 rows – each row represents one unique user

Variable  |Description |
-----|-----|
ID |A sequential numbered index
label | Binary target variable (“retained” vs “churned”) for if a user has churned anytime during the course of the month 
sessions | The number of occurrence of a user opening the app during the month
drives | An occurrence of driving at least 1 km during the month
device | The type of device a user starts a session with
total_sessions | A model estimate of the total number of sessions since a user has onboarded
n_days_after_onboarding | The number of days since a user signed up for the app
total_navigations_fav1 | Total navigations since onboarding to the user’s favorite place 1
total_navigations_fav2 | Total navigations since onboarding to the user’s favorite place 2
driven_km_drives | Total kilometers driven during the month
duration_minutes_drives | Total duration driven in minutes during the month
activity_days | Number of days the user opens the app during the month 
driving_days | Number of days the user drives (at least 1 km) during the month

</details>

## Steps

### 1. Exploratory data analysis

At this stage I explored tha data and its descriptive stats; and also checked for data errors, NaNs, outliers

#### 1a Feature engineering
I engineered some features that might contribute to finding answers.

* km_per_driving_day
* km_per_drive
* drives_per_driving_day
* sessions_per_activity_day

While exploring the engineered features I found quite a lot of data errors and entries that raised suspicion.

<details>
<summary>Erroneous entries</summary>
Some examples include (the list is not exhaustive):

* 106 rows where `drives` is zero, but `driven_km_drives` value is more than zero
* 1017 rows where `driving_days` = 0 but `drives` value is more than zero
* 2458 rows where `activity_days` value is bigger than `sessions` (when in fact there can be multiple sessions in a day and not vice verca)
</details>


<details>
<summary>Confusing / suspicious entry examples</summary>
Some examples include (the list is not exhaustive):

* an average value of 14867 km per driving day for a user
* an average value of 5862 km per drive
* entries with 100+ and even 200+  drives per day on average

</details>

I removed such entries based probability distributions and my domain knowledge + intuition.

#### 1b Survival analysis

Using `KaplanMeierFitter`, I built several survival curves to check if some features could yield any insights. Turned out, for example, that Android users seem more likely to churn after 200 drives than iPhone users.

<details>
<summary>Plot</summary>

![CM](illustrations/os_surv_curve_drives.png)
</details>

### 2. Hypothesis testing
I performed a t-test to determine if there is a statistically significant difference in the number of drives between iPhone and Android users.
The null and alternative hypotheses were as follows:
* H0: mean drives are the same for iPhone and Android users
* Ha: mean drives are different depending on the OS

With the P-value of 0.03, I rejected the null hypothesis. This means that there is statistical significance in the difference of the mean rides between Android and iPhone users.

### 3. Logistic regression to predict churn
Using sklearn `LogisticRegression`, I built a logistic regression model to try and predict user churn.

Checked model assumptions:
* Little to no multicollinearity among X predictors
* Linear relationship between x and the logit of y

Here are the results:

<details>
<summary>Classification report</summary>

label | precision | recall | F1   | accuracy
-----|-----------|--------|------|-----|
retained | 0.83      | 0.99   | 0.90 |
churned | 0.55      | 0.06   | 0.11 |
| |           |        |      | 0.83

</details>

<details>
<summary>Confusion matrix</summary>

![CM](illustrations/logreg_cm.png)

</details>

<details>
<summary>Feature importance</summary>

![CM](illustrations/logreg_feature_importances.png)

</details>

This model has mediocre precision and very low recall, which means that it makes a lot of false negative predictions and fails to capture users who will churn.

### 4. Tree-based models (Random Forest, XGBoost) to predict churn

Using grid search, I trained two tree-based models: `RandomForestClassifier` and `XGBClassifier` and compared their performance.

<details>
<summary>Metric comparison on the train and validation set:</summary>


model | precision | recall | F1 | accuracy
-----|----------|-----|-----|-----|
Random Forest: train | 0.482791 |	0.097997 | 0.162712	 | 0.823611
XGB: train| 0.357803 | 0.132127 | 0.192709 | 0.806614
Random Forest: validation | 0.486111 | 0.091864 | 0.154525 | 0.824070
XGB: validation | 0.333333 | 0.112861  | 0.168627  |0.805237

</details>


XGBClassifier's metrics on the test set:

label | precision | recall | F1   | accuracy 
-----|-----------|--------|------|-----|
retained | 0.84      | 0.96   | 0.90 | 
churned | 0.40      | 0.11   | 0.19 |
| |           |        |      | 0.81

Again, the metrics show that this model is bad at predicting user churn.

## Conclusions so far

The results are unsatisfactory, none of the models is good at predicting users who are likely to churn. 

This might actually be a problem with the data itself, since at the EDA stage I've established that there are many errors. Additionally, most features are extremely right skewed. The data needs further investigation and modeling. 