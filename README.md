# Data Analyst Job Market Analysis

## Overview

Welcome to my analysis of the **data job market**, with a focus on **Data Analyst** roles. This project was driven by a desire to better understand and navigate the job landscape in this field. It explores top-paying and in-demand skills to help identify the most valuable opportunities for aspiring data analysts.

The dataset is sourced from **Luke Barousse’s Python Course** and contains detailed information on job titles, salaries, locations, and essential skills. Through Python-based analysis, I dive into key questions such as:

- Which skills are most in demand?
- What are the salary trends?
- How do demand and salary overlap for data analytics roles?

---

## ❓ The Questions

Here are the main questions I aim to answer in this project:

- What are the most in-demand skills for the top 3 most common data roles?
- How are in-demand skills trending for Data Analysts?
- How well do jobs and skills pay for Data Analysts?
- What are the optimal skills for Data Analysts to learn? (High Demand **and** High Paying)

---

## 🛠️ Tools & Technologies

To perform this analysis, I used the following tools:

### Programming Language
- **Python** — The core language for data manipulation and analysis.

### Python Libraries
- **Pandas** — For data cleaning, transformation, and analysis.
- **Matplotlib** — For creating basic visualizations.
- **Seaborn** — For advanced data visualizations and styling.

### Development Tools
- **Jupyter Notebooks** — For writing, documenting, and executing analysis.
- **Visual Studio Code** — My preferred environment for developing Python scripts.
- **Git & GitHub** — For version control and sharing the project with others.

---
## Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

---

### Import & Clean Up Data

I begin by importing the necessary libraries and loading the dataset, followed by essential cleaning tasks to improve data quality and consistency.

#### Code Snippet

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
---

### Filter US Jobs
To focus the analysis on the U.S. job market, I apply a filter to include only roles based in the United States.

```python
  df_US = df[df['job_country'] == 'United States']
```
---
## The Analysis

Each Jupyter notebook in this project focuses on a specific question about the data job market. Here's how I approached each one:

---

### 1. What are the most in-demand skills for the top 3 most popular data roles?

To identify the top skills for the most common data roles:

- I filtered the dataset to find the **top 3 most frequent job titles**.
- Then, I calculated the **top 5 skills** associated with each of these roles.
- This analysis helps pinpoint the most essential skills to focus on based on your target role.

**View the full notebook** for this analysis: [2_Skills_Count.ipynb](2_Project/2_Skills_Count.ipynb)

---

### Visualizing the Data

The following code snippet creates horizontal bar charts to show the **top 5 skills** for each of the top 3 roles:

```python
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(
        data=df_plot, 
        x='skill_percent', 
        y='job_skills', 
        ax=ax[i], 
        hue='skill_count', 
        palette='dark:b_r'
    )

plt.tight_layout()
plt.show()
```
### Results

![(2_Project/Images/Skill_demand.png)](https://github.com/JavadovSaid/Python_for_Data_Analysis/blob/main/2_Project/Images/Skill_demand.png)
---
### Insights:
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).
