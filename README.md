# Bellabeat Case Study — User Health Insights

## Project Overview

This project analyzes user activity, sleep, and weight data to extract **actionable health insights**.  
Using datasets from Bellabeat / Fitbit, it employs Python (Pandas & NumPy), SQL queries (via `pandasql`), and visualizations (Seaborn & Matplotlib) to uncover trends in:

- User consistency (how often users logged activity)  
- Active vs. sedentary lifestyle patterns  
- Daily step distributions by hour  
- Relationship between sleep duration and activity  
- Weight and BMI trends over time  

---

## 📁 Data & Sources

- **Datasets used** (from Kaggle / Fitbit data collection):  
  - `dailyActivity_merged.csv`  
  - `hourlySteps_merged.csv`  
  - `minuteSleep_merged.csv`  
  - `weightLogInfo_merged.csv`  

- **Dataset download & source link:**  
  [Bellabeat Case Study (NicoleLynns) on Kaggle](https://www.kaggle.com/code/nicolelynns/bellabeat-case-study-excel-sql-tableau/notebook)

---

## 🛠️ Preprocessing Steps

1. **Load** all CSV files (`dailyActivity`, `hourlySteps`, `sleepMinutes`, `weightLog`) into Pandas DataFrames.  
2. **Convert date / time** columns to Pandas `datetime` type:  
   - `ActivityDate` in `dailyActivity`  
   - `ActivityHour` in `hourlySteps`  
   - `date` in `sleepMinutes` → aggregated to `SleepDate`  
   - `Date` in `weightLog`  
3. **Remove duplicates** by key fields (`Id` + date or hour as appropriate).  
4. **Handle outliers**: for example, drop rows in `dailyActivity` where `TotalSteps = 0`.  
5. **Aggregate sleep data**: Sum minute-level sleep to daily totals per user.  
6. **Trim weight data** to relevant columns (`Id`, `Date`, `WeightKg`, `BMI`) for weight/BMI trend analysis.  

---

## 🔍 Five Key Facts Extracted & SQL Queries

These are the five facts/questions explored in the analysis, along with SQL query logic and what was found:

| # | Fact / Question | SQL Query Summary | Key Insight |
|---|------------------|---------------------|-------------|
| 1 | **User Consistency**: How many days did each user log activity? Categorize into Light / Moderate / Active users. | Count `(ActivityDate)` grouped by `Id`, then `CASE` to bucket days logged. | Many users are in the *Moderate* or *Light* bands. Few users logged nearly all days. |
| 2 | **Sedentary vs Active Lifestyle**: Average minutes spent in Very, Fairly, Light activity vs Sedentary. | Use `AVG(...)` for each minutes column, then compute `TotalActiveMinutes = Avg(Very + Fairly + Light)` per user. | Some users have very low “active” minutes; lifestyle movement (light activity) often contributes more than high-intensity. |
| 3 | **Steps Distribution by Hour**: Which hours of day do users take more steps? | Sum `StepTotal` grouped by the hour of `ActivityHour` (from `hourlySteps`) sorted descending. | Peak activity hours tend to be early evening / after work hours. |
| 4 | **Sleep vs Activity**: How does sleep duration relate to steps taken? | Join `dailyActivity` with `sleep_daily` on `Id` + date; group by sleep duration buckets, average steps per bucket. | Moderate sleep (≈ 6-8 hours) correlates with highest step counts; very low or very high sleep correspond to fewer steps. |
| 5 | **Weight & BMI Trends**: How stable or volatile is weight / BMI over time per user? | Select users with more than one weight record; compute `Min`, `Max`, `Avg`, and *WeightChange* (max-min) per user. | Some users show little change; some show fluctuations; average BMI indicates user weight status. |

---

## 📊 Visualizations & Insights

- Bar charts showing **distribution of users** by consistency category (Light/Moderate/Active)  
- Stacked bars comparing active vs. sedentary minutes for top users  
- Line / bar chart of **steps by hour of day** to identify when users are most active  
- Bar chart of **average steps by sleep-duration buckets**  
- Comparison of **weight changes over time** for users with multiple logs  

These visualizations help communicate trends clearly for stakeholders, enabling actionable takeaways such as encouraging better sleep schedules, increasing light activity, or targeting less active users with goal-setting features.

---

## 🛠 Technologies & Tools Used

- **Python**: Pandas & NumPy for data loading, cleaning, aggregation  
- **SQL** via `pandasql`: running SQL-style queries directly over DataFrames  
- **Seaborn & Matplotlib**: for plotting trends, distributions, and comparisons  
- Jupyter Notebook in VSCode: combining code, SQL, output tables, and charts in one reproducible analysis  

---

## ✅ How to Run / Reproduce

1. Download the datasets from the Kaggle link above and place them in a `data/` folder alongside the notebook.  
2. Install required libraries (if not already):  
   ```bash
   pip install pandas numpy pandasql matplotlib seaborn
