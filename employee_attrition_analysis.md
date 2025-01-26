HR data analysis project
================
quanggiang169
2024-10-28

# Employee Retention Analytics: Uncovering the Factors Behind Attrition

<!-- Objective section to explain the aim of the project -->

<div style="margin-top: -10px; font-weight: normal; font-size: 20px;">

Objective: Analyze HR data to understand the factors influencing
employee attrition.

</div>

<!-- Horizontal line to separate sections -->
<hr>

## 1. Understanding the context and the problem

### 1.1. Understanding Attrition in the Workplace

**Attrition** refers to the gradual reduction of a company’s workforce
due to voluntary employee departures, such as resignations, retirements,
or other forms of separation where roles are not immediately filled.
Unlike turnover, which includes both voluntary and involuntary exits,
attrition typically highlights employees’ personal decisions to leave,
often for new opportunities or lifestyle changes.

Attrition is a natural part of an organization’s lifecycle, but high
attrition rates can signal deeper issues and create significant
challenges. Managing attrition is crucial for several reasons:

- **Organizational Continuity**: Each departure affects the flow of
  knowledge, skills, and experience within the company. Frequent
  attrition disrupts team dynamics, lowers continuity, and may delay
  projects as new hires get up to speed.
- **Financial Impact**: High attrition incurs substantial costs,
  including expenses for recruiting, onboarding, and training new
  employees, which can be as high as 30% to 50% of a departing
  employee’s annual salary.
- **Employee Morale and Productivity**: High attrition often places
  extra pressure on remaining staff, who must compensate for the loss.
  This can lead to employee burnout, reduced morale, and, ultimately, a
  drop in productivity, affecting team performance and the company’s
  overall efficiency.

In recent years, attrition rates have reached new highs. For example,
according to the U.S. Bureau of Labor Statistics, voluntary departures
accounted for 67% of separations in 2021—a significant increase driven
by evolving employee priorities, heightened by the pandemic. This trend,
known as the “Great Attrition,” underscores the urgent need for
companies to understand why employees leave and to implement strategies
that foster retention, engagement, and a positive work environment.

**References**

