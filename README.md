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

As a data science student interested in entrepreneurship, I wanted to examine these ideas using real startup data. Understanding these relationships can help founders and investors develop more realistic expectations about funding, and can serve as a small, data-driven case study of how team structure and growth stage interact with investment outcomes.

---

## Datasets

### Dataset 1: Startup Founders and Funding (Raw)
* **Name:** Startup Founders and Funding  
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

For this project, I will collect data manually publicly available startup profiles from Wellfound.
I will extract basic information about startups, including:
- Number of founders
- Total funding amount 
- Funding stage
- Industry


### 2. Data Cleaning

- Remove duplicates or incomplete entries
- Convert funding amounts to numeric form
- Standardize categories (e.g., industry names, funding stages)
- Calculate new variables such as, founder group (solo, two founders, more than two)

### 3. Exploratory Data Analysis (EDA) Pipeline

Methods Used

Summary statistics (mean, median, counts)

Visualizations (bar charts, boxplots)

Simple statistical tests (e.g., correlations)


#### 3.2 Visualization Suite

| Goal                                             | Visualization Type                      | Tool/Library            |
| ------------------------------------------------ | --------------------------------------- | ----------------------- |
| Founder count distribution                       | Bar plot / Histogram                    | `matplotlib`, `seaborn` |
| Funding stage proportions                        | Pie chart / Count plot                  | `seaborn`               |
| Relationship between founders and funding amount | Boxplot                                 | `seaborn`               |
| Correlation heatmap                              | Heatmap (funding_amount, founders, age) | `seaborn`               |
| Industry vs founder count                        | Bar chart                               | `pandas`, `matplotlib`  |
| Funding stage vs age                             | Boxplot                                 | `seaborn`               |

#### 3.3 Statistical Testing

- ANOVA / Kruskal-Wallis Test: To test if funding amounts differ significantly between groups with different founder counts.

- Chi-Square Test: To test if num_founders and funding_stage are independent.

- Correlation Analysis: Between num_founders and funding_amount.

#### 3.4 Feature Engineering 

- Encode categorical variables (funding_stage, industry) numerically.

- Create founder_group categories: Solo, Two-Founder, Team (>2).

---
## Analysis Plan

1. **Exploratory Data Analysis (EDA)**  
   - Displayed summary statistics and distributions for all core features (`num_founders`, `funding_amount`, `startup_age`).  
   - Used `.describe()` and `.skew()` to assess spread, central tendency, and data symmetry.  
   - Generated a correlation matrix and visualized it with a heatmap.  
   - Created the following plots:  
     - Bar plots for founder count distribution.  
     - Boxplots showing funding amount across different founder counts.  
     - Count plots for funding stages.  
     - Scatter plots between number of founders and total funding.  
     - Histograms for startup age and funding distributions.  

   **Key Insights:**  
   - Most startups are founded by 2 people.  
   - Two-founder startups tend to appear more frequently in higher funding stages (e.g., Series A/B).  
   - Weak positive correlation between number of founders and funding amount.  
   - Certain industries (like FinTech and AI) show higher funding levels regardless of founder count.

---

2. **Hypothesis Testing**

   **Hypotheses:**  
   - **H₀ (Null):** There is no statistically significant relationship between the number of founders and the funding stage of a startup.  
   - **H₁ (Alternative):** There is a statistically significant relationship between the number of founders and the funding stage of a startup.

   **Method:**  
   - Used the **Chi-Square Test of Independence** to evaluate whether `num_founders` and `funding_stage` are related.  
   - Additionally, computed **ANOVA** on `funding_amount` across founder groups (Solo, Two-Founders, Team > 2).  

   **Results:**  
   - *Chi-Square p-value ≈ 0.018* → Reject the null hypothesis at α = 0.05.  
   - *ANOVA F-statistic significant (p < 0.05)* → Founders count influences funding amount.  
   - Interpretation: Startups with **two founders** are statistically more likely to reach higher funding stages than solo or larger teams.
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
