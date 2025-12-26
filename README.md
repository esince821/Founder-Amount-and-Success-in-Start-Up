# Founder-Number-and-Success-in-StartUp

## Table of Contents
- [Project Description](#project--description)  
- [Motivation](#motivation)  
- [Datasets](#dataset)  
- [Methods](#methods)  
- [Limitations & Future Work](#limitations--&--future--work)   

---

## Project Description
This project investigates how the number of founders of a startup correlates with success indicators such as funding stage(the level of investment a startup has reached, e.g., Seed, Series A, etc.) and longevity.
Although many startups are founded by two people (often hypothesized as one with technical skills and one with business skills), this analysis aims to validate that pattern using real‑world data.

---

## Motivation
The composition of a founding team and the stage of funding are central topics in startup and venture capital research. Some theories suggest that:

- Larger founding teams can combine more skills and networks, potentially attracting more capital.
- Later funding stages, by definition, are linked to higher valuations and larger deal sizes.
- Younger vs. older startups may face different funding environments.

As I have an interest on entrepreneurship, I wanted to examine these ideas using real startup data. Understanding these relationships can help founders and investors develop more realistic expectations about funding, and can serve as a small, data-driven case study of how team structure and growth stage interact with investment outcomes.

---

## Datasets

### Dataset 1: Startup Information (Raw)
* **Name:** Startup Informations
* **Source:** Kaggle.com
* **Link:** https://www.kaggle.com/datasets/ramjasmaurya/indian-startupsin-2021?resource=download
* **Data Acquisition Method:** Downloaded the dataset as a CSV file using Kaggle’s download feature and imported it into a Jupyter notebook (Anaconda environment). 


### Key Columns for CSV

#### 1. `founders.csv`
| Column Name     | Description                              |
|-----------------|------------------------------------------|
| `Company/Brand` | Name of the startup |  
| `Founded` | Founding year | 
| `Headquarters` | Location of the company |  
| `Sector` | Industry/sector label | 
| `What it does` | Short textual description | 
| `Founder/s` | Names of founders in a single text field | 
| `Investor/s` | Names of major investors | 
| `Amount(in dollars)` | Total funding amount (string with currency formatting) | 
| `Stage` | Funding stage (e.g., Pre-seed, Seed, Series A, Series B, …) | 
| `Month` | Month of funding |

---

## Methods
### 1. Data Collection 

1.  **Data Cleaning**
   - Removed encoding artifacts from column names (e.g., BOM characters in `"Company/Brand"`).
   - Stripped whitespace from all column names.
   - Converted `Amount(in dollars)` from string format to a numeric variable `Amount_clean`.
   - Dropped rows with missing values in critical fields such as `Amount_clean` and `Stage` for the main analysis dataset.

2.  **Feature Engineering**
   - **Founder Count (`founder_count`)**:  
     Parsed the `Founder/s` text field by splitting on commas, then counted the resulting names to obtain the number of founders per startup.
   - **Log Funding (`log_amount`)**:  
     Created `log_amount = log10(Amount_clean + 1)` to stabilize variance and make funding distributions more interpretable in boxplots.
   - **Founded Year (`Founded_year`)**:  
     Converted the `Founded` column to a numeric year where possible, to study trends over time.

3.  **Final Analysis Dataset**
   - The cleaned and feature-engineered data is stored as `startupdataset_final_clean_l.csv` and used for all EDA and hypothesis exploration.

## Analysis Plan

### 1. Exploratory Data Analysis (EDA)

**Univariate EDA**
- Examined distribution of:
  - `founder_count` (histograms, value counts)
- Calculated summary statistics (`.describe()`) for key numeric variables.

**Bivariate EDA**

- **Founder Count vs Funding Amount**
  - Grouped by `founder_count` and computed the `count`, `mean`, and `median` funding for each group.
  - Plotted boxplots of `log_amount` by `founder_count` to compare funding distributions across different team sizes.

- **Funding Stage vs Funding Amount**
  - Grouped the data by `Stage` and calculated the number of startups and median `log_amount` for each stage.
  - Plotted boxplots of `log_amount` by `Stage` (only including stages with enough observations) to see how typical funding changes from early rounds (Pre-seed, Seed) to later rounds (Series A–F).

- **Sector vs Funding Amount**
  - Examined how funding varies across different `Sector` categories.
  - Focused on the sectors with more observations and compared their funding using `log_amount` (e.g., with grouped summaries or boxplots).

- **Founded Year vs Funding Amount**
  - Investigated how funding relates to `Founded_year`.
  - Created a scatter plot of `log_amount` versus `Founded_year` and computed the correlation to check whether older or newer startups tend to receive higher funding.


### 2. Hypothesis Exploration

The project qualitatively examines the following questions:

- **H₀ (RQ1):** There is no relationship between the number of founders and funding amount.  
- **H₁ (RQ1):** Startups with more founders tend to receive higher funding.

- **H₀ (RQ2):** Funding stage is not associated with funding amount.  
- **H₁ (RQ2):** Later stages (Series A–F) are associated with larger funding amounts than early stages (pre-seed, seed).

- **H₀ (RQ3):** There is no relationship between founding year and funding amount.  
- **H₁ (RQ3):** Newer startups receive higher funding amounts than older ones.

Correlation coefficients, group summaries, and boxplots are used to assess these hypotheses descriptively rather than with full formal statistical tests.

---
## Machine Learning Models

In addition to exploratory analysis and hypothesis testing, I trained simple
regression models to predict the log of total funding (`log_amount`) for each
startup.

### Goal

The main prediction task is:

> Predict `log_amount` (log₁₀ of the total funding amount) from basic startup features.

The features used are:

- `founder_count` – number of founders
- `Founded_year` – year the startup was founded
- `Stage` – funding stage (Pre-seed, Seed, Series A–F, etc., one-hot encoded)

The target variable is:

- `log_amount` – log₁₀-transformed version of `Amount_clean`, used to reduce skewness
  in funding amounts.

Only rows with complete values for these columns were kept for modelling.

### Models

I trained two regression models:

1. **Linear Regression (Baseline)**  
   A simple linear model that assumes a straight-line relationship between the
   features and `log_amount`. This model is mainly used as a baseline to check
   how much variation can be explained with a very simple approach.

2. **Random Forest Regressor (Main Model)**  
   A tree-based ensemble model that combines many decision trees.  
   Random Forest can capture non-linear relationships and interactions between
   features (for example, between `Stage` and `Founded_year`) and does not
   require feature scaling.

The data was split into training and test sets:

- 80% for training  
- 20% for testing (used only for evaluation)

### Evaluation

The models were evaluated on the test set using:

- **R² (coefficient of determination)** – how much of the variation in `log_amount`
  is explained by the model (values closer to 1 are better).
- **RMSE (Root Mean Squared Error)** on the log₁₀ scale – typical size of the
  prediction error in log units.

For the **Random Forest model**:

- **R² ≈ 0.69** on the test set  
- **RMSE ≈ 0.47** on the log₁₀ scale

This means that:

- The model explains about **69%** of the variation in log funding.
- The prediction error on the log scale is moderate, which is reasonable given
  the high variability and noise in startup funding.

Overall, the Random Forest model performs noticeably better than the simple
Linear Regression baseline, and supports the earlier results from EDA and
hypothesis testing: funding **Stage** carries the strongest signal for funding
amount, while `founder_count` and `Founded_year` have a smaller additional
effect.

### Outputs

The predictions from the Random Forest model on the test set are saved in:

- `ml_predictions_random_forest.csv`

This file includes:

- `Company/Brand`  
- `log_amount_actual` – true log funding  
- `log_amount_predicted` – model prediction (rounded to 2 decimals)

These predictions can be used for inspection, plotting, or further analysis.

---

## Key Insights 

- **Funding Stage vs Funding Amount:**  
  Median log funding clearly increases from early stages (Pre-seed, Seed) to later rounds (Series C–F). Because the y-axis is on a log₁₀ scale, a difference of about 1 unit corresponds to roughly a tenfold change in funding. This, together with the ANOVA results, shows that funding stage is strongly associated with funding size.

- **Founder Count vs Funding Amount:**  
  Boxplots and the Pearson correlation both suggest that there is no clear linear relationship between `founder_count` and `log_amount`. The distributions for different founder team sizes overlap heavily, and although some small teams raise very large amounts, having more founders does not systematically lead to higher funding in this dataset.

- **Founded Year vs Funding Amount:**  
  There is a modest **negative** correlation between `Founded_year` and `log_amount`. Older startups tend to have slightly higher funding on average than more recently founded startups, but the effect size is limited. Founding year alone is not enough to reliably predict funding.

- **Sector Effects:**  
  Some sectors (e.g., finance, biotechnology, crypto) show very wide funding ranges, with both small and extremely large deals, while others have more concentrated funding levels. This suggests that industry can influence funding patterns, although there is still substantial variation within each sector.

---

## Timeline
| Task | Deadline |
|------|----------|
|Project Proposal Submission| October 31, 2025|
|EDA & Hypothesis Testing| November 28, 2025|
|ML Phase| ...|

---
## Limitations and Future Work

### Limitations

- **Single Dataset and Context:**  
  The analysis is based on one Kaggle dataset of startups, which seems to be limited to a specific region and time period. As a result, the findings may not generalize to all startups or to other ecosystems.

- **Parsing of Founder Information:**  
  The number of founders was extracted from a free-text `Founder/s` column using simple string rules. This can lead to counting errors (for example, cases with 0 founders or unusual formatting), which reduces the accuracy of the `founder_count` feature.

- **Incomplete or Missing Values:**  
  Some startups have missing or unclear funding amounts or stages. These rows were dropped during cleaning, which reduces the sample size and may introduce bias if missingness is not random.

- **Funding Amount Interpretation:**  
  Funding amounts are treated as a single numeric value without distinguishing between different rounds or currencies. Using `log_amount` helps with skewness, but it still mixes potentially different types of funding events.

- **Limited Feature Set:**  
  Important factors such as sector detail, geography, team experience, product type, market conditions, or investor quality are not fully included. This means the models and hypotheses are based on a simplified view of what drives funding.

---

### Future Work

- **Richer and Larger Datasets:**  
  Combine multiple datasets or sources (e.g., other Kaggle datasets or startup databases) to cover more countries, more years, and more startups. This would make the conclusions more robust.

- **Better Founder Features:**  
  Improve the extraction of founder information by using more reliable structured data or more advanced text processing. Future work could also include founder background, education, or prior startup experience rather than just counting the number of founders.

- **More Detailed Funding Data:**  
  Separate different funding rounds (Pre-seed, Seed, Series A, etc.) by date and amount, and analyze the evolution of funding over time for the same startup instead of using a single aggregated number.

- **Advanced Modelling and Validation:**  
  Explore more complex machine learning models (e.g., Random Forests, Gradient Boosting) with proper cross-validation to see whether richer features can significantly improve prediction of funding amounts.

- **External Data Enrichment:**  
  Add external variables such as macroeconomic indicators, market size, or sector-specific trends to better understand how the environment around a startup influences funding outcomes.