- [The Great Attrition: Facing the labor shortage conundrum - McKinsey &
  Company](https://www.mckinsey.com/capabilities/people-and-organizational-performance/our-insights/the-organization-blog/the-great-attrition-facing-the-labor-shortage-conundrum)

- [Employee Attrition: Meaning, Impact & Attrition Rate Calculation-
  AIHR](https://www.aihr.com/blog/employee-attrition/)

- [Attrition: Definition, Types, Causes & Mitigation Tips-
  SHRM](https://www.shrm.org/in/topics-tools/news/employee-relations/attrition-definition-types-causes-mitigation-tips)

### 1.2. Project Objective

The primary aim of this project is to analyze employee attrition data to
uncover the key factors influencing employees’ decisions to leave.
Through Exploratory Data Analysis (EDA), I hope to identify patterns and
insights that reveal which characteristics, such as age, job position,
experience, salary, or company culture, most strongly impact employee
attrition.

Key questions guiding this analysis include:

- Are certain groups of employees more likely to leave than others?
- What characteristics are commonly found among employees who decide to
  leave?
- Which factors can be managed or improved to help reduce attrition
  rates?

By answering these questions, the EDA aims to provide actionable
insights that can inform strategies to enhance employee retention and
foster a more stable workforce.

## 2. Load libraries

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(moments)
library(janitor)
```

    ## 
    ## Attaching package: 'janitor'
    ## 
    ## The following objects are masked from 'package:stats':
    ## 
    ##     chisq.test, fisher.test

``` r
library(knitr)
library(gridExtra)
```

    ## 
    ## Attaching package: 'gridExtra'
    ## 
    ## The following object is masked from 'package:dplyr':
    ## 
    ##     combine

``` r
library(ggcorrplot)
library(reshape2)
```

    ## 
    ## Attaching package: 'reshape2'
    ## 
    ## The following object is masked from 'package:tidyr':
    ## 
    ##     smiths

## 3. Checking the data set

### 3.1. Data Dictionary

The IBM HR Analytics Employee Attrition & Performance dataset is a
fictional dataset created by IBM data scientists. Since not all features
have descriptions, I have made interpretations of what they represent.

You can find the dataset
[here](https://www.kaggle.com/pavansubhasht/ibm-hr-analytics-attrition-dataset).

| Feature Name | Data Type | Description |
|----|----|----|
| Age | integer | Employee’s age in years |
| Attrition | factor | If the employee stayed or left the company (Yes ; No) |
| BusinessTravel | factor | How often the employee has business travel (Travel_Rarely ; Travel_Frequently ; Non-Travel) |
| DailyRate | integer | Employee’s daily rate in USD. |
| Department | factor | Which department the employee belongs to (Sales ; Research & Development ; Human Resources) |
| DistanceFromHome | integer | How far the employee lives from work in kilometers. |
| Education | integer | Employee’s education level (1 = Below College ; 2 = College ; 3 = Bachelor ; 4 = Master ; 5 = Doctor) |
| EducationField | factor | Employee’s education field (Life Sciences ; Medical ; Marketing ; Technical Degree, Human Resources ; Other) |
| EmployeeCount | integer | How many employees the current record represents. |
| EmployeeNumber | integer | Employee’s unique identification number |
| EnvironmentSatisfaction | integer | How satisfied the employee is with the company’s environment (1 = Low ; 2 = Medium ; 3 = High ; 4 = Very High) |
| Gender | factor | Employee’s gender (Female ; Male) |
| HourlyRate | integer | Employee’s hourly rate in USD |
| JobInvolvement | integer | How involved the employee feels with his/her job (1 = Low ; 2 = Medium ; 3 = High ; 4 = Very High) |
| JobLevel | integer | Employee’s job level (1 = Junior ; 2 = Mid ; 3 = Senior ; 4 = Manager ; 5 = Director) |
| JobRole | factor | Employee’s job role (Sales Executive ; Research Scientist ; Laboratory Technician ; Manufacturing Director ; Healthcare Representative ; Manager ; Sales Representative ; Research Director ; Human Resources) |
| JobSatisfaction | integer | How satisfied the employee feels with his/her job (1 = Low ; 2 = Medium ; 3 = High ; 4 = Very High) |
| MaritalStatus | factor | Employee’s marital status (Single ; Married ; Divorced) |
| MonthlyIncome | integer | Employee’s monthly income in USD |
| MonthlyRate | integer | Employee’s monthly rate in USD |
| NumCompaniesWorked | integer | Number of companies that the employee has already worked. |
| Over18 | factor | If the employee is over 18 years old (Yes) |
| OverTime | factor | If the employee makes overtime (Yes ; No) |
| PercentSalaryHike | integer | The percentage of the amount a salary is increased |
| PerformanceRating | integer | Employee’s performance rating (1 = Low ; 2 = Good ; 3 = Excellent ; 4 = Outstanding) |
| RelationshipSatisfaction | integer | How satisfied the employee feels with the relationship with his/her manager (1 = Low ; 2 = Medium ; 3 = High ; 4 = Very High) |
| StandardHours | integer | Employee’s standard hours of work per day |
| StockOptionLevel | integer | Employee’s stock option level (refer to: What You Should Know About Option Trading Levels) |
| TotalWorkingYears | integer | Total years that the employee has professionally worked |
| TrainingTimesLastYear | integer | Total times that the employee had a training session the last year |
| WorkLifeBalance | integer | How the employee feels about his/her work-life balance (1 = Bad ; 2 = Good ; 3 = Better ; 4 = Best) |
| YearsAtCompany | integer | Total years that the employee has worked at the company |
| YearsInCurrentRole | integer | Total years that the employee has worked in his/her current job role |
| YearsSinceLastPromotion | integer | Total years since the employee had his/her last promotion at the company |
| YearsWithCurrManager | integer | Total years that the employee has worked under his/her current manager |

### 3.2. Loading data set

``` r
df_hratt <- read_csv("dataset/WA_Fn_UseC_HR_Employee_Attrition.csv", col_types = cols())
```

### 3.3. Renaming columns

``` r
df_hratt_cln <- clean_names(df_hratt)
```

### 3.4. Checking data dimensions

    ## Number of rows: 1470

    ## Number of cols: 35

### 3.5. Checking data types

    ## spc_tbl_ [1,470 × 35] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ age                       : num [1:1470] 41 49 37 33 27 32 59 30 38 36 ...
    ##  $ attrition                 : chr [1:1470] "Yes" "No" "Yes" "No" ...
    ##  $ business_travel           : chr [1:1470] "Travel_Rarely" "Travel_Frequently" "Travel_Rarely" "Travel_Frequently" ...
    ##  $ daily_rate                : num [1:1470] 1102 279 1373 1392 591 ...
    ##  $ department                : chr [1:1470] "Sales" "Research & Development" "Research & Development" "Research & Development" ...
    ##  $ distance_from_home        : num [1:1470] 1 8 2 3 2 2 3 24 23 27 ...
    ##  $ education                 : num [1:1470] 2 1 2 4 1 2 3 1 3 3 ...
    ##  $ education_field           : chr [1:1470] "Life Sciences" "Life Sciences" "Other" "Life Sciences" ...
    ##  $ employee_count            : num [1:1470] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ employee_number           : num [1:1470] 1 2 4 5 7 8 10 11 12 13 ...
    ##  $ environment_satisfaction  : num [1:1470] 2 3 4 4 1 4 3 4 4 3 ...
    ##  $ gender                    : chr [1:1470] "Female" "Male" "Male" "Female" ...
    ##  $ hourly_rate               : num [1:1470] 94 61 92 56 40 79 81 67 44 94 ...
    ##  $ job_involvement           : num [1:1470] 3 2 2 3 3 3 4 3 2 3 ...
    ##  $ job_level                 : num [1:1470] 2 2 1 1 1 1 1 1 3 2 ...
    ##  $ job_role                  : chr [1:1470] "Sales Executive" "Research Scientist" "Laboratory Technician" "Research Scientist" ...
    ##  $ job_satisfaction          : num [1:1470] 4 2 3 3 2 4 1 3 3 3 ...
    ##  $ marital_status            : chr [1:1470] "Single" "Married" "Single" "Married" ...
    ##  $ monthly_income            : num [1:1470] 5993 5130 2090 2909 3468 ...
    ##  $ monthly_rate              : num [1:1470] 19479 24907 2396 23159 16632 ...
    ##  $ num_companies_worked      : num [1:1470] 8 1 6 1 9 0 4 1 0 6 ...
    ##  $ over18                    : chr [1:1470] "Y" "Y" "Y" "Y" ...
    ##  $ over_time                 : chr [1:1470] "Yes" "No" "Yes" "Yes" ...
    ##  $ percent_salary_hike       : num [1:1470] 11 23 15 11 12 13 20 22 21 13 ...
    ##  $ performance_rating        : num [1:1470] 3 4 3 3 3 3 4 4 4 3 ...
    ##  $ relationship_satisfaction : num [1:1470] 1 4 2 3 4 3 1 2 2 2 ...
    ##  $ standard_hours            : num [1:1470] 80 80 80 80 80 80 80 80 80 80 ...
    ##  $ stock_option_level        : num [1:1470] 0 1 0 0 1 0 3 1 0 2 ...
    ##  $ total_working_years       : num [1:1470] 8 10 7 8 6 8 12 1 10 17 ...
    ##  $ training_times_last_year  : num [1:1470] 0 3 3 3 3 2 3 2 2 3 ...
    ##  $ work_life_balance         : num [1:1470] 1 3 3 3 3 2 2 3 3 2 ...
    ##  $ years_at_company          : num [1:1470] 6 10 0 8 2 7 1 1 9 7 ...
    ##  $ years_in_current_role     : num [1:1470] 4 7 0 7 2 7 0 0 7 7 ...
    ##  $ years_since_last_promotion: num [1:1470] 0 1 0 3 2 3 0 0 1 7 ...
    ##  $ years_with_curr_manager   : num [1:1470] 5 7 0 0 2 6 0 0 8 7 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Age = col_double(),
    ##   ..   Attrition = col_character(),
    ##   ..   BusinessTravel = col_character(),
    ##   ..   DailyRate = col_double(),
    ##   ..   Department = col_character(),
    ##   ..   DistanceFromHome = col_double(),
    ##   ..   Education = col_double(),
    ##   ..   EducationField = col_character(),
    ##   ..   EmployeeCount = col_double(),
    ##   ..   EmployeeNumber = col_double(),
    ##   ..   EnvironmentSatisfaction = col_double(),
    ##   ..   Gender = col_character(),
    ##   ..   HourlyRate = col_double(),
    ##   ..   JobInvolvement = col_double(),
    ##   ..   JobLevel = col_double(),
    ##   ..   JobRole = col_character(),
    ##   ..   JobSatisfaction = col_double(),
    ##   ..   MaritalStatus = col_character(),
    ##   ..   MonthlyIncome = col_double(),
    ##   ..   MonthlyRate = col_double(),
    ##   ..   NumCompaniesWorked = col_double(),
    ##   ..   Over18 = col_character(),
    ##   ..   OverTime = col_character(),
    ##   ..   PercentSalaryHike = col_double(),
    ##   ..   PerformanceRating = col_double(),
    ##   ..   RelationshipSatisfaction = col_double(),
    ##   ..   StandardHours = col_double(),
    ##   ..   StockOptionLevel = col_double(),
    ##   ..   TotalWorkingYears = col_double(),
    ##   ..   TrainingTimesLastYear = col_double(),
    ##   ..   WorkLifeBalance = col_double(),
    ##   ..   YearsAtCompany = col_double(),
    ##   ..   YearsInCurrentRole = col_double(),
    ##   ..   YearsSinceLastPromotion = col_double(),
    ##   ..   YearsWithCurrManager = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

    ## 
    ## character   numeric 
    ##         9        26

### 3.6. Checking missing data

    ##                        age                  attrition 
    ##                          0                          0 
    ##            business_travel                 daily_rate 
    ##                          0                          0 
    ##                 department         distance_from_home 
    ##                          0                          0 
    ##                  education            education_field 
    ##                          0                          0 
    ##             employee_count            employee_number 
    ##                          0                          0 
    ##   environment_satisfaction                     gender 
    ##                          0                          0 
    ##                hourly_rate            job_involvement 
    ##                          0                          0 
    ##                  job_level                   job_role 
    ##                          0                          0 
    ##           job_satisfaction             marital_status 
    ##                          0                          0 
    ##             monthly_income               monthly_rate 
    ##                          0                          0 
    ##       num_companies_worked                     over18 
    ##                          0                          0 
    ##                  over_time        percent_salary_hike 
    ##                          0                          0 
    ##         performance_rating  relationship_satisfaction 
    ##                          0                          0 
    ##             standard_hours         stock_option_level 
    ##                          0                          0 
    ##        total_working_years   training_times_last_year 
    ##                          0                          0 
    ##          work_life_balance           years_at_company 
    ##                          0                          0 
    ##      years_in_current_role years_since_last_promotion 
    ##                          0                          0 
    ##    years_with_curr_manager 
    ##                          0

Note: Because the data set is fictional, it is free of any NaN values.

### 3.7. Descriptive statistics

#### 3.7.1. Separate Numerical and Categorical Variables

``` r
# Separate numerical and categorical attributes
num_attributes <- df_hratt_cln %>% select(where(is.numeric))
cat_attributes <- df_hratt_cln %>% select(where(negate(is.numeric)))
```

#### 3.7.2. Create a Summary Data Frame

| attributes | min | max | range | mean | median | std | skewness | kurtosis |
|:---|---:|---:|---:|---:|---:|---:|---:|---:|
| age | 18 | 60 | 42 | 3.692381e+01 | 36.0 | 9.1353735 | 0.4128645 | 2.593149 |
| daily_rate | 102 | 1499 | 1397 | 8.024857e+02 | 802.0 | 403.5090999 | -0.0035150 | 1.796189 |
| distance_from_home | 1 | 29 | 28 | 9.192517e+00 | 7.0 | 8.1068644 | 0.9571400 | 2.771852 |
| education | 1 | 5 | 4 | 2.912925e+00 | 3.0 | 1.0241649 | -0.2893854 | 2.438706 |
| employee_count | 1 | 1 | 0 | 1.000000e+00 | 1.0 | 0.0000000 | NaN | NaN |
| employee_number | 1 | 2068 | 2067 | 1.024865e+03 | 1020.5 | 602.0243348 | 0.0165571 | 1.776899 |
| environment_satisfaction | 1 | 4 | 3 | 2.721769e+00 | 3.0 | 1.0930822 | -0.3213261 | 1.797487 |
| hourly_rate | 30 | 100 | 70 | 6.589116e+01 | 66.0 | 20.3294276 | -0.0322780 | 1.803588 |
| job_involvement | 1 | 4 | 3 | 2.729932e+00 | 3.0 | 0.7115611 | -0.4979106 | 3.265999 |
| job_level | 1 | 5 | 4 | 2.063946e+00 | 2.0 | 1.1069399 | 1.0243547 | 3.393717 |
| job_satisfaction | 1 | 4 | 3 | 2.728571e+00 | 3.0 | 1.1028461 | -0.3293355 | 1.777882 |
| monthly_income | 1009 | 19999 | 18990 | 6.502931e+03 | 4919.0 | 4707.9567831 | 1.3684185 | 3.997738 |
| monthly_rate | 2094 | 26999 | 24905 | 1.431310e+04 | 14235.5 | 7117.7860441 | 0.0185588 | 1.785094 |
| num_companies_worked | 0 | 9 | 9 | 2.693197e+00 | 2.0 | 2.4980090 | 1.0254234 | 3.006100 |
| percent_salary_hike | 11 | 25 | 14 | 1.520952e+01 | 14.0 | 3.6599377 | 0.8202899 | 2.696344 |
| performance_rating | 3 | 4 | 1 | 3.153742e+00 | 3.0 | 0.3608235 | 1.9199210 | 4.686097 |
| relationship_satisfaction | 1 | 4 | 3 | 2.712245e+00 | 3.0 | 1.0812089 | -0.3025185 | 1.815133 |
| standard_hours | 80 | 80 | 0 | 8.000000e+01 | 80.0 | 0.0000000 | NaN | NaN |
| stock_option_level | 0 | 3 | 3 | 7.938776e-01 | 1.0 | 0.8520767 | 0.9679913 | 3.359316 |
| total_working_years | 0 | 40 | 40 | 1.127959e+01 | 10.0 | 7.7807817 | 1.1160316 | 3.911070 |
| training_times_last_year | 0 | 6 | 6 | 2.799320e+00 | 3.0 | 1.2892706 | 0.5525596 | 3.489232 |
| work_life_balance | 1 | 4 | 3 | 2.761224e+00 | 3.0 | 0.7064758 | -0.5519164 | 3.413956 |
| years_at_company | 0 | 40 | 40 | 7.008163e+00 | 5.0 | 6.1265252 | 1.7627284 | 6.918057 |
| years_in_current_role | 0 | 18 | 18 | 4.229252e+00 | 3.0 | 3.6231370 | 0.9164268 | 3.471720 |
| years_since_last_promotion | 0 | 15 | 15 | 2.187755e+00 | 1.0 | 3.2224303 | 1.9822646 | 6.596318 |
| years_with_curr_manager | 0 | 17 | 17 | 4.123129e+00 | 3.0 | 3.5681361 | 0.8326003 | 3.166398 |

#### 3.7.3. Check Unique Value Counts for Categorical Attributes

    ##       attrition business_travel      department education_field          gender 
    ##               2               3               3               6               2 
    ##        job_role  marital_status          over18       over_time 
    ##               9               3               1               2

#### 3.7.4. Analyze Target Variable (attrition)

    ## 
    ##   No  Yes 
    ## 1233  237

### 3.8. Checkpoint

``` r
# saves the current data set state
write.csv(df_hratt_cln, file = 'dataset/Human_Resources_clean.csv', row.names = FALSE)
```

## 4. Outlining the Hypotheses

### 4.1. Hypothesis generation

| Factor | Hypothesis |
|----|----|
| Age | People up to 40s tend to leave. |
|  | People 40+ tend not to leave. |
| Education | People that have higher degree of education tend to leave. |
|  | People that have lower degree of education tend not to leave. |
| Distance from home | People who live far from work tend to leave. |
|  | People who live near work tend not to leave. |
| Marital status | Single people tend to leave. |
|  | Married people tend not to leave. |
| Overtime | People who make overtime tend to leave. |
|  | People who don’t make overtime tend to stay. |
| Performance rating | People who present higher performance ratings tend to leave. |
|  | People who present lower performance ratings tend to leave. |
|  | People who present medium performance ratings tend not to leave. |
| Job level and role | People who have lower job level tend to leave. |
|  | People who have medium and higher job level tend not to leave. |
|  | People who weren’t promoted for a long time tend to leave. |
|  | People who are in the current role for a long time tend to leave. |
| Job involvement | People who feel less involved with the job tend to leave. |
|  | People who feel more involved with the job tend not to leave. |
| Job satisfaction | People who feel less satisfied with the job tend to leave. |
|  | People who feel more satisfied with the job tend not to leave. |
| Environment satisfaction | People who feel less satisfied with the environment tend to leave. |
|  | People who feel more satisfied with the environment tend not to leave. |
| Work life balance | People who have lower work life balance tend to leave. |
|  | People who have higher work life balance tend not to leave. |
| Working years | People who have worked professionally for more years tend not to leave. |
|  | People who have worked at the same company for more years tend not to leave. |
|  | People who are job hoppers tend to leave. |
| Payment | People who are making more money tend not to leave. |
|  | People who have lower salary hikes tend to leave. |
| Training | People who didn’t receive training for long years tend to leave. |
|  | People who constantly received training tend not to leave. |
| Current manager | People who have been working for the same manager for short years tend to leave. |
|  | People who have been working for the same manager for long years tend not to leave. |
|  | People who have lower quality of relationship with the manager tend to leave. |
|  | People who have higher quality of relationship with the manager tend not to leave. |
| Business travel | People who travel more frequently tend to leave. |
|  | People who don’t travel tend to stay. |
|  | People who travel less frequently tend to stay. |
| Other questions that we need to answer | Which departments has more turnover? |
|  | Which education field has more turnover? |

### 4.2. Selected Hypotheses

| Category | Hypotheses |
|----|----|
| **Age** | H1. People up to 40s tend to leave. |
| **Education** | H2. People that have higher degree of education tend to leave more. |
| **Distance from home** | H3. People who live far from work tend to leave. |
| **Marital status** | H4. Single people tend to leave. |
| **Overtime** | H5. People who make overtime tend to leave more. |
| **Performance rating** | H6. People who present higher performance ratings tend to leave more. |
|  | H7. People who present lower performance ratings tend to leave more. |
| **Job level and role** | H8. People who have lower job level tend to leave more. |
|  | H9. People who weren’t promoted for a long time tend to leave more. |
|  | H10. People who are in the current role for a long time tend to leave more. |
| **Job involvement** | H11. People who feel less involved with the job tend to leave more. |
| **Job satisfaction** | H12. People who feel less satisfied with the job tend to leave more. |
| **Environment satisfaction** | H13. People who feel less satisfied with the environment tend to leave more. |
| **Work life balance** | H14. People who have lower work life balance tend to leave more. |
| **Working years** | H15. People who professionally worked for more years tend to not leave. |
|  | H16. People who worked at the same company for more years tend not to leave. |
|  | H17. People who are job hoppers tend to leave. |
| **Payment** | H18. People who are making more money tend not to leave. |
|  | H19. People who have lower salary hikes tend to leave. |
| **Training** | H20. People who didn’t receive training for long years tend to leave more. |
| **Current manager** | H21. People who have been working for the same manager for short years tend to leave more. |
|  | H22. People who have lower quality of relationship with the manager tend to leave more. |
| **Business travel** | H23. People who travel more frequently tend to leave more. |
| **Other questions** | H24. Which departments have more turnover? |
|  | H25. Which education field has more turnover? |

## 5. Feature Engineering

### 5.1. Loading data set

``` r
# Loads data set
df_hratt <- read_csv("dataset/Human_Resources_clean.csv")
```

    ## Rows: 1470 Columns: 35
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (9): attrition, business_travel, department, education_field, gender, j...
    ## dbl (26): age, daily_rate, distance_from_home, education, employee_count, em...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

### 5.2. Column filtering

In this step of my analysis, I’m going to remove certain columns from my
dataset to simplify the data and focus on the most important
information.

1.  **Removing `over18` and `standard_hours`:**  

- I have a column called `over18`, which indicates whether employees are
  over 18 years old. Since all employees in my dataset are over 18, this
  column doesn’t provide any useful information.

- I also have a column called `standard_hours`, which shows the standard
  working hours for employees. If every employee has the same standard
  hours, this column also doesn’t help me differentiate between them.

Therefore, I will remove these columns from the dataset to streamline
the analysis and retain only the relevant information that can
contribute to my insights about employee attrition.

2.  **Dropping `employee_count` and `employee_number`:**
    - The `employee_count` column shows the total number of employees,
      but since this number is the same for everyone, it doesn’t provide
      any unique insights for our analysis.
    - The `employee_number` column is like an ID for each employee.
      While it identifies individuals, it doesn’t give us valuable
      information about their work or performance.

By removing these columns, I can focus on the information that will help
me analyze employee behavior and make better predictions about their
actions.

    ## # A tibble: 1 × 31
    ##     age attrition business_travel daily_rate department       distance_from_home
    ##   <dbl> <chr>     <chr>                <dbl> <chr>                         <dbl>
    ## 1    32 No        Non-Travel            1184 Research & Deve…                  1
    ## # ℹ 25 more variables: education <dbl>, education_field <chr>,
    ## #   environment_satisfaction <dbl>, gender <chr>, hourly_rate <dbl>,
    ## #   job_involvement <dbl>, job_level <dbl>, job_role <chr>,
    ## #   job_satisfaction <dbl>, marital_status <chr>, monthly_income <dbl>,
    ## #   monthly_rate <dbl>, num_companies_worked <dbl>, over_time <chr>,
    ## #   percent_salary_hike <dbl>, performance_rating <dbl>,
    ## #   relationship_satisfaction <dbl>, stock_option_level <dbl>, …

### 5.3. Preparing data for EDA

When I examine my data, I notice that some categories are represented by
numbers. For instance, education levels might be coded numerically
(e.g., 1 for ‘Below College,’ 2 for ‘College,’ etc.). This numerical
representation makes the data more challenging to interpret.

To enhance clarity, I will convert these numbers into descriptive
labels. By replacing the numerical codes with clear names, my data will
become easier to read and understand, ultimately improving the quality
of my analysis.

``` r
# Convert education levels
df_hratt <- df_hratt %>%
  mutate(education = recode(education, 
                            `1` = "Below College", 
                            `2` = "College", 
                            `3` = "Bachelor", 
                            `4` = "Master", 
                            `5` = "Doctor"))

# Convert environment satisfaction levels
df_hratt <- df_hratt %>%
  mutate(environment_satisfaction = recode(environment_satisfaction, 
                                           `1` = "Low", 
                                           `2` = "Medium", 
                                           `3` = "High", 
                                           `4` = "Very High"))

# Convert job involvement levels
df_hratt <- df_hratt %>%
  mutate(job_involvement = recode(job_involvement, 
                                  `1` = "Low", 
                                  `2` = "Medium", 
                                  `3` = "High", 
                                  `4` = "Very High"))

# Convert job levels
df_hratt <- df_hratt %>%
  mutate(job_level = recode(job_level, 
                            `1` = "Junior", 
                            `2` = "Mid", 
                            `3` = "Senior", 
                            `4` = "Manager", 
                            `5` = "Director"))

# Convert job satisfaction levels
df_hratt <- df_hratt %>%
  mutate(job_satisfaction = recode(job_satisfaction, 
                                   `1` = "Low", 
                                   `2` = "Medium", 
                                   `3` = "High", 
                                   `4` = "Very High"))

# Convert performance rating levels
df_hratt <- df_hratt %>%
  mutate(performance_rating = recode(performance_rating, 
                                     `1` = "Low", 
                                     `2` = "Good", 
                                     `3` = "Excellent", 
                                     `4` = "Outstanding"))

# Convert relationship satisfaction levels
df_hratt <- df_hratt %>%
  mutate(relationship_satisfaction = recode(relationship_satisfaction, 
                                            `1` = "Low", 
                                            `2` = "Medium", 
                                            `3` = "High", 
                                            `4` = "Very High"))

# Convert work life balance levels
df_hratt <- df_hratt %>%
  mutate(work_life_balance = recode(work_life_balance, 
                                    `1` = "Bad", 
                                    `2` = "Good", 
                                    `3` = "Better", 
                                    `4` = "Best"))
```

### 5.4. Checkpoint

``` r
# Save the current data set state
write.csv(df_hratt, file = "dataset/Human_Resources_fe.csv", row.names = FALSE)
```

## 6. Exploratory data analysis

### 6.1. Loading data set

``` r
df_hratt <- read.csv("dataset/Human_Resources_fe.csv")
```

### 6.2. Separating data types

``` r
#selects only numerical attributes
num_attributes <- df_hratt %>% select(where(is.numeric))
#selects only categorical attributes
cat_attributes <- df_hratt %>% select(where(negate(is.numeric)))
```

### 6.3. Univariate Analysis

In this section, I will conduct a univariate analysis to explore the
distribution and characteristics of my dataset. I will particularly
focus on the target variable, as well as other numerical and categorical
variables. This analysis will help me gain insights into the data and
better understand the factors influencing employee attrition.

#### 6.3.1. Target Variable

I will start by visualizing the target variable, attrition, using a
count plot to compare the number of employees who left the company
versus those who stayed. This visualization will help me understand the
distribution of attrition within the dataset. After plotting, I observe
that a significantly larger number of employees remained in the company
compared to those who left.
![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

To quantify this, I will count how many employees left the company and
how many stayed. I will display these numbers along with their
percentages. This analysis reveals that most employees (83.88%) stayed,
while only a small number (16.12%) left. Understanding these figures
helps me gain a clearer perspective on the situation.

    ## Number of employees who left: 237

    ## This is equivalent to 16.12 % of the total employees.

    ## Number of employees who stayed: 1233

    ## This is equivalent to 83.88 % of the total employees.

#### 6.3.2. Numerical Variables

Next, I will visualize the distribution of all numerical attributes in
the dataset using histograms. This step allows me to examine the
frequency distribution of numerical variables, helping to identify
patterns such as skewness, central tendencies, and potential outliers.
![](employee_attrition_analysis_files/figure-gfm/histogram-plotting-num-1.png)<!-- -->![](employee_attrition_analysis_files/figure-gfm/histogram-plotting-num-2.png)<!-- -->![](employee_attrition_analysis_files/figure-gfm/histogram-plotting-num-3.png)<!-- -->![](employee_attrition_analysis_files/figure-gfm/histogram-plotting-num-4.png)<!-- -->

As we can see, except for the age variable, no other variable follows a
normal distribution, and even age is not perfectly normal. Therefore,
it’s better to leave the age variable as it is, rather than attempting
normalization, which could lead to errors.

#### 6.3.3. Categorical Variables

Finally, I will analyze the categorical variables through count plots
for each attribute. I will create subplots for each categorical
variable, enabling me to compare the counts across different categories
effectively. This step enhances my understanding of how various
categorical factors are distributed in the dataset.
![](employee_attrition_analysis_files/figure-gfm/histogram-plotting-cat-1.png)<!-- -->![](employee_attrition_analysis_files/figure-gfm/histogram-plotting-cat-2.png)<!-- -->

### 6.4 Bivariate analysis - hypotheses validation

#### H1. People up to 40s tend to leave. **<span style="color: green;">TRUE</span>**

    ## 
    ## Call:  glm(formula = attrition ~ age, family = binomial, data = df_hratt)
    ## 
    ## Coefficients:
    ## (Intercept)          age  
    ##     0.20620     -0.05225  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1259  AIC: 1263

    ## [1] "There is a significant association between attrition and age."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

This analysis looks at how age affects whether people leave or stay
(attrition). The two variables used are *attrition* (Yes or No) and
*age* (ranging from 18 to 60 years old). A logistic regression test was
done to see if age is related to attrition. The result showed a p-value
less than 0.05, which means that there is a significant association
between age and attrition. Additionally, two bar charts were created to
show how attrition is distributed across different age groups. The
charts show that people under 40 have a higher rate of leaving
(attrition) compared to those over 40.

**Conclusion:** The analysis supports the hypothesis H1, showing a clear
association between age and attrition.

#### H2. People that have higher degree of education tend to leave more. **<span style="color: red;">FALSE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table_e
    ## X-squared = 3.074, df = 4, p-value = 0.5455

    ## [1] "There is no significant association between attrition and education."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

This analysis explores the association between education level and
attrition. The two variables examined are *attrition* (Yes or No) and
*education* (ranging from Below College to Doctorate). A Chi-square test
was performed to determine if there is an association between education
level and attrition. The result yielded a p-value greater than 0.05,
which suggests that there is no significant association between
education and attrition. Additionally, a bar chart was created to
visualize the attrition rate across different education levels (Below
College, College, Bachelor, Master, Doctor). The chart shows that the
attrition rate is approximately 0.125 across all education levels.

**Conclusion:** The analysis does not support the hypothesis H2.

#### H3. People who live far from work tend to leave. (Need for further analysis)

    ## 
    ## Call:  glm(formula = attrition ~ distance_from_home, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##        (Intercept)  distance_from_home  
    ##           -1.89005             0.02471  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1290  AIC: 1294

    ## [1] "There is a significant association between attrition and distance from home."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

The bar chart displays the distribution of attrition (Yes/No) across
different distances from home. From the chart, it can be observed that
there is no clear trend indicating an increase in attrition as the
distance from home increases. The proportions of employees leaving
(shown in blue) do not exhibit a significant rise across the different
distance categories.

**Conclusion:** Based on the Logistic Regression analysis (p-value \<
0.05), there is a significant association between attrition and distance
from home, suggesting that employees living farther from work are more
likely to leave. However, the bar chart does not visually support a
clear increase in attrition with distance, highlighting the need for
further analysis to better understand this relationship.

#### H4. Single people tend to leave more. **<span style="color: green;">TRUE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table_ms
    ## X-squared = 46.164, df = 2, p-value = 9.456e-11

    ## [1] "There is a significant association between attrition and marital status."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

This analysis explores the association between marital status and
attrition. The two variables examined are attrition (Yes or No) and
marital status (Single, Married, Divorced). A Chi-square test was
performed to determine if there is an association between marital status
and attrition. The result yielded a p-value less than 0.05, which
suggests that there is a significant association between marital status
and attrition. Additionally, a bar chart was created to visualize the
attrition rate across different marital statuses (Single, Married,
Divorced). The chart shows that the attrition rate for single employees
is higher compared to those who are married or divorced.

**Conclusion:** The analysis supports the hypothesis H4.

#### H5. People who make overtime tend to leave more. **<span style="color: green;">TRUE</span>**

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  table_ot
    ## X-squared = 87.564, df = 1, p-value < 2.2e-16

    ## [1] "There is a significant association between attrition and overtime."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-35-1.png)<!-- -->

This analysis explores the association between overtime and attrition.
The two variables examined are overtime (Yes or No) and attrition (Yes
or No). A Chi-square test was performed to determine if there is an
association between overtime and attrition. The result indicated a
significant association between the two variables. Additionally, a bar
chart was created to visualize the attrition rate for employees who make
overtime compared to those who do not. The chart shows that employees
who make overtime have a higher attrition rate compared to those who do
not.

**Conclusion:** The analysis supports the hypothesis H5

#### H6. People who present higher performance ratings tend to leave more. **<span style="color: red;">FALSE</span>**

#### H7. People who present lower performance ratings tend to leave more. **<span style="color: red;">FALSE</span>**

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  table_pr
    ## X-squared = 0.00015475, df = 1, p-value = 0.9901

    ## [1] "There is no significant association between attrition and performance ratings."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-38-1.png)<!-- -->

This analysis examines the association between performance ratings and
attrition, specifically testing whether employees with either higher or
lower performance ratings have different rates of attrition. The two
variables analyzed are performance rating (categorized as Outstanding
and Excellent) and attrition (Yes or No). A Chi-square test was
conducted to check for any association between performance ratings and
attrition, and the results yielded a p-value greater than 0.05,
indicating no statistically significant association. A bar chart was
generated to visualize attrition rates across different performance
ratings, showing similar attrition rates for both Outstanding and
Excellent ratings, with no data available for Low and Good ratings.

**Conclusion:** The analysis does not support either hypothesis H6 or
H7. There is no significant association between performance rating and
attrition, as employees with both higher and lower ratings exhibit
similar rates of attrition.

#### H8. People who have lower job level tend to leave more. **<span style="color: green;">TRUE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table_jl
    ## X-squared = 72.529, df = 4, p-value = 6.635e-15

    ## [1] "There is a significant association between attrition and job level."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-41-1.png)<!-- -->

This analysis examines the association between job level and attrition,
specifically testing whether employees at lower job levels tend to have
higher rates of attrition. The two variables analyzed are job level
(categorized as Junior, Mid, Senior, Manager, and Director) and
attrition (Yes or No). A Chi-square test was conducted to determine if
there is an association between job level and attrition, with results
yielding a p-value less than 0.05, indicating a statistically
significant association between these two variables. A bar chart was
also created to show attrition rates across job levels. The chart
indicates that Junior employees have the highest attrition rate,
followed by Senior, Mid, Manager, and Director.

It’s noteworthy that although Mid-level employees show a lower attrition
rate than Senior-level employees, the overall trend still aligns with
the hypothesis. The general pattern observed is that employees in
lower-level positions, particularly those at the Junior level, exhibit
higher rates of attrition compared to those at higher levels.

**Conclusion:** The analysis supports hypothesis H8.

#### H9. People who weren’t promoted for long time tend to leave more. **<span style="color: red;">FALSE</span>**

    ## 
    ## Call:  glm(formula = attrition ~ years_since_last_promotion, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##                (Intercept)  years_since_last_promotion  
    ##                   -1.58703                    -0.02979  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1297  AIC: 1301

    ## [1] "There is no significant association between attrition and years since last promotion."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-44-1.png)<!-- -->

This analysis examines the association between the length of time since
an employee’s last promotion and their likelihood of attrition, testing
whether employees who have not been promoted for a longer period are
more likely to leave. The two variables analyzed are
years_since_last_promotion and attrition (Yes or No). A Logistic
regression test was conducted to assess if there is an association
between these variables, yielding a p-value greater than 0.05,
suggesting no statistically significant association between years since
last promotion and attrition. Additionally, a bar chart was created to
visualize attrition rates across different intervals of years since last
promotion. The chart indicates no clear pattern linking the time since
last promotion to attrition rates.

**Conclusion:** The analysis does not support hypothesis H9.

#### H10. People who are in the current role for long time tend to leave more. (Need for further analysis)

    ## 
    ## Call:  glm(formula = attrition ~ years_in_current_role, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##           (Intercept)  years_in_current_role  
    ##               -1.1184                -0.1463  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1256  AIC: 1260

    ## [1] "There is a significant association between attrition and years in current role."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-47-1.png)<!-- -->

This analysis examines whether employees who have remained in their
current role for a longer duration are more likely to leave, testing the
hypothesis that a prolonged time in the current role increases attrition
likelihood. The two variables analyzed are years_in_current_role and
attrition (Yes or No). A Logistic regression test was conducted,
yielding a p-value less than 0.05, indicating a statistically
significant association between years in the current role and attrition.
However, a bar chart visualizing attrition rates across various lengths
of time in the current role does not display a clear pattern or trend
supporting this association.

**Conclusion:** The Logistic regression analysis suggests a
statistically significant association between years in the current role
and attrition. However, this finding is not supported by the bar chart,
which shows no distinct relationship between length of time in the
current role and attrition rates. This discrepancy indicates that while
a statistically significant association exists, it is not consistently
observed across all visualized data points.

#### H11. People who feel less involved with the job tend to leave more. **<span style="color: green;">TRUE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table_ji
    ## X-squared = 28.492, df = 3, p-value = 2.863e-06

    ## [1] "There is a significant association between attrition and job involvement."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-50-1.png)<!-- -->

This analysis explores the association between job involvement and
attrition, testing the hypothesis that people with lower job involvement
are more likely to leave. The two variables analyzed are attrition (Yes
or No) and job_involvement (categorized as Low, Medium, High, Very
High). A Chi-square test was conducted, showing a significant
association between job involvement and attrition. The bar chart
visualizing attrition rates across the different levels of job
involvement indicates that the highest attrition rate is observed in the
Low job involvement category, followed by Medium, High, and Very High
involvement categories in descending order. This pattern suggests that
lower job involvement may be associated with higher attrition rates.

**Conclusion:** The analysis supports hypothesis H11.

#### H12. People who feel less satisfied with the job tend to leave more. **<span style="color: green;">TRUE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table_js
    ## X-squared = 17.505, df = 3, p-value = 0.0005563

    ## [1] "There is a significant association between attrition and job satisfaction."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-53-1.png)<!-- -->

This analysis examines the association between job satisfaction and
attrition, testing the hypothesis that people who are less satisfied
with their job are more likely to leave. The two variables analyzed are
attrition (Yes or No) and job_satisfaction (categorized as Low, Medium,
High, Very High). A Chi-square test was conducted, revealing a
significant association between job satisfaction and attrition. The bar
chart visualizing attrition rates across different levels of job
satisfaction shows that the highest attrition rate is observed in the
Low job satisfaction category, followed by Medium, High, and Very High
satisfaction categories in descending order. This suggests that lower
job satisfaction may be associated with higher attrition rates.

**Conclusion:** The analysis supports hypothesis H12.

#### H13. People who feel less satisfied with the environment tend to leave more. **<span style="color: green;">TRUE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table_es
    ## X-squared = 22.504, df = 3, p-value = 5.123e-05

    ## [1] "There is a significant association between attrition and environment satisfaction."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-56-1.png)<!-- -->

This analysis examines the association between environment satisfaction
and attrition, testing the hypothesis that lower environment
satisfaction leads to higher attrition. The two variables analyzed are
attrition (Yes or No) and environment_satisfaction (Low, Medium, High,
Very High). A Chi-square test showed a significant association between
the two variables. The bar chart indicates the highest attrition rate in
the Low satisfaction category, followed by Medium, and then High and
Very High, with attrition rates for High and Very High being quite
similar. Although the differences between Medium, High, and Very High
are small, the trend still supports the hypothesis that lower
satisfaction is linked to higher attrition.

**Conclusion:** The analysis supports hypothesis H13.

#### H14. People who have lower work life balance tend to leave more. **<span style="color: green;">TRUE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table_wl
    ## X-squared = 16.325, df = 3, p-value = 0.0009726

    ## [1] "There is a significant association between attrition and work-life balance (Chi-square test)."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-59-1.png)<!-- -->

This analysis examines the association between work-life balance and
attrition, testing the hypothesis that people with lower work-life
balance are more likely to leave. The two variables analyzed are
attrition (Yes or No) and work_life_balance (Bad, Good, Better, Best). A
Chi-square test revealed a significant association between the two
variables. The bar chart shows the highest attrition rate in the Bad
work-life balance category, followed by similar rates in the Good and
Best categories, and the lowest rate in the Better category. Despite the
smaller differences between Good, Better, and Best, the trend still
supports the hypothesis that lower work-life balance is associated with
higher attrition.

**Conclusion:** The analysis supports hypothesis H14.

#### H15. People who professionally worked for more years tend to not leave. **<span style="color: green;">TRUE</span>**

    ## 
    ## Call:  glm(formula = attrition ~ total_working_years, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##         (Intercept)  total_working_years  
    ##            -0.88306             -0.07773  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1248  AIC: 1252

    ## [1] "There is a significant association between attrition and total working years."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-62-1.png)<!-- -->

This analysis examines the association between total working years and
attrition, testing the hypothesis that people with more professional
experience are less likely to leave. The two variables analyzed are
attrition (Yes or No) and total_working_years. A Logistic regression
test showed a significant association between the variables. The bar
chart shows that for most years of experience, attrition is low.
However, at 40 years of experience, attrition increases sharply. This
increase could be due to factors like retirement or career changes, but
the overall trend still supports the hypothesis.

**Conclusion:** The analysis supports hypothesis H15.

#### H16. People who worked at the same company for more years tend not to leave. **<span style="color: green;">TRUE</span>**

    ## 
    ## Call:  glm(formula = attrition ~ years_at_company, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##      (Intercept)  years_at_company  
    ##         -1.15577          -0.08076  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1266  AIC: 1270

    ## [1] "There is a significant association between attrition and years at company."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-65-1.png)<!-- -->

This analysis examines the association between years at the company and
attrition, testing the hypothesis that employees with more years at the
company are less likely to leave. A Logistic regression test revealed a
significant association between attrition (Yes or No) and
years_at_company.

The bar chart shows a general decline in attrition up to 22 years,
followed by a sharp increase at 23 years, then a drop to 0. After 31-33
years, attrition rises again before dropping to 0, with another spike at
40 years. These fluctuations could be related to factors like retirement
or career changes, but overall, attrition tends to be lower for
employees with more years at the company.

**Conclusion:** The analysis supports the hypothesis, although the
spikes in attrition at certain years (23, 31-33, and 40) suggest other
factors may influence attrition at these points.

#### H17. People who are job hoppers tend to leave more. **<span style="color: red;">FALSE</span>**

    ## 
    ## Call:  glm(formula = attrition ~ num_companies_worked, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##          (Intercept)  num_companies_worked  
    ##             -1.77652               0.04565  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1296  AIC: 1300

    ## [1] "There is no significant association between attrition and the number of companies worked."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-68-1.png)<!-- -->

This analysis examines the association between the number of companies
worked at and attrition, testing the hypothesis that job hoppers are
more likely to leave. The two variables analyzed are attrition (Yes or
No) and num_companies_worked. A Logistic regression test showed no
significant association between the two variables. The bar chart shows
no clear trend, with attrition rates remaining relatively stable across
different numbers of companies worked. This lack of pattern suggests
that the number of companies an employee has worked for does not
significantly affect their likelihood of leaving.

**Conclusion:** The analysis does not support hypothesis H17.

#### H18. People who are making more money tend not to leave. (Need for further analysis)

    ## 
    ## Call:  glm(formula = attrition ~ hourly_rate, family = binomial, data = df_hratt)
    ## 
    ## Coefficients:
    ## (Intercept)  hourly_rate  
    ##  -1.5889155   -0.0009159  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1299  AIC: 1303

    ## 
    ## Call:  glm(formula = attrition ~ daily_rate, family = binomial, data = df_hratt)
    ## 
    ## Coefficients:
    ## (Intercept)   daily_rate  
    ##  -1.3495400   -0.0003834  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1294  AIC: 1298

    ## 
    ## Call:  glm(formula = attrition ~ monthly_rate, family = binomial, data = df_hratt)
    ## 
    ## Coefficients:
    ##  (Intercept)  monthly_rate  
    ##   -1.733e+00     5.798e-06  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1298  AIC: 1302

    ## [1] "Hourly rate is not significantly associated with attrition."

    ## [1] "Daily rate is significantly associated with attrition. (Odds Ratio:  1 )"

    ## [1] "Monthly rate is not significantly associated with attrition."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-70-1.png)<!-- -->

This analysis examines the association between compensation and
attrition, testing the hypothesis that people earning more money are
less likely to leave. The variables analyzed are attrition (Yes or No),
hourly_rate, daily_rate, and monthly_rate. A Logistic regression test
showed that only daily_rate has a significant association with
attrition. The boxplot further supports this finding, as it shows that
for daily_rate, the no attrition group has a higher median compared to
the yes attrition group. However, no clear pattern was observed for
hourly_rate and monthly_rate in relation to attrition.

**Conclusion:** While the analysis suggests that daily_rate is
associated with attrition, further investigation is needed to understand
why hourly_rate and monthly_rate do not show similar patterns.
Additional factors may be influencing attrition that were not captured
in this analysis.

#### H19. People who have shorter salary hike range tend to leave. **<span style="color: red;">FALSE</span>**

    ## 
    ## Call:  glm(formula = attrition ~ percent_salary_hike, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##         (Intercept)  percent_salary_hike  
    ##            -1.49563             -0.01012  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1298  AIC: 1302

    ## [1] "There is no significant association between attrition and percent_salary_hike."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-73-1.png)<!-- -->

This analysis examines the association between salary hike range and
attrition, testing the hypothesis that people with shorter salary hike
ranges are more likely to leave. The variables analyzed are attrition
(Yes or No) and percent_salary_hike. A Logistic regression test showed
no significant association between the two variables. The bar chart also
does not reveal any clear relationship, with attrition rates being
fairly consistent across different ranges of salary hikes.

**Conclusion:** The analysis does not support hypothesis H19.

#### H20. People who received less training last year tend to leave more. (Need further analysis)

    ## 
    ## Call:  glm(formula = attrition ~ training_times_last_year, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##              (Intercept)  training_times_last_year  
    ##                  -1.2948                   -0.1299  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1468 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1293  AIC: 1297

    ## [1] "There is a significant association between attrition and training times last year."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-76-1.png)<!-- -->

This analysis examines the association between training received last
year and attrition, testing the hypothesis that people who received less
training tend to leave more. The variables analyzed are attrition (Yes
or No) and training_times_last_year. A Logistic regression test showed a
significant association between the two variables. However, the bar
chart does not reveal a clear relationship, as attrition rates appear
relatively stable across different levels of training received.

**Conclusion:** While the analysis indicates a significant association
between training_times_last_year and attrition, the bar chart suggests
that other factors may be influencing attrition. Further analysis could
help clarify the relationship between these variables.

#### H21. People who have been working for the same manager for short years tend to leave more. **<span style="color: red;">FALSE</span>**

    ## 
    ## Call:  glm(formula = attrition ~ relationship_satisfaction, family = binomial, 
    ##     data = df_hratt)
    ## 
    ## Coefficients:
    ##                        (Intercept)        relationship_satisfactionLow  
    ##                           -1.69833                             0.35231  
    ##    relationship_satisfactionMedium  relationship_satisfactionVery High  
    ##                           -0.04797                            -0.05087  
    ## 
    ## Degrees of Freedom: 1469 Total (i.e. Null);  1466 Residual
    ## Null Deviance:       1299 
    ## Residual Deviance: 1294  AIC: 1302

    ## [1] "There is no significant association between relationship satisfaction and attrition."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-79-1.png)<!-- -->

This analysis examines the association between years with the current
manager and attrition, testing the hypothesis that people who have been
working with the same manager for fewer years tend to leave more. The
variables analyzed are attrition (Yes or No) and
years_with_curr_manager. A Logistic regression test showed no
significant association between the two variables. The bar chart reveals
that the density of attrition (Yes) generally decreases as years with
the current manager increase. However, at 14 years, the density of
attrition unexpectedly rises before dropping back to 0 in subsequent
years.

**Conclusion:** The analysis does not support hypothesis H21

#### H22. People who have lower quality of relationship with the manager tend to leave more. **<span style="color: red;">FALSE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  contingency_table
    ## X-squared = 5.2411, df = 3, p-value = 0.155

    ## [1] "There is no significant association between relationship satisfaction and attrition."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-82-1.png)<!-- -->

This analysis examines the association between relationship satisfaction
with the manager and attrition, testing the hypothesis that people with
lower quality relationships with their manager tend to leave more. The
variables analyzed are attrition (Yes or No) and
relationship_satisfaction (Low, Medium, High, Very High). A Chi-square
test showed no significant association between the two variables. The
bar chart reveals that the highest attrition rate is in the Low
relationship satisfaction category, followed by Medium, with High and
Very High having similar, lower rates of attrition.

**Conclusion:** The analysis does not support hypothesis H22.

#### H23. People who travel more frequently tend to leave more. **<span style="color: red;">FALSE</span>**

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  contingency_table
    ## X-squared = 24.182, df = 2, p-value = 5.609e-06

    ## [1] "There is a significant association between business travel and attrition."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-85-1.png)<!-- -->

This analysis examines the association between business travel frequency
and attrition, testing the hypothesis that people who travel more
frequently tend to leave more. The variables analyzed are attrition (Yes
or No) and business_travel (Non-Travel, Travel Rarely, Travel
Frequently). A Chi-square test showed a significant association between
the two variables. The bar chart indicates that the highest attrition
rate is in the “Travel Frequently” category, followed by “Travel
Rarely,” and the lowest attrition rate is in the “Non-Travel” category.

**Conclusion:** The analysis supports hypothesis H23

#### H24. Which departments has more turnover?

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  contingency_table
    ## X-squared = 10.796, df = 2, p-value = 0.004526

    ## [1] "There is a significant association in attrition rates across departments."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-88-1.png)<!-- -->

This analysis examines which departments have higher turnover, testing
the hypothesis that some departments experience more attrition than
others. The variables analyzed are attrition (Yes or No) and department.
A Chi-square test showed a significant association between the two
variables. The bar chart indicates that the highest attrition rate is in
the Sales department, followed by HR, and the lowest attrition rate is
in the R&D department.

**Conclusion:** The analysis confirms that there is a significant
association between department and attrition. The Sales department has
the highest turnover, followed by HR, while the R&D department shows the
lowest attrition rate.

#### H25. Which education field has more turnover? (Need for further analysis)

    ## Warning in stats::chisq.test(x, y, ...): Chi-squared approximation may be
    ## incorrect

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  contingency_table_education
    ## X-squared = 16.025, df = 5, p-value = 0.006774

    ## [1] "There is a significant association between education field and attrition."

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-91-1.png)<!-- -->

This analysis examines which education field has higher turnover,
testing the hypothesis that certain education fields experience more
attrition. The variables analyzed are attrition (Yes or No) and
education_field. A Chi-square test showed an association between the two
variables; however, it was noted that the Chi-square test results may be
unreliable. The bar chart indicates that the highest attrition rate is
in the HR field, followed by Technical Degree, Marketing, and other
fields.

**Conclusion:** The analysis suggests a potential association between
education field and attrition, with the highest turnover in the HR
field, followed by Technical Degree and Marketing. However, due to
concerns about the reliability of the Chi-square results, further
investigation is needed to confirm these findings.

#### Hypotheses Summary

| Hypothesis | Conclusion |
|:---|:---|
| H1: People up to 40s tend to leave | TRUE |
| H2: People with higher education tend to leave more | FALSE |
| H3: People who live far from work tend to leave | Need for further analysis |
| H4: Single people tend to leave more | TRUE |
| H5: People who work overtime tend to leave more | TRUE |
| H6: People with higher performance ratings tend to leave more | FALSE |
| H7: People with lower performance ratings tend to leave more | FALSE |
| H8: People with lower job levels tend to leave more | TRUE |
| H9: People not promoted for a long time tend to leave more | FALSE |
| H10: People in the same role for a long time tend to leave more | Need for further analysis |
| H11: People who feel less involved tend to leave more | TRUE |
| H12: People who feel less satisfied with the job tend to leave more | TRUE |
| H13: People who feel less satisfied with the environment tend to leave more | TRUE |
| H14: People with lower work-life balance tend to leave more | TRUE |
| H15: People with more professional years tend not to leave | TRUE |
| H16: People with more years in the company tend not to leave | TRUE |
| H17: People who are job hoppers tend to leave more | FALSE |
| H18: People with higher salaries tend not to leave | Need for further analysis |
| H19: People with shorter salary hike ranges tend to leave | FALSE |
| H20: People with less training last year tend to leave more | Need for further analysis |
| H21: People who have worked with the same manager for short years tend to leave more | FALSE |
| H22: People with lower manager relationship quality tend to leave more | FALSE |
| H23: People who travel frequently tend to leave more | FALSE |
| H24: Which department has more turnover? | Sales |
| H25: Which education field has more turnover? | Need for further analysis |

Summary of Hypotheses Testing Results

### 6.5. Multivariate analysis - correlations

#### 6.5.1. Correlation between numerical attributes

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-93-1.png)<!-- -->

**Analysis of the Data:**

Correlation between age and other factors:

- Monthly income has a strong correlation with age (0.497), suggesting
  that older employees tend to have higher salaries. This could reflect
  that employees with more years of experience tend to reach higher
  salary levels.
- Total working years also shows a strong correlation with age (0.680),
  indicating that older employees often have more years of experience in
  the workforce.
- Years at company and years in current role also show high correlations
  with age, at 0.514 and 0.364, respectively. This suggests that older
  employees tend to stay longer at a company and often remain in the
  same role for longer periods.

Correlation between salary and other factors:

- Monthly income shows a strong correlation with total working years
  (0.773), implying that employees with more experience tend to have
  higher salaries.
- Monthly income also has a weak negative correlation with percent
  salary hike (-0.027), indicating that employees with higher salaries
  may not necessarily receive substantial salary increases in recent
  years.

Correlation between years of work experience and other factors:

- Total working years shows a strong correlation with years at company
  (0.628) and years in current role (0.460), indicating that employees
  with more years of experience tend to stay longer at a company and
  often in the same role.

Other factors such as the number of companies worked at and training
hours:

- Number of companies worked at (0.149) has a weak correlation with
  monthly income, suggesting that employees who have worked at multiple
  companies do not necessarily earn higher salaries.
- Training hours last year show weak correlations with most other
  factors, indicating that training does not have a clear relationship
  with salary or years of experience.

**Conclusion:**

- Factors such as age, total working years, and tenure at the company
  have a significant impact on salary and career progression. Older
  employees with more experience and longer tenure at a company
  typically earn higher salaries.
- However, factors like the number of companies worked at or the number
  of training hours have weak relationships with other factors,
  suggesting they are not directly decisive in determining salary or
  career advancement.
- Employee training and development programs may need further evaluation
  to determine their impact on performance and salary growth.

#### 6.5.2. Correlations between categorical attributes

    ## Warning in stats::chisq.test(x, y, ...): Chi-squared approximation may be
    ## incorrect
    ## Warning in stats::chisq.test(x, y, ...): Chi-squared approximation may be
    ## incorrect
    ## Warning in stats::chisq.test(x, y, ...): Chi-squared approximation may be
    ## incorrect
    ## Warning in stats::chisq.test(x, y, ...): Chi-squared approximation may be
    ## incorrect

    ##                   job_role department business_travel
    ## job_role        1.00000000 0.93939265     0.066845326
    ## department      0.93939265 1.00000000     0.007979847
    ## business_travel 0.06385489 0.00828664     0.044060835

![](employee_attrition_analysis_files/figure-gfm/unnamed-chunk-94-1.png)<!-- -->

The correlation analysis between job_role, department, and
business_travel shows:

- job_role and department: There is a strong correlation (0.939),
  meaning job roles are closely linked to specific departments. Each
  department tends to have certain job roles.
- job_role and business_travel: The correlation is very low (0.067),
  which means that job roles don’t have much impact on business travel.
  Employees in different roles seem to travel for work at similar rates.
- department and business_travel: The correlation is almost zero
  (0.008), suggesting that business travel is not influenced by the
  department an employee works in.

In conclusion, while job roles are closely tied to departments, business
travel is mostly unaffected by both job roles and departments.

## 7. Insights and Interpretation

The exploratory data analysis (EDA) of employee attrition has uncovered
critical trends that reveal both high retention rates (83.88%) and a
focused set of factors contributing to the smaller, but significant,
attrition group (16.12%). Through examining both numerical and
categorical data, we found patterns that could help in identifying
specific employee groups with higher likelihoods of leaving the company.
These insights lay a foundation for targeted retention efforts and
further, more focused analysis.

### 7.1. Key Patterns and Relationships

- **Demographics and Salary:** We found strong relationships between
  age, work experience, and monthly income. Older employees with more
  years in the workforce tend to earn higher salaries and are more
  likely to stay with the company. Additionally, older employees are
  often in longer-tenured positions, showing less job movement, which
  suggests a more stable relationship with the company.
- **Job Satisfaction and Retention:** Employees who report low
  satisfaction—whether due to work-life balance, involvement, job level,
  or the work environment—tend to leave more frequently. This suggests
  that improving job satisfaction could significantly enhance retention.
- **Department-Specific Trends:** The Sales department has a higher
  turnover rate compared to other departments. Additionally, job roles
  are closely linked to departments, indicating that specific roles
  within certain departments may have unique retention challenges.
- **Career Progression and Training:** Employees with longer tenure or
  who remain in the same role for extended periods tend to stay with the
  company. Training, however, does not show a strong relationship with
  retention or salary. This weak link suggests that training may not
  directly influence retention or career advancement, though it may have
  indirect benefits.

### 7.2. Implications for Further Analysis

The identified patterns suggest several avenues for deeper analysis and
action:

- Predictive Modeling: Factors such as age, job satisfaction, and job
  level could be key inputs for predicting attrition. Further modeling
  can use these insights to identify employees at higher risk of
  leaving.
- Targeted Retention Strategies: Specific departments, especially Sales,
  and employees with low satisfaction levels would benefit from focused
  retention strategies. Further analysis may reveal more targeted
  approaches within these groups.
- Optimizing Training Programs: Given the minimal impact of training on
  retention or salary, a closer look at how training aligns with
  employee satisfaction and performance could help refine these programs
  to add more value.

These insights from the EDA provide a solid understanding of employee
attrition patterns and guide the next steps in formulating predictive
models and strategic retention plans.

## 8. Conclusion

The analysis provides valuable insights into the factors influencing
employee attrition and addresses the key questions guiding this project:

**Are certain groups of employees more likely to leave than others?**

Based on the analysis, specific groups exhibit a higher likelihood of
attrition:

- Employees in the HR field show the highest turnover rates among all
  education fields.
- Employees aged under 40 are more prone to leave their jobs.
- Single employees are more likely to experience attrition compared to
  their married counterparts.
- Employees with lower job levels tend to leave at higher rates.
- Employees who report lower satisfaction with their jobs, environment,
  or work-life balance are more likely to leave.

**What characteristics are commonly found among employees who decide to
leave?**

The analysis identified several common characteristics among employees
who tend to leave:

- Lack of job involvement significantly correlates with higher attrition
  rates.
- Dissatisfaction with the work environment and job satisfaction are
  prominent factors driving attrition.
- Limited opportunities for promotion contribute to higher turnover.
- Employees remaining in the same role for extended periods are more
  likely to leave.
- Frequent overtime is a significant predictor of attrition.
- Lower quality relationships with their managers are associated with
  higher turnover rates.

**Which factors can be managed or improved to help reduce attrition
rates?**

To mitigate attrition, organizations can focus on:

- Enhancing job satisfaction by fostering a supportive and engaging work
  culture.
- Promoting career growth opportunities to prevent stagnation in roles
  and provide clear advancement paths.
- Addressing work-life balance concerns by reducing excessive overtime
  and offering flexible work arrangements.
- Strengthening manager-employee relationships through targeted training
  programs and fostering open communication channels.

**Additional Insights**

- Strong correlations were observed between age and variables such as
  monthly income, total working years, and years in the company,
  indicating that older employees with more experience are generally
  less likely to leave.
- Job roles are closely linked to departments but have minimal impact on
  business travel, highlighting opportunities for role-specific
  interventions rather than department-wide strategies.
- Training hours and the number of companies worked at show weak
  correlations with key attrition factors, suggesting these are not
  primary drivers of turnover.

**Reliability and Future Work**

While the findings reveal meaningful trends, the reliability of some
statistical tests, such as the Chi-square test for education fields,
requires further investigation. Future research should focus on
gathering more robust data and conducting additional tests to validate
these conclusions.

By understanding and addressing these factors, organizations can develop
effective strategies to retain valuable employees and reduce attrition
rates.

## 9. Limitations

While this project provides valuable insights into employee attrition
through exploratory data analysis (EDA), there are several limitations
that need to be considered

- Focus on Exploratory Data Analysis: The project primarily centers
  around EDA, which is an essential step in understanding the data.
  However, it does not extend beyond this phase to more advanced
  analyses or predictive modeling. Without further analysis, the
  findings may lack the depth necessary to draw strong, actionable
  conclusions that can directly inform HR strategies. -Need for Further
  Analysis: As the project concludes after the EDA phase, it does not
  dive into validating or refining the insights gained. For example,
  factors such as salary, training, and job distance need further
  analysis to fully explore their impact on attrition. Without this next
  step, the insights may remain underdeveloped and not as robust as they
  could be for practical use in HR decision-making.
- Data Quality: The dataset used in this project is idealized and
  sourced from GitHub for educational purposes. As such, it lacks
  real-world complexities and does not necessarily reflect the nuances
  of an actual company’s workforce. Consequently, while the project is
  beneficial for practicing data analysis techniques, the insights may
  not fully align with the realities of employee attrition in a
  corporate setting.

Therefore, this project serves more as a training exercise in data
analysis rather than a fully actionable study.
