**Uber Ride Analysis Dashboard - Project Documentation**

---

### 🚀 Project Overview
This project aims to analyze and visualize Uber ride data to understand user behavior, ride patterns, and travel purposes using Python for data cleaning and Power BI for dashboard creation. The dashboard offers dynamic insights into trip volume, duration, mileage, and purpose trends.

---

### 📃 Dataset Description
**Source**: `UberDataset.csv`

**Original Columns:**
- `START_DATE`: Start time of the trip
- `END_DATE`: End time of the trip
- `CATEGORY`: Type of ride (Business/Personal)
- `START`: Starting location
- `STOP`: Ending location
- `MILES`: Distance of trip
- `PURPOSE`: Purpose of ride (Meeting, Meal/Entertain, etc.)

**Total Records**: 1,156

---

### ⚙️ Data Cleaning & Preparation (Python)

#### 1. **Handling Missing & Duplicate Values**
- Dropped 1 row with missing `END_DATE`
- Filled missing `PURPOSE` values with "Unknown"
- Removed duplicate rows

#### 2. **Date Cleaning and Feature Engineering**
Converted `START_DATE` and `END_DATE` to datetime formats.
Extracted the following features:
- `Start_Date`, `Start_Hour`, `Start_Minute`
- `End_Date`, `End_Hour`, `End_Minute`
- `Month`, `Year`, `Weekday`
- `Day_Time`: Categorized into Morning, Afternoon, Evening, Night
- `Duration_MIN`: Trip duration in minutes
- `IS_WEEKEND`: Boolean for Saturday/Sunday
- `Month_Name`: Full month name derived from Month

**Final Shape**: 1,154 rows × 18 columns

#### 3. **Saved Output**
- Cleaned dataset exported as `UberDataCleaned.csv`

---

### 📊 Dashboard Design (Power BI)

#### 🌐 Theme
- Inspired by Uber’s brand colors: black background, white text, yellow highlights

#### 🗕️ Filters Used
- Month
- Category
- Purpose
- Weekday

#### 📈 KPIs
- **Total Trips**
- **Total Miles**
- **Avg Trip Duration**
- **Avg Miles per Trip**

#### 🔍 Visualizations & Interpretations

| Chart | Description | Usage |
|-------|-------------|--------|
| **Line Chart** | Total Trips by Month (Abbreviated) | Tracks monthly trip trends |
| **Bar Chart** | Total Trips by Weekday | Shows ride frequency on weekdays |
| **Bar Chart** | Total Trips by Day_Time | Time-of-day analysis of rides |
| **Bar Chart** | Total Trips by Category | Ride type distribution |
| **Stacked Bar Chart** | Total Trips by Purpose and Category | Comparison across business/personal purposes |
| **Donut Chart** | Purpose Distribution | Breakdown of all ride purposes |

#### 🌍 Interactions
- Dynamic filtering based on slicers
- Tooltips display actual values
- Color formatting used to highlight key insights (e.g., highest weekday trip volume)

---

### ➖ Calculated Measures (DAX)

```DAX
Total Trips = COUNTROWS('Table')
Total Miles = SUM('Table'[MILES])
Avg Trip Duration = AVERAGE('Table'[Duration_MIN])
Avg Miles Per Trip = [Total Miles] / [Total Trips]
Weekend % =
    DIVIDE(
        CALCULATE(COUNTROWS('Table'), 'Table'[IS_WEEKEND] = TRUE()),
        [Total Trips]
    )
```

### ✍️ Axis Sorting and Formatting
- Created separate `Month_Abbr` column using Power BI's `FORMAT` function:
  ```DAX
  Month_Abbr = FORMAT('Table'[START_DATE], "mmm")
  ```
- Weekday sorting controlled using a custom `Weekday_Number` column (0=Sunday, ..., 6=Saturday)

---

### 🌟 Highlighting Best Performing Bars
Used DAX measures and conditional formatting to:
- Identify highest value using `MAXX`
- Return distinct color if record equals max value

```DAX
Max_Miles_Flag =
IF(
    SUM('Table'[MILES]) = CALCULATE(MAXX(VALUES('Table'[Weekday]), SUM('Table'[MILES]))),
    1,
    0
)
```

Then used a custom `Bar_Color` measure to apply conditional formatting.

---

### 🏆 Key Insights
- Most trips occurred in **December** (146 trips)
- **Friday** had the highest weekly ride count (206)
- **Afternoon** is the most popular travel time (446 trips)
- Majority of trips are **Business** related (93.3%)
- Primary ride purpose is **Unknown**, followed by **Meeting** and **Meal/Entertainment**

---

### 📊 Deliverables
- `UberDataCleaned.csv`: Cleaned dataset
- **Power BI Dashboard** (refer to image)

---

### 🔐 Tools & Libraries Used
- Python (pandas, numpy, matplotlib, seaborn)
- Power BI (DAX, visualizations, custom themes)

---

### 📁 Suggested Dashboard Title
**"Uber Ride Analysis Dashboard"**  
*Trends, Purposes & Patterns in Ride Usage*

---

### 📂 Future Improvements
- Predictive modeling for trip duration
- Integration with maps for location heatmaps
- Monthly reports and alerts using Power BI service

---

Prepared by: **Syed Nazmul Hasan**
