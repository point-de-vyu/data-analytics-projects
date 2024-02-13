This is a project that I completed as the capstone project for [Google Advanced Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-advanced-data-analytics)

## Project overview

This is a project for determining factors that drive employee departure and building a model that will predict if an employee will quit the company.

### Business context

Currently, there is a high rate of turnover among Salifort employees. Salifort’s senior leadership team is concerned about how many employees are leaving the company, since the high turnover rate is costly in the financial sense. If Salifort could predict whether an employee will leave the company, and discover the reasons behind their departure, they could better understand the problem and develop a solution.

## Approach

The aim is to build the most effective model to predict employee departure, explore factors driving employee turnover, and share recommendations for next steps with the leadership team. 

<details>
<summary>Data</summary>
The dataset contains 14,999 rows – each row is a different employee’s self-reported information

Variable  |Description |
-----|-----|
satisfaction_level|Employee-reported job satisfaction level [0&ndash;1]|
last_evaluation|Score of employee's last performance review [0&ndash;1]|
number_project|Number of projects employee contributes to|
average_monthly_hours|Average number of hours employee worked per month|
time_spend_company|How long the employee has been with the company (years)
Work_accident|Whether or not the employee experienced an accident while at work
left|Whether or not the employee left the company
promotion_last_5years|Whether or not the employee was promoted in the last 5 years
Department|The employee's department
salary|The employee's salary range (low / medium / high)
</details>


### Steps
#### EDA
- Data Cleaning: Check NaNs, duplicates, outliers, renaming
- Data exploration: pairplots, histograms, boxplots, correlation matrix

#### Model building
Compared decision tree and random forest

#### Model evaluation
Metrics on test set, winner on validation set (cf `model_res_comparison.csv`)

TODO: precision matrix

TODO feature importance

#### Formulating recommendations

Since the most important feature is the satisfaction level, it would be beneficial to look into the problems indicated in the responses. Maybe this feature can be divided into multiple more precise ones, or we can build a model without it (since it does not indicate a root cause for (dis)satisfaction

Also, it it very important to address the issue of overworking. Firstly, consider taking the load off the employees who work more that 250 or even 200 hours, especially if they have more than 5 projects.

Consider implementing changes to the company policy:

fix the maximum number of projects that an employee can work on (4-5)
do not require employees to take longer hours or introduce a fair overtime policy
change the company's promotion policy so that a promotion is more attainable
Initiate an open discussion about these changes with the employees, explain the changes to be implemented and take into account to their opinions


### Results

precision                     0.973262
recall                        0.914573
F1                            0.943005
accuracy                      0.981651
AUC                           0.954786