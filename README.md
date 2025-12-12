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
- Examined distributions of:
  - `founder_count` (histograms, value counts),
  - `Amount_clean` and `log_amount` (histograms),
  - `Stage` and `Sector` (frequency tables).
- Calculated summary statistics (`.describe()`) for key numeric variables.

**Bivariate EDA**
- **Founder Count vs Funding Amount**
  - Grouped by `founder_count` and computed `count`, `mean`, and `median` funding.
  - Plotted boxplots of `log_amount` by `founder_count` to compare funding distributions across different team sizes.

- **Stage vs Funding Amount**
  - Grouped by `Stage` to obtain counts and median log funding amounts.
  - Plotted boxplots of `log_amount` by `Stage` (restricted to stages with at least 5 observations) to visualize how funding grows across Pre-seed, Seed, Series A–F, etc.

- **Sector vs Funding Amount**
  - Aggregated funding by `Sector` and plotted boxplots for the top 10 sectors by median funding.

- **Founded Year vs Funding Amount**
  - Calculated correlations between `Founded_year` and `Amount_clean`.
  - Created a scatter plot of `Funding Amount vs Founded Year` to check for temporal trends.

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
## Key Insights 

- **Funding Stage vs Funding Amount:**  
  Median log funding increases monotonically from early stages (pre-seed, seed) to later rounds (Series C–F). Because the y-axis is on a log₁₀ scale, differences of 1 unit correspond to roughly tenfold differences in funding, indicating that later-stage rounds are substantially larger.

- **Founder Count vs Funding Amount:**  
  Startups with 1–4 founders show a gradual increase in median log funding as the founder count increases, although variability within each group is large and there are outliers where small founding teams raise very large amounts.

- **Founded Year vs Funding Amount:**  
  The correlation between founding year and funding amount is close to zero, and the scatter plot does not reveal a clear trend. In this dataset, older and younger startups do not systematically differ in the amount of funding they have raised, aside from a few extreme outliers.

- **Sector Effects:**  
  Some sectors (e.g., finance, biotechnology, crypto) exhibit very wide funding ranges, with both small and extremely large deals, whereas others show more homogeneous deal sizes.
  
---

## Timeline
| Task | Deadline |
|------|----------|
|Project Proposal Submission| October 31, 2025|
|EDA & Hypothesis Testing| November 28, 2025|

---
## Limitations and Future Work

### Limitations
- **Manual Data Collection:** The data was manually gathered from Wellfound, which limits sample size and may introduce selection bias (only startups with public profiles are included).  
- **Missing or Incomplete Fields:** Some startups lacked detailed funding or founder information, reducing data completeness.   
- **Uncontrolled Factors:** Other variables such as team experience, product type, market conditions, or geographic region were not included but may influence funding outcomes.  
- **Approximation in Funding Amounts:** Inconsistent currency formats and missing values required normalization and estimation.
- **Generalized Industry Category:** To simplify I added only one field of company.
  
---

### Future Work
- **Expand Dataset:** Collect a larger and more diverse dataset, including international startups and additional success metrics (e.g., revenue, exit status).  
- **Automate Data Collection:** Implement web-scraping or API-based collection from platforms like Crunchbase or PitchBook for greater scalability.  
- **Longitudinal Analysis:** Track startups over multiple years to observe how founder composition affects long-term performance.  
- **Advanced Modeling:** Apply regression or machine learning models to predict funding success based on founder count and industry.  
- **Broader Variables:** Incorporate qualitative factors such as founder background, education, and experience for a deeper understanding of team dynamics.
