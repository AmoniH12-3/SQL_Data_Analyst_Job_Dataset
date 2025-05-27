# Introduction
This project analyzes a 2023 dataset of data analyst job postings to identify:
- 🔝 The most in-demand technical skills
- 💰The highest paying Data Analyst roles
- 💰The Highest Paying Technical Skills (Based on Avg Salary)
- 📈 Recommendations on the most optimal skills to learn for career growth
# Background
## Tools I used
- **Visual Studio Code** — used for writing, testing, and managing SQL queries
- **PostgreSQL** — backend database for querying job and skills data
- **Git + GitHub** — version control and project sharing
-  ## The Analysis
-  Each query was examined to uncover the aspects within the data analyst job market.
## 🗂️ Data Schema
- The project uses four key tables:
- `job_postings_fact`: Main job data (location, salary, remote status)
- `skills_dim`: Unique list of skills
- `skills_job_dim`: Many-to-many join table between jobs and skills
- `company_dim`: Company details

## 💰 Top Paying Jobs in Data Analytics (Any Location)

| Role                                 | Company          | Salary (USD) |
|--------------------------------------|------------------|--------------|
| Data Analyst                         | Mantys           | $650,000     |
| Director of Analytics                | Meta             | $336,500     |
| Associate Director - Data Insights   | AT&T             | $255,829     |
| Data Analyst, Marketing              | Pinterest        | $232,423     |
| Principal Data Analyst               | SmartAsset       | $205,000     |

``` sql
SELECT 
   job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
    FROM job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE job_title_short = 'Data Analyst' AND job_location = 'Anywhere'
    AND salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 10;
```
### Number of Unique skills required by Top paying Data Analyst jobs (2023)
![image](https://github.com/user-attachments/assets/95b23648-171c-4962-9b7f-5c72560878f3)

### 10 Most Common Skills for the highest paid Data Analyst jobs (2023)
![image](https://github.com/user-attachments/assets/15507e8b-1841-4e5e-8dd5-baeae9b469de)

### 💡 Insights:

- Seniority significantly boosts pay—**Director-level roles** often exceed **$200K**.
- Companies like **Meta, AT&T, Pinterest, and SmartAsset** offered competitive compensation for data leadership roles.
- Even non-director positions can cross the $200K threshold depending on the stack and industry.

## 🔝 Top 5 In-Demand Core Skills (Based on Demand Count for all Data Analyst jobs)

| Skill     | Demand Count |
|-----------|--------------|
| SQL       | 198          |
| Excel     | 128          |
| Python    | 123          |
| Tableau   | 109          |
| R         | 68           |

#### SQL Code: 
``` sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
    FROM job_postings_fact AS top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE job_title_short = 'Data Analyst'
    AND job_location = 'New York, NY'
    AND salary_year_avg > 60000
    GROUP BY skills
    ORDER BY demand_count DESC
    LIMIT 5;
```

### 💡 Insights:

- **SQL** remains the most in-demand data skill, required for everything from querying databases to backend analysis.
- **Excel** continues to be a must-know tool, particularly in roles involving business operations or legacy systems.
- **Python** is a staple for automation, data manipulation, and analysis.
- **Tableau** shows high demand for data visualization and dashboarding roles.
- **R** is useful for statistics-heavy roles and remains a niche but valuable skill

### 💰 Highest Paying Technical Skills (Based on Avg Salary)

| Skill          | Avg Salary ($) |
|----------------|----------------|
| Neo4j          | 185,000.00     |
| Elasticsearch  | 185,000.00     |
| Cassandra      | 175,000.00     |
| dplyr          | 167,500.00     |
| Unix           | 162,500.00     |
| Perl           | 157,000.00     |
| Twilio         | 150,000.00     |
| Spring         | 147,500.00     |
| C              | 146,500.00     |
| Angular        | 138,516.00     |
| GCP            | 135,293.75     |
| Kafka          | 135,000.00     |
| Pandas         | 133,169.36     |
| Scikit-learn   | 130,000.00     |
| Linux          | 127,500.00     |
| Shell          | 126,250.00     |
| Express        | 126,004.73     |
| Java           | 125,146.88     |
| NumPy          | 125,061.83     |
| C++            | 124,043.75     |
| Git            | 123,750.00     |
| Azure          | 122,692.31     |
| Plotly         | 122,500.00     |
| Airflow        | 122,500.00     |
| Qlik           | 120,762.50     |
| Snowflake      | 119,576.92     |
| GDPR           | 117,750.00     |
| SQL Server     | 114,327.13     |
| C#             | 111,570.83     |
| Looker         | 111,020.44     |
| Python         | 110,395.54     |
| PySpark        | 109,166.67     |
| Jupyter        | 107,561.83     |
| BigQuery       | 107,437.50     |
| Jira           | 107,001.45     |
| AWS            | 106,887.50     |
| Flow           | 106,687.74     |
| Spark          | 105,242.00     |
| SQL            | 104,757.08     |
| Databricks     | 103,500.00     |
| SAP            | 102,760.00     |
| GitHub         | 102,625.00     |
| PowerShell     | 101,250.00     |
| DB2            | 100,833.33     |
| NoSQL          | 100,766.21     |
| Confluence     | 100,586.00     |
| Tableau        | 100,029.62     |
| SSIS           | 100,026.00     |
| MongoDB        | 100,000.00     |
| MySQL          | 99,500.00      |

> 💡 **Insight:** Niche and backend-heavy skills like Neo4j, Cassandra, and GCP command the highest salaries, while foundational tools like SQL and Tableau remain essential but earn slightly lower on average.
``` sql
 SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 2) AS avg_salary
    FROM job_postings_fact AS top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE job_title_short = 'Data Analyst'
    AND job_location ='New York, NY'
    AND salary_year_avg IS NOT NULL
    GROUP BY skills
    ORDER BY avg_salary DESC
    LIMIT 50;
```
### 💡 Insights:

- **SQL and Python** appear across nearly every high-paying listing.
- **Cloud platforms** (Azure, AWS, Snowflake) are major salary boosters.
- Tools like **Pandas, Jupyter, Tableau**, and **Power BI** are expected in modern analyst workflows.
- **R and Excel** still matter and are well-compensated when combined with modern tools.

### 🎯 Optimal Skills to Learn (Based on Demand and Salary):

| Skill        | Demand Count | Avg Salary ($) |
|--------------|---------------|----------------|
| SQL          | 398           | 97,237.16      |
| Excel        | 256           | 87,288.21      |
| Python       | 236           | 101,397.22     |
| Tableau      | 230           | 99,287.65      |
| R            | 148           | 100,498.77     |
| Power BI     | 110           | 97,431.30      |
| SAS          | 63            | 98,902.37      |
| PowerPoint   | 58            | 88,701.09      |
| Looker       | 49            | 103,795.30     |
| Word         | 48            | 82,576.04      |
| Snowflake    | 37            | 112,947.97     |
| Oracle       | 37            | 104,533.70     |
| SQL Server   | 35            | 97,785.73      |
| Azure        | 34            | 111,225.10     |
| Google Sheets| 32            | 86,087.79      |
| AWS          | 32            | 108,317.30     |
| Flow         | 28            | 97,200.00      |
| Go           | 27            | 115,319.89     |
| VBA          | 24            | 88,783.29      |
| SPSS         | 24            | 92,169.68      |
| Hadoop       | 22            | 113,192.57     |
| JavaScript   | 20            | 97,587.00      |
| Jira         | 20            | 104,917.90     |
| SharePoint   | 18            | 81,633.58      |

> 💡 **Tip:** Prioritize high-demand, high-salary tools like **SQL, Python, Tableau, Snowflake, and Azure** to boost both employability and earnings potential.
> *Excel salaries vary widely depending on role, company, and combination with other tools.

## SQL Code
``` sql
WITH skills_demand AS (
    SELECT 
        skills_job_dim.skill_id,
        skills_dim.skills,  -- Get skill name
        COUNT(*) AS demand_count
    FROM job_postings_fact AS jpf
    INNER JOIN skills_job_dim ON jpf.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE jpf.job_title_short = 'Data Analyst'
      AND jpf.job_work_from_home = TRUE  -- Filter for remote jobs
      AND jpf.salary_year_avg IS NOT NULL
    GROUP BY skills_job_dim.skill_id, skills_dim.skills
),
avg_salary AS (
    SELECT
        skills_job_dim.skill_id,
        ROUND(AVG(jpf.salary_year_avg), 2) AS avg_salary
    FROM job_postings_fact AS jpf
    INNER JOIN skills_job_dim ON jpf.job_id = skills_job_dim.job_id
    WHERE jpf.job_title_short = 'Data Analyst'
      AND jpf.job_work_from_home = TRUE
      AND jpf.salary_year_avg IS NOT NULL
    GROUP BY skills_job_dim.skill_id
)
SELECT
    sd.skill_id,
    sd.skills,
    sd.demand_count,
    avg_salary.avg_salary
FROM skills_demand sd
JOIN avg_salary ON sd.skill_id = avg_salary.skill_id
WHERE demand_count > 10  -- Filter for skills with significant demand
ORDER BY demand_count DESC
LIMIT 25;
```

## What I learned
- **SQL + Python + Tableau + Excel** = the golden combo for job opportunities and competitive salaries.
- Cloud and big data skills (e.g., **Snowflake, Azure, AWS**) are in high demand for senior roles.
- **Director** and **Principal Analyst** roles commonly pay above **$200K** and require both technical depth and business communication.
### Conclusions
- The data makes it clear: success in data analytics hinges on both foundational skills and alignment with industry demand.
- **SQL, Python, Excel, and Tableau** continue to be the essentials of a strong data analytics toolkit, as they are highly demanded across entry-level to director-level roles.
- Cloud tools like **Snowflake, AWS, Azure**, and **BigQuery** are becoming essential, especially for mid-to-senior-level analysts.
-  **SmartAsset, AT&T, and Pinterest** are offering salaries exceeding **$200K** for analysts with a production-ready skill set.
- **Specialized tools** like R, Pandas, Jupyter, and Databricks retain value when paired with strong business acumen or statistical modeling.

