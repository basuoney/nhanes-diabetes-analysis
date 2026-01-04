# nhanes-diabetes-analysis

# Diabetes Risk Analysis - NHANES Data

Analyzing what factors are associated with diabetes using public health survey data from NHANES.

## Why This Project?

Diabetes is a huge public health problem affecting millions of Americans. I wanted to understand which risk factors matter most - especially the ones we can actually do something about like BMI and physical activity. This analysis uses logistic regression to identify key predictors of diabetes.

## The Data

I used the NHANES dataset (National Health and Nutrition Examination Survey) which has health data on ~10,000 people. After cleaning, I had 7,959 people with complete information. About 9% had diabetes.

**Variables I looked at:**
- Age
- Gender  
- BMI (body mass index)
- Blood pressure
- Physical activity (yes/no)

I picked these because they're well-known diabetes risk factors and include both things you can change (BMI, activity) and things you can't (age, gender).

## What I Found

Built a logistic regression model and got an AUC of 0.81, which means the model does a pretty good job separating people with and without diabetes.

**The big three predictors:**
- **Age:** Each year older = 6% higher odds of diabetes
- **Being male:** 41% higher odds compared to females
- **BMI:** Each unit higher = 11% higher odds

**What didn't matter (statistically):**
- Blood pressure (p = 0.96)
- Physical activity (p = 0.17)

The physical activity thing surprised me. I expected it to be significant based on all the health guidelines about exercise preventing diabetes. Maybe it's because I only measured it as yes/no, which is pretty crude. Or maybe the effect shows up through BMI instead (active people → lower BMI → less diabetes).

## The Threshold Problem (My Main Learning)

This was the most interesting part. When I first tested the model using the standard 0.5 probability cutoff, it only caught **4.6%** of diabetes cases. That's terrible - basically useless for screening.

The problem? With only 9% of people having diabetes, the model learned to be super conservative. Using 0.5 as the cutoff doesn't make sense when the disease is this rare.

I fixed it by using something called Youden's Index to find a better threshold (0.083). With this:
- **Sensitivity jumped to 85%** - now catches most diabetes cases
- Specificity dropped to 69% but that's an acceptable tradeoff

This taught me that you can't just use default settings without thinking about your actual problem. For health screening, missing true cases is way worse than some false alarms.

## Files in This Repo

```
├── diabetes_analysis.R          # The R code
├── figures/                     # Plots I made
└── model_coefficients.csv       # Model results
```

## Running It Yourself

Need these R packages: `tidyverse`, `NHANES`, `broom`, `pROC`, `caret`, `car`

Then just run:
```r
source("diabetes_analysis.R")
```

Takes about 30 seconds. Creates all the plots and outputs.

## What I'd Do Differently

**Missing data:** I just dropped people with missing info (lost 20% of data). Turned out the missing data was mostly from kids/teens, so not a huge deal for studying adult diabetes, but for a real research paper I'd use multiple imputation.

**Physical activity:** The yes/no measure is too simple. Would be better to ask about hours per week or intensity levels.

**More variables:** NHANES has diet info, family history, cholesterol levels, etc. Could add those.

**Cross-validation:** I just did a simple train/test split. Would be more rigorous to do k-fold cross-validation.

**External validation:** Should test the model on a completely different dataset to see if it generalizes.

## What This Taught Me

1. Default thresholds (like 0.5) don't work for imbalanced data
2. Always check your missing data patterns before dropping it
3. How you measure things matters - crude variables give crude results
4. Sometimes "non-significant" results are the most interesting because they make you question your assumptions
5. A simple model that you understand is better than a complex one you don't

This was my first complete start-to-finish health data analysis project. Not perfect but I learned a ton about what goes into real public health research beyond just running models.

---

**Contact:** [Your Name] | [your.email@example.com]

*January 2026 - Created for MPH Data Science application*
