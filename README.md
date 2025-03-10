# NYC-GT-data-preparation

README: Data Cleaning and Processing for G&T Results

This script performs **data cleaning, transformation, and standardization** for two datasets containing G&T (Gifted & Talented) assessment results from the years **2017-18** and **2018-19**. The R script ensures that data inconsistencies, missing values, and outliers are handled for analysis.

Dataset Information
- **File 1**: `Copy of G&T Results 2018-19 Responses - Sheet1.xlsx`
- **File 2**: `Copy of G&T Results 2017-18 (Responses) - Form Responses 1 (1).xlsx`

Objectives
- Standardize column names for consistency.
- Convert timestamps into proper date format.
- Clean and categorize categorical variables (e.g., grade levels, birth months).
- Convert assessment scores to numeric values.
- Handle missing values and incorrect inputs.
- Standardize school preferences and enrollment responses.
- Merge both datasets into a single cleaned dataset for analysis.

Required Libraries
The following R libraries are required to run the script:
library(openxlsx)  # For reading Excel files
library(dplyr)  # For data manipulation
library(janitor)  # For cleaning column names
library(tidyverse)  # For data processing and visualization
library(ggplot2)  # For plotting
library(stringr)  # For string manipulation

Data Cleaning Steps
1. **Cleaning Column Names**
- `clean_names()` function is applied to ensure all column names are standardized (lowercase, no spaces, underscores for separation).

 2. **Handling Timestamps**
-The `timestamp` column is converted to a proper date format using:
  
  mutate(date = as.Date(as.numeric(timestamp), origin = "1899-12-30"))
 
-The `timestamp` column is then removed.

3. **Standardizing Categorical Variables**
- `entering_grade_level` is converted into meaningful categories such as `first`, `second`, `kindergarten`, etc.
- `birth_month` is standardized to correct spelling errors and numeric representations.

4. **Cleaning OLSAT Verbal Scores & Percentiles**
- Scores in formats like `25/30` are extracted and converted into numeric values.
- Outliers (scores <10 or >30) are set to `NA`.
- Percentile values that were incorrectly formatted (e.g., `0.91` instead of `91`) are corrected.

5. **Cleaning NNAT Non-Verbal Scores & Percentiles**
- Scores are extracted and converted to numeric values.
- Non-relevant characters (e.g., `**`, `---`, `Fill out later`) are removed.
- NNAT percentiles <10 are corrected to appropriate scale (multiplied by 100 when necessary).

6. **Handling Missing Values**
- If missing values were less than **5%**, they were left as `NA`.
- Specific placeholders (e.g., `:-(`, `No idea!`, `None`) in `school_preferences` were replaced with "none".

7. **Processing School Preferences**
- `school_preferences` was cleaned by:
  - Removing unnecessary characters (`?`, `/` replaced with `, `)
  - Extracting the **first choice** from the list of preferences.
  - Creating a new `remaining_choices` column by removing the `first_choice`.

8. **Cleaning Enrollment Responses**
- Standardized `will_you_enroll_there` values:
  - "No", "no", "NO" → `No`
  - "Maybe" → `Unsure`
  - `NA` values replaced with `""`.

9. **Merging Cleaned Datasets**
- After processing both datasets separately, they were merged using:
  ```r
  combined_gt <- bind_rows(gt_1_clean1, gt_2_clean1)
  ```
- The final dataset is ready for further analysis and visualization.
