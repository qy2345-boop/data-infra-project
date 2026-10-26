# Lab 1

**Name:** Qiaochu (Poppy) Yang
**Due:** Sunday, October 25 at 11:59 PM
**Submit:** your repo URL in the Lab 1 assignment on CourseWorks

| Part | Where | File |
|---|---|---|
| **In class:** first DuckDB query on the penguins data | Session 1 | `notebooks/lab1_first_query.ipynb` |
| **At home:** two related tables of your own in DuckDB | Before Sunday | `notebooks/lab1_home_your_data.ipynb` |
| **At home:** lifecycle map of your data | Before Sunday | This file (below) |

---

## Lifecycle map of your data

In class we named five stages every data system has. Trace them for the two tables you loaded at home. Some rows ask what you did this week; the last two ask you to look ahead to the system you'll build by December. Educated guesses are fine; say where you're unsure.

**My tables:** Uber Eats USA Restaurants + Zillow Observed Rent Index (ZORI)

| Stage | For your data | Your answer |
|---|---|---|
| **1. Ingestion** | I downloaded the Uber Eats restaurant dataset from Kaggle and the Zillow rental dataset from Zillow Research as CSV files. If their column names, formats, or structures change, the existing SQL queries and cleaning process may stop working. | |
| **2. Validation** | I used DESCRIBE and SUMMARIZE to check data types and missing values. The restaurant data contained inconsistent ZIP code formats, invalid coordinates, and potential duplicates. The key check found 1,584 matching ZIP codes before the final correction. I would check ZIP code validity and geographic consistency whenever new data arrives. | |
| **3. Transformation** | I standardized ZIP codes into five-digit formats and removed records with unreliable ZIP codes or zero coordinates. I also identified 83 pairs of potentially duplicate restaurants. To compare restaurants with rental costs, I would join the two datasets by ZIP code and account for differences in geographic coverage. | |
| **4. Scheduling** | The Uber Eats dataset publisher previously reported updates about every 7–10 days, although its current schedule is uncertain. Zillow releases updated rental data monthly. For a future application, I would refresh restaurant records when new snapshots become available and update rental data monthly. | |
| **5. Interface** | Researchers or data analysts could use an interface to examine the quality of collected datasets. They might ask: "How reliable is this dataset, and what issues should I address before using it?" A useful interface would show missing values, inconsistent formats, potential duplicates, and geographic coverage gaps. | |

**Which stage feels most fragile for your data, and why?** Ingestion feels most fragile because some problems originate from the way the data was collected. While formatting issues and duplicates can often be corrected through cleaning, uneven geographic coverage cannot be fully resolved without collecting additional data. This makes it important to assess the limitations of the original source before analysis.

---

## Checklist before you submit

- [ ] In-class notebook: every cell ran, including your own query and three observations
- [ ] At-home notebook: both CSVs in `data/`, both tables loaded and checked, the key relationship described, five queries each with a one-line note, three observations, two dataset cards
- [ ] This lifecycle map filled in, plus the "most fragile" answer
- [ ] Every notebook saved (Cmd+S), then committed and pushed
- [ ] Repo URL pasted into CourseWorks
