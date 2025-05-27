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

## 🔄 2. How are in-demand skills trending for Data Analysts?

To understand skill trends in 2023 for Data Analysts, I:

- Filtered the dataset to include only **Data Analyst** positions.
- Grouped the job postings by month.
- Identified the **top 5 skills per month**, revealing how their popularity evolved throughout the year.

**View the full notebook** with detailed analysis here: [3_Skills_Trend.ipynb](2_Project/3_Skills_Trend.ipynb)

---

### 📈 Visualize Data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```
### Results
![Trending Top Skills for Data Analysts in the US](
https://github.com/JavadovSaid/Python_for_Data_Analysis/blob/main/2_Project/Images/Trending_Top_Skills.png)

### 📌 Insights

- **SQL** remained the most in-demand skill across the year, though there was a **slight decline** in its relative frequency toward the end of 2023.
- **Excel** saw a **sharp rise in demand starting around September**, eventually surpassing both Python and Tableau by December.
- **Python** and **Tableau** maintained relatively **steady demand**, experiencing minor fluctuations but consistently ranking as essential skills.
- **Power BI** was the least demanded among the top 5 but showed a **gradual increase** in popularity toward the end of the year.

---

## 3. How well do jobs and skills pay for Data Analysts?
To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [4_Salary_Analysis_.ipynb](2_Project/4_Salary_Analysis.ipynb)
### Visualization

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results
![Salary_Distribution](https://github.com/JavadovSaid/Python_for_Data_Analysis/blob/main/2_Project/Images/Salary_Distribution.png)

Box plot visualizing the salary distributions for the top 6 data job titles.

### Insights
- There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

---

## Highest Paid & Most Demanded Skills for Data Analysts
Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

### Visualize Data
```python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```
### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the US: 
![Salary Data Analysis](https://github.com/JavadovSaid/Python_for_Data_Analysis/blob/main/2_Project/Images/Salary_Analysis.png)

### Insights
- There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

---

## 4. What Are the Most Optimal Skills to Learn for Data Analysts?
To pinpoint the best skills to focus on — those that combine the highest pay with the greatest demand — I calculated both the percentage of skill demand and the median salary for each skill. This approach helps clearly highlight the most valuable skills for data analysts to develop.

View my notebook with detailed steps here:  [5_Optimal_Skills.ipynb](2_Project/5_Optimal_Skills.ipynb).

### Visualize Data
```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```
### Results
![Simple_Optimal_Skills](https://github.com/JavadovSaid/Python_for_Data_Analysis/blob/main/2_Project/Images/Simple_Optimal_Skills.png)
A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.

### Insights
- Oracle commands the highest median salary—around $97K—despite its relatively low frequency in job postings. This highlights the premium placed on specialized database expertise within the data analyst field.

- Widely demanded skills like Excel and SQL appear frequently in listings but offer lower median salaries compared to more specialized tools such as Python and Tableau, which enjoy both higher salaries and moderate demand.

- Skills like Python, Tableau, and SQL Server strike a balance between high salary potential and common demand, suggesting that mastering these tools can open doors to well-paying roles in data analytics.

## Visualizing Different Techonologies
Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

### Visualize Data
```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()
```

### Results
![Optimal_Skills](https://github.com/JavadovSaid/Python_for_Data_Analysis/blob/main/2_Project/Images/Optimal_Skills.png)

A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color-coded labels representing technology categories.

### Insights

- **Programming Skills (blue)** tend to cluster at higher salary levels, indicating that programming expertise often leads to greater salary benefits within data analytics.
- **Database Skills (orange)** such as Oracle and SQL Server are linked to some of the highest salaries, showing strong demand and high valuation for data management expertise.
- **Analyst Tools (green)** including Tableau and Power BI are frequently required in job listings and offer competitive salaries, highlighting the importance of visualization and analysis software across data roles.

---

## What I Learned

Throughout this project, I enhanced my technical and domain knowledge:

- **Advanced Python Usage:** Utilized libraries like Pandas for data manipulation, Seaborn and Matplotlib for visualization, enabling complex and efficient analysis.
- **Data Cleaning Importance:** Learned that thorough cleaning and preparation are essential to ensure accurate insights.
- **Strategic Skill Analysis:** Understanding the relationship between skill demand, salary, and job availability is key to effective career planning in tech.

---

## General Insights

- **Skill Demand and Salary Correlation:** High-demand skills often correlate with higher salaries. Specialized skills like Python and Oracle command premium pay.
- **Market Trends:** Skill demand is dynamic, requiring continuous monitoring to stay current and competitive.
- **Economic Value of Skills:** Identifying skills that are both highly demanded and well-compensated helps prioritize learning efforts for maximum career impact.

---

## Challenges I Faced

- **Data Inconsistencies:** Managing missing or inconsistent data required careful cleaning to maintain analysis integrity.
- **Complex Data Visualization:** Crafting clear and informative visuals for complex datasets was challenging but necessary to convey findings effectively.
- **Balancing Breadth and Depth:** Maintaining a comprehensive overview while diving deep into specific analyses demanded careful focus and planning.

---

## Conclusion

This project provided valuable insights into the data analyst job market, revealing the critical skills and trends shaping the profession. The experience deepened my understanding of data analytics and improved my Python skills, especially in data manipulation and visualization.

As the data job market evolves, continuous learning and analysis are crucial to staying competitive. This project serves as a solid foundation for future exploration and underscores the importance of adaptability and ongoing skill development in the field.

---

