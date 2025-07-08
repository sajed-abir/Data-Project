# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skills_Count.ipynb](Project\2_Skills_Count.ipynb)

### Visualize Data
```python
fig, ax = plt.subplots(len(job_titles),1)

sns.set_theme(style='ticks')
for i, job_title in enumerate(job_titles):

    df_plot = (df_skill_perc[df_skill_perc['job_title_short'] == job_title].head(5))

    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

    ax[i].set_title(job_title)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].legend().set_visible(False)
    ax[i].set_xlim(0, 78)

    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v+1, n, f'{v:.0f}%', va='center')

    if i != len(job_titles)-1:
     ax[i].set_xticks([])

fig.suptitle('Percentage(%) of top Skills in Job Postings')
fig.tight_layout(h_pad=0.5)
```
### Results

![Visualization of Top Skills for Data Jobs](Project\Images\top_3_indemand_skills.png)

### Insights

- SQL and Python are the most in-demand skills across all roles.
- Data Analysts often require Excel and Tableau alongside SQL.
- Data Engineers need strong cloud skills (AWS, Azure, Spark).
- Data Scientists heavily rely on Python, with R and SAS also in use.
- Role-specific tools vary, but foundational data skills remain key.


## 2. How are in-demand skills trending for Data Analysts?

### Visualize Data

```python

df_DA_plot = df_DA_us_percent.iloc[:, :5]
sns.lineplot(data=df_DA_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()
plt.title('Trending Top Skills for Data Analyst Role in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax= plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

last_x = df_DA_plot.index[-1]
used_y = []

for i in range(5):
    last_x = df_DA_plot.index[-1]
    used_y = []

    for i in range(5):
        y = df_DA_plot.iloc[-1, i]

        while any(abs(y - used) < 1.5 for used in used_y):
            y += 1  
        used_y.append(y)

        plt.text(11.2, y, df_DA_plot.columns[i], va='center')
```

### Results
![Trending Skills for Data Analyst Roles in the US](Project\Images\trending_skills.png)

### Insights

- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decline in percentage presence.
- Excel experiences a marked increase starting around September, eventually surpassing Python and Tableau by the end of the year.
- Python and Tableau display relatively stable demand with some minor fluctuations, maintaining their importance in data analyst roles.
- Sas, while less prominent compared to the others, exhibits a slight upward trend toward the year’s end.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [4_Salary_Analysis](Project\4_Salary_Analysis.ipynb)

### Visualize Data

```python
sns.boxplot(data=df_us_top5, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distribution in the US')
plt.xlabel('Yearly Salary($USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results
![Salary Analysis for Data Analyst Role](Project\Images\salary.png)

### Insights

- Senior roles (Data Scientist/Engineer) earn significantly more, reaching up to $400K-$600K.
- Data Scientists show slightly higher top-end salaries than Data Engineers.
- Data Analysts have the lowest salary range among the roles.
- Wide IQR for senior roles indicates varied compensation.
- High-earning outliers suggest top performers earn exceptionally more.