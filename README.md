# Introduction
This project analyzes a dataset of data analyst job postings to identify:
- 🔝 The most in-demand technical skills
- 💰 The highest average salaries by skill
- 🌐 Remote-first trends
- 📈 Recommendations on the most optimal skills to learn for career growth
# Background
## Tools I used
- **Visual Studio Code** — used for writing, testing, and managing SQL queries
- **PostgreSQL** — backend database for querying job and skills data
- **Git + GitHub** — version control and project sharing
-  ## The Analysis
-  Each query was examined to ubcover jey aspects within the data analyst job market.
## 🗂️ Data Schema
- The project uses four key tables:
- `job_postings_fact`: Main job data (location, salary, remote status)
- `skills_dim`: Unique list of skills
- `skills_job_dim`: Many-to-many join table between jobs and skills
- `company_dim`: Company details
  
 ### Top Paying Data Analyst Jobs (Any Location)
- ```sql
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
#### Key Insights
##### Three highest paying Data Analyst Jobs:
- 1. Data Analyst	Mantys	650,000
- 2. Director of Analytics	Meta	336,500
- 3. Associate Director - Data Insights	AT&T	255,829
- The Mantys listing is an extreme outlier at $650K — possibly equity-heavy or a founder-type role misclassified as a standard "Data Analyst".
- The top 10 highest paying Data Analyst salaries ranged from  $180K–$650K and includes titles like Director, Principal, and Marketing Analysts — all with senior or leadership slants.
**🔍 Insight:**
- 6 out of 10 roles include “Director” or “Principal” — seniority strongly correlates with salary.
- Even plain “Data Analyst” roles (Mantys, Pinterest) can reach high pay when tied to marketing, product, or startup equity.
 **🌍 All Roles Are Fully Remote ("Anywhere")**
- Every role listed is labeled as “Anywhere” — signaling high flexibility in remote hiring for top-tier analysts.
- Companies range from tech giants (Meta) to startups (Mantys, SmartAsset) and public-sector-like orgs (UCLA Health).
**📌 Key Takeaways**
- To maximize salary potential, targeting titles like Principal, Director, or Marketing Analyst are the best options
- Focusing on companies where data plays a core business role are also the best options — like Meta, SmartAsset, or digital-first firms
- Even top-paying roles don’t require relocation but these roles likely involve strategic, technical, and cross-functional ownership

### 🔝 Top 10 Most Common Skills from the Top Data Analyst Roles
``` sql
    WITH top_paying_jobs AS (
        SELECT 
            job_id,
            job_title,
            salary_year_avg,
            name AS company_name
            FROM job_postings_fact
            LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
            WHERE job_title_short = 'Data Analyst' AND job_location = 'Anywhere'
            AND salary_year_avg IS NOT NULL
            ORDER BY salary_year_avg DESC
            LIMIT 10
    )
    SELECT top_paying_jobs.*,
    skills
    FROM top_paying_jobs
    INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
 ```
-- **Skill Frequency***
![image](https://github.com/user-attachments/assets/deca5afd-751a-45e7-b060-197557c8e724)
--  SQL and Python are the most in-demand skills, appearing in nearly every high-paying role. Visualization (Tableau) and statistical tools (R) also rank highly.

--**📊 Jobs with the Most Diverse Skill Requirements**
-- ![image](https://github.com/user-attachments/assets/bf9e9f33-0f9d-46f5-bf1b-646aa0db0189)
-- Insight: Senior-level roles such as Director and Principal Analyst typically demand a wider range of tools and technologies, reflecting their broader responsibilities.
/*
- ### Top skills in Demand
- 
### 📈 Key Insights
 -- 💼 Top Paying Skills — Niche & Specialized Technologies Dominate
# 💼 Data Skills & Salary Report (2025)

An analysis of the top-paying tech skills based on average salaries, categorized by specialization. This includes niche tools, programming languages, cloud platforms, data science tools, and enterprise workflows.

---

## 💼 Top Paying Skills — Niche & Specialized Technologies Dominate

| Skill                  | Avg Salary ($) | Insight                                                                                   |
|------------------------|----------------|--------------------------------------------------------------------------------------------|
| Neo4j, Elasticsearch   | 185,000        | Graph DBs and search engines—rare but extremely valuable when needed.                     |
| Cassandra              | 175,000        | Distributed NoSQL DB for high-scale, real-time data applications.                         |

> 💡 **Insight:** Niche, high-demand skills in scalable or unstructured data management pay off significantly.

---

## 🧰 Programming & Scripting Languages

| Language           | Avg Salary ($) |
|--------------------|----------------|
| Perl               | 157,000        |
| C                  | 146,500        |
| Java               | 125,146        |
| C++                | 124,044        |
| Python             | 110,396        |
| Shell, PowerShell  | ~100K–126K     |

> 💡 **Insight:** More systems-level or backend-oriented languages (like C, Perl, Java) bring higher compensation than Python—likely due to legacy system work or performance-critical infrastructure.

---

## ☁️ Cloud and Big Data Tools

| Skill       | Avg Salary ($) |
|-------------|----------------|
| GCP         | 135,294        |
| Azure       | 122,692        |
| Snowflake   | 119,577        |
| BigQuery    | 107,438        |
| AWS         | 106,888        |
| Databricks  | 103,500        |

> 💡 **Insight:** All part of the modern cloud-based analytics stack. GCP surprisingly edges out AWS and Azure, likely due to a lower supply of GCP-proficient professionals.

---

## 📈 Data Science & Visualization Tools

| Tool               | Avg Salary ($) |
|--------------------|----------------|
| Scikit-learn       | 130,000        |
| Pandas, NumPy      | ~125,000       |
| Jupyter            | 107,562        |
| Looker, Qlik       | ~110K–120K     |
| Plotly, Tableau    | ~100K–122K     |

> 💡 **Insight:** Python-based libraries like Scikit-learn and Pandas command strong salaries. Visualization tools are valuable but most lucrative when paired with modeling or engineering skills.

---

## 🔐 Enterprise & Workflow Tools

| Skill              | Avg Salary ($) |
|--------------------|----------------|
| GDPR               | 117,750        |
| Git, GitHub        | ~102K–124K     |
| Jira, Confluence   | ~100K–107K     |

> 💡 **Insight:** Version control and compliance expertise (e.g., GDPR) are increasingly valued, especially in senior or regulatory-facing roles.

---

## 🧠 Summary of Takeaways

- 🧠 **Niche tools pay more**: Less common tools like Neo4j, Cassandra, and Elasticsearch dominate the top of the pay scale.
- ☁️ **Cloud and modern data stacks** (GCP, Snowflake, Airflow) are well-compensated.
- 📊 **Deep data science skills** (Pandas, Scikit-learn, NumPy) fetch strong salaries, especially when combined with cloud or production tools.
- 💻 **Low-level programming** (C, Perl) still pays surprisingly well due to legacy system and infrastructure demand.

---

📌 _Source: Compiled from industry salary benchmarks and job market insights (2025)._


--🧠 **Summary of Takeaways**
--🧠 Niche tools pay more: Less common tools like Neo4j, Cassandra, and Elasticsearch dominate the top of the pay scale.

--💼 Cloud and modern data stacks (GCP, Snowflake, Airflow) are well-compensated.

--🔍 Deep data science skills (Pandas, Scikit-learn, Numpy) fetch strong salaries, especially when combined with cloud or production tools.

--🧱 Low-level programming (C, Perl) still pays surprisingly well, likely due to legacy systems or specialized infrastructure needs.
- ✅ Snowflake, Azure, and AWS combine high demand with excellent salary potential — optimal for modern data workflows.
- 🧠 Python and Tableau remain the most common tools in remote roles, offering wide applicability.
- 💼 Niche tools like Go, Hadoop, and BigQuery offer high salaries but appear less frequently.

### Skill Recommendations

## What I learned
### Conclusions
- 
