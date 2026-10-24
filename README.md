# 🗄️ MySQL Data Cleaning & Analysis

A collection of MySQL projects showing the full SQL workflow: cleaning raw data, exploring it for insights, and practising core to advanced SQL concepts.


---



## Layoffs Data Cleaning

**File:** `layoffs-project/data_cleaning.sql`

Raw layoffs data is cleaned in a **staging table**, so the original table is never modified. The workflow follows three steps.

### 1. Remove duplicates
- Created `layoffs_staging` as a copy of the raw `layoffs` table.
- Used `ROW_NUMBER() OVER (PARTITION BY ...)` across all columns to flag duplicate records.
- Created `layoffs_staging2` with an extra `row_num` column, then deleted every row where `row_num > 1`.

### 2. Standardise the data
- **Industry:** merged variations such as `Crypto%` into a single `Crypto` label.
- **Country:** removed trailing full stops (e.g. `United States.`) using `TRIM(TRAILING '.' FROM country)`.
- **Date:** converted text dates to a proper `DATE` type using `STR_TO_DATE(date, '%m/%d/%Y')` followed by `ALTER TABLE ... MODIFY COLUMN`.

### 3. Handle null and blank values
- Converted blank industry values to `NULL`.
- Used a **self-join** to fill missing industries from other rows of the same company.
- Deleted rows where both `total_laid_off` and `percentage_laid_off` were `NULL`, since they carry no usable information.
- Dropped the helper `row_num` column to finish the clean table.

---

##  Layoffs Exploratory Data Analysis

**File:** `layoffs-project/exploratory_data_analysis.sql`

Analysis performed on the cleaned `layoffs_staging2` table:

| Question | SQL techniques |
|---|---|
| What was the largest single layoff? | `MAX()` |
| Which companies shut down completely (100% laid off)? | `WHERE percentage_laid_off = 1`, `ORDER BY` funds raised |
| Total layoffs by company, industry, and country | `GROUP BY`, `SUM()` |
| Layoffs by year and by month | `YEAR()`, `SUBSTRING()` |
| How did layoffs accumulate over time? | CTE + rolling total with `SUM() OVER (ORDER BY ...)` |
| Which 5 companies laid off the most each year? | Multiple CTEs + `DENSE_RANK() OVER (PARTITION BY year ...)` |

---
##  Skills Demonstrated

- Data cleaning and standardisation in SQL
- Duplicate detection with window functions
- Handling missing values with self-joins
- Date and string manipulation
- Exploratory analysis with aggregations, CTEs, and ranking
- Rolling totals and time-series analysis
- Stored procedures, triggers, and events
