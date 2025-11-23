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
Understanding team structures in early‑stage ventures is valuable for both entrepreneurs and investors.  
Questions we address include:  
- What is the typical number of founders in startups?  
- Do startups with more founders tend to raise more funding? 
- Is there an industry‑specific pattern in founder count and funding stage?

---

## Datasets

### Dataset 1: Startup Founders Dataset
* **Name:** Startup Founders Dataset  
* **Source:** Wellfound (https://wellfound.com)  
* **Link:** Collected via custom web scraper; not publicly hosted  
* **Data Acquisition Method:** Gathered from startup pages on Wellfound using web scraping, focusing on how many founders found the start-up. The data was exported into a CSV file (`founders.csv`) for analysis.  
---
### Dataset 2: Startup Funding Stage 
* **Name:** Startup Funding Dataset  
* **Source:** Wellfound (https://wellfound.com)  
* **Link:** Collected via custom web scraper; not publicly hosted  
* **Data Acquisition Method:** Gathered from startup pages on Wellfound using web scraping, focusing on funding stages (Seed, Series A, etc.) and total funding (if listed). Saved as `funding.csv`.  
---
### Dataset 3: Sector or industry of the Startup
* **Name:** Startup Industry Dataset  
* **Source:** Wellfound (https://wellfound.com)  
* **Link:** Collected via custom web scraper; not publicly hosted  
* **Data Acquisition Method:** Categorized each startup by its primary industry (e.g., AI, FinTech, HealthTech, EdTech) based on its Wellfound description using web scraping. Saved as `industry.csv`.  


### Key Columns for Each CSV

#### 1. `founders.csv`
| Column Name     | Description                              |
|-----------------|------------------------------------------|
| `startup_name`  | Name of the startup                       |
| `num_founders`  | Number of founding individuals           |
| `founded_year`  | Year the startup was founded             |

#### 2. `funding.csv`
| Column Name     | Description                              |
|-----------------|------------------------------------------|
| `startup_name`  | Name of the startup                       |
| `funding_stage` | Current funding stage (Seed, Series A, etc.) |
| `funding_amount`| Total funding raised (numeric, if available) |

#### 3. `industry.csv`
| Column Name     | Description                              |
|-----------------|------------------------------------------|
| `startup_name`  | Name of the startup                       |
| `industry`      | Sector or industry of the startup (e.g., AI, FinTech, HealthTech, EdTech) |

---

## Methods
### 1. Data Collection 

For this project, I will collect data by web scraping publicly available startup profiles from Wellfound.
Using Python (with libraries such as requests and BeautifulSoup or Scrapy), I will extract basic information about startups, including:
- Number of founders
- Total funding amount 
- Funding stage
- Industry

#### Scraping Steps

- Gather a list of startup profile URLs from Wellfound’s search or listing pages.
- Write a Python scraper to visit each profile and collect the data fields above.
- Store the results in a CSV file for later analysis.

##### Important Note

- I will follow polite scraping practices (slow request rate, no heavy crawling).
- I will only scrape publicly accessible information.
- I will check and respect Wellfound’s Terms of Service when collecting data.

### 2. Data Cleaning

After scraping:
- Remove duplicates or incomplete entries
- Convert funding amounts to numeric form
- Standardize categories (e.g., industry names, funding stages)
- Calculate new variables such as, founder group (solo, two founders, more than two)

### 3. Exploratory Data Analysis (EDA) Pipeline

Methods Used

Summary statistics (mean, median, counts)

Visualizations (bar charts, boxplots)

Simple statistical tests (e.g., correlations)

#### 3.1 Descriptive Statistics

- Summary statistics for numeric variables (num_founders, funding_amount, startup_age).

- Frequency table for categorical variables (funding_stage, industry).

- Cross-tabulation: num_founders × funding_stage.

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

#### 3.5 Reproducibility

- Implement the full EDA in a Jupyter Notebook (eda.ipynb).

- Use modular code blocks:

  data_cleaning.py

  eda_visuals.py

  stat_tests.py

- Export all figures automatically to a /figures directory.

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
- **Cross-Sectional Nature:** The dataset represents a single point in time, making it difficult to infer causal relationships between founder count and startup success.  
- **Uncontrolled Factors:** Other variables such as team experience, product type, market conditions, or geographic region were not included but may influence funding outcomes.  
- **Approximation in Funding Amounts:** Inconsistent currency formats and missing values required normalization and estimation.

write this-> in the industry part I too generalized, i.e. wrote fintech for both fintech elec market etc. ALSO since I collect manually it can e minor mistakes
---

### Future Work
- **Expand Dataset:** Collect a larger and more diverse dataset, including international startups and additional success metrics (e.g., revenue, exit status).  
- **Automate Data Collection:** Implement web-scraping or API-based collection from platforms like Crunchbase or PitchBook for greater scalability.  
- **Longitudinal Analysis:** Track startups over multiple years to observe how founder composition affects long-term performance.  
- **Advanced Modeling:** Apply regression or machine learning models to predict funding success based on founder count and industry.  
- **Broader Variables:** Incorporate qualitative factors such as founder background, education, and experience for a deeper understanding of team dynamics.
