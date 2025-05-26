# Introducton
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
- ```sql
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

- ### 
### 📈 Key Insights
- ✅ Snowflake, Azure, and AWS combine high demand with excellent salary potential — optimal for modern data workflows.
- 🧠 Python and Tableau remain the most common tools in remote roles, offering wide applicability.
- 💼 Niche tools like Go, Hadoop, and BigQuery offer high salaries but appear less frequently.

### Skill Recommendations
Goal	Recommended Skills
Enter Data Analytics	Python, SQL, Tableau
Target Data Engineering Roles	Snowflake, AWS, BigQuery, SSIS
Maximize Remote Job Pay	Azure, Go, Hadoop
Enterprise Analytics	SAS, SQL Server, Oracle
## What I learned
### Conclusions
- 
