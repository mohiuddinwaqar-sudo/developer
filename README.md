#The Analysis 

## What are the most in demand job skills for the top 3 most popular data jobs 

For this part I wanted to explore the skillsets that are most desired in the top data jobs. I observed the United Kingdom as it has a similar context to Australia!

For my analysis I utilised various tools. I created a new dataframe of the original that focused on the UK and specific job titles. Following this I highlighted the specific job skills and their counts for each job. Finally I visualised this data using seaborn. Although you can use matplotlib, I prefer seaborn due to its ease and aesthetics!

View my notebook for detailed steps here:  
[2.Skills_Count](2_ADVANCED/1_BASIC/3.Project/skills_count.ipynb)

## Visualise Data (seaborn)
```python
fig, ax = plt.subplots(len(job_titles), 1, figsize=(10, 15))

for i, job_title in enumerate(job_titles):
    subset = UK_skill_percentages[UK_skill_percentages['job_title_short'] == job_title]
    subset = subset.sort_values(by='skill_counts', ascending=False).head(10)
    #used seaborn for better visualization
    sns.barplot(data=subset, x='skill_percentage', y='job_skills', ax=ax[i], palette='viridis')
    ax[i].set_title(f'Top 10 Skills for {job_title}', weight='bold')
    ax[i].set_xlim(0, 80)
    ax[i].set_xlabel(' ')
    ax[i].set_ylabel(' ')
    ax[i].set_yticklabels(ax[i].get_yticklabels(), fontstyle='italic')
    plt.suptitle('In Demand Skills by Percentage for Top 3 Job Titles in the UK Job Market', fontsize=16, fontweight='bold')

    for n, v in enumerate(subset['skill_percentage']):
        ax[i].text(v + 1, n, f"{v:.0f}%", color='black', va='center', fontweight='bold')
```
## Results
![Visualisation of Top Skills for Data Jobs in the UK](SKILL_DEMAND_UK.png)


### Insights 

1. In the UK, SQL 👾 and Python 🐍 are in high demand for all top data jobs.

2. Data Scientists place more emphasis on python, likely due to python's superior capability in drawing insights and various other coding techniques.

3. As pay goes up from data analyst, engineer and scientist, there is a greater demand for advanced coding skills such as python and R. 

## How are job skills trending for Data Science jobs in the UK 

## Visualisation 

``` python 
#using seaborn to plot the trends


sns.lineplot(data=data_jobs_percent_UK, dashes=False, markers=True, marker='o', markersize=8, palette='tab10')
# Customizing the plot
plt.title('Trends in Demand for Data Science Skills in the UK (by % of Job Postings)', fontsize=14, fontweight='bold', pad=20)
plt.xlabel(' ')
# Mapping month numbers to month names
plt.xticks(ticks=range(1, 13), labels=[calendar.month_name[i] for i in range(1, 13)], rotation=45)

#adding y label percentage of job postings
from matplotlib.ticker import FuncFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, _: '{:.0f}%'.format(y)))

plt.ylabel('Percentage of Job Postings (%)')
plt.legend(title='Skills', bbox_to_anchor=(1.05, 1), loc='upper left', fontsize=10, title_fontsize=12, borderaxespad=0.)
plt.tight_layout()
```
## Results

![Visualisation of Skill Demands for Data Jobs postings in the UK](output.png) 

- Python remains stable throughout the year and above skills like sql and R. 

## Differences between median salaries of the top 3 Data Roles 

``` python 
order = medians.index.tolist()

combined_median = df_UK_top_jobs["salary_year_avg"].median()

sns.boxplot( 
    data=df_UK_top_jobs,
    x="salary_year_avg",
    y="job_title_short",
    order= order,
    palette="Set2"  
)

#Calculate medians
medians = (
    df_UK_top_jobs.groupby("job_title_short")["salary_year_avg"]
    .median()
    .reindex(job_titles)
    .sort_values(ascending=False)
    
)

# Add labels
for i, (job, median) in enumerate(medians.items()):
    plt.text(
        median + 1000,        # move label a bit to the right
        i,                    # vertical position
        f"${int(median/1000)}K",
        va="center",
        fontweight="bold",
        color="black"
    )

# Format x-axis labels as $XXK
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}K"))

plt.title("Top Data Jobs in UK", fontsize= 20)

# Add combined median line
plt.axvline(
    combined_median,
    color="red",
    linestyle="--",
    linewidth=2,
    label=f"Combined Median (${int(combined_median/1000)}K)"
)
# Add legend for the reference lines
plt.legend(
    title="Reference Lines",
    bbox_to_anchor=(1.02, 1),
    loc="upper left",
    borderaxespad=0
)
plt.xlabel("Salary", fontsize = 12)
plt.ylabel(" ")
plt.show()

```

## Results 

![salary_analysis](Boxplot.png)

- Data engineer leads the charge when discussing median salaries. This is likely due to a greater demand of relevant experience to become a data engineer. 

- The median across all three roles combined was $97,000 USD. Both Data Scientists and Engineers had higher medians than the combined median, however, data analyst did not. 

- It is highly likely data analyst roles have a lower median due to years of experience and skillset. As previously shown above, there is less emphasis on data analysts to learn advanced coding languages such as python and r. These languages command a higher salary for data roles. 
