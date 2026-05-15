# Student Lifestyle, Academic Performance and Wellbeing Dashboard Documentation

## 1. Dashboard Overview

This Power BI dashboard analyzes the relationship between student lifestyle factors, academic performance, stress level, and depression status.

The dashboard is designed to be simple, clean, and useful. Instead of focusing only on demographic information, it focuses on meaningful lifestyle indicators such as sleep duration, study hours, social media usage, physical activity, stress level, CGPA, and depression status.

The main purpose of the dashboard is to help users understand how student habits and wellbeing indicators may be connected to academic performance and mental health.

---

## 2. Data Source

The dataset used for this dashboard is:

```text
student_lifestyle_100k.csv
```

After importing the data into Power BI, the table was renamed to:

```text
Fact_Table
```

The dataset contains student-level records with lifestyle, academic, demographic, and wellbeing information.

### Main Fields Used

| Field Name | Description |
|---|---|
| Student_ID | Unique identifier for each student |
| Gender | Student gender |
| Age | Student age |
| Department | Student department |
| CGPA | Academic performance score |
| Study_Hours | Daily study hours |
| Sleep_Duration | Daily sleep duration in hours |
| Social_Media_Hours | Daily social media usage in hours |
| Physical_Activity | Physical activity duration in minutes |
| Stress_Level | Student stress level |
| Depression | Depression status, shown as TRUE or FALSE |

---

## 3. Dashboard Objective

The dashboard aims to answer the following questions:

1. What is the overall academic and wellbeing condition of the students?
2. What percentage of students are marked as depressed?
3. How does stress level relate to depression rate?
4. How does sleep duration relate to stress level?
5. How does study time relate to CGPA?
6. How does social media usage relate to CGPA?
7. How do lifestyle indicators compare between depressed and not depressed students?

---

## 4. Key Performance Indicators

The dashboard uses five main KPI cards.

| KPI | Purpose |
|---|---|
| Total Students | Shows the total number of students in the dataset |
| Average CGPA | Shows the overall academic performance level |
| Average Stress Level | Shows the general stress condition of students |
| Depression Rate | Shows the percentage of students marked as depressed |
| Average Sleep Hours | Shows the average sleep duration of students |

### KPI Summary from the Dataset

| Metric | Value |
|---|---:|
| Total Students | 100,000 |
| Average CGPA | 2.90 |
| Average Stress Level | 4.13 |
| Depression Rate | 10.06% |
| Average Sleep Hours | 7.00 |

---

## 5. Power BI Measures

The following DAX measures were created in Power BI.

### Total Students

```DAX
Total Students = DISTINCTCOUNT('Fact_Table'[Student_ID])
```

### Average CGPA

```DAX
Average CGPA = AVERAGE('Fact_Table'[CGPA])
```

### Average Sleep Hours

```DAX
Average Sleep Hours = AVERAGE('Fact_Table'[Sleep_Duration])
```

### Average Study Hours

```DAX
Average Study Hours = AVERAGE('Fact_Table'[Study_Hours])
```

### Average Social Media Hours

```DAX
Average Social Media Hours = AVERAGE('Fact_Table'[Social_Media_Hours])
```

### Average Physical Activity

```DAX
Average Physical Activity = AVERAGE('Fact_Table'[Physical_Activity])
```

### Average Stress Level

```DAX
Average Stress Level = AVERAGE('Fact_Table'[Stress_Level])
```

### Depressed Students

```DAX
Depressed Students =
CALCULATE(
    [Total Students],
    'Fact_Table'[Depression] = TRUE()
)
```

### Depression Rate

```DAX
Depression Rate =
DIVIDE(
    [Depressed Students],
    [Total Students]
)
```

The Depression Rate measure should be formatted as a percentage in Power BI.

---

## 6. Calculated Columns

Calculated columns were created to group numeric lifestyle variables into simple categories. These categories make the dashboard easier to understand.

### Depression Label

```DAX
Depression Label =
IF(
    'Fact_Table'[Depression] = TRUE(),
    "Depressed",
    "Not Depressed"
)
```

### Sleep Group

```DAX
Sleep Group =
SWITCH(
    TRUE(),
    'Fact_Table'[Sleep_Duration] < 6, "Low sleep (<6h)",
    'Fact_Table'[Sleep_Duration] <= 8, "Healthy sleep (6-8h)",
    "High sleep (>8h)"
)
```

### Study Group

```DAX
Study Group =
SWITCH(
    TRUE(),
    'Fact_Table'[Study_Hours] < 3, "Low study (<3h)",
    'Fact_Table'[Study_Hours] <= 6, "Medium study (3-6h)",
    "High study (>6h)"
)
```

### Social Media Group

```DAX
Social Media Group =
SWITCH(
    TRUE(),
    'Fact_Table'[Social_Media_Hours] < 2, "Low social media (<2h)",
    'Fact_Table'[Social_Media_Hours] <= 5, "Moderate social media (2-5h)",
    "High social media (>5h)"
)
```

### Activity Group

```DAX
Activity Group =
SWITCH(
    TRUE(),
    'Fact_Table'[Physical_Activity] < 60, "Low activity (<60 mins)",
    'Fact_Table'[Physical_Activity] <= 120, "Moderate activity (60-120 mins)",
    "High activity (>120 mins)"
)
```

### Stress Group

```DAX
Stress Group =
SWITCH(
    TRUE(),
    'Fact_Table'[Stress_Level] <= 3, "Low stress",
    'Fact_Table'[Stress_Level] <= 6, "Medium stress",
    "High stress"
)
```

---

## 7. Sort Columns

Sort columns were created so that grouped categories appear in a logical order instead of alphabetical order.

### Sleep Group Sort

```DAX
Sleep Group Sort =
SWITCH(
    TRUE(),
    'Fact_Table'[Sleep_Duration] < 6, 1,
    'Fact_Table'[Sleep_Duration] <= 8, 2,
    3
)
```

### Study Group Sort

```DAX
Study Group Sort =
SWITCH(
    TRUE(),
    'Fact_Table'[Study_Hours] < 3, 1,
    'Fact_Table'[Study_Hours] <= 6, 2,
    3
)
```

### Social Media Group Sort

```DAX
Social Media Group Sort =
SWITCH(
    TRUE(),
    'Fact_Table'[Social_Media_Hours] < 2, 1,
    'Fact_Table'[Social_Media_Hours] <= 5, 2,
    3
)
```

### Stress Group Sort

```DAX
Stress Group Sort =
SWITCH(
    TRUE(),
    'Fact_Table'[Stress_Level] <= 3, 1,
    'Fact_Table'[Stress_Level] <= 6, 2,
    3
)
```

In Power BI, each group column should be sorted by its matching sort column.

Example:

```text
Sleep Group -> Sort by column -> Sleep Group Sort
```

---

## 8. Dashboard Layout

The dashboard uses a one-page layout.

```text
Title: Student Lifestyle, Academic Performance and Wellbeing Dashboard

Top Row:
[Total Students] [Average CGPA] [Average Stress Level] [Depression Rate] [Average Sleep Hours]

Left Side:
Department slicer

Main Visuals:
1. Depression Distribution
2. CGPA by Social Media Usage
3. Stress Level by Sleep Duration
4. CGPA by Study Group
5. Depression Risk by Stress Level
6. Lifestyle Summary by Depression Status
```

---

## 9. Dashboard Visuals

### Visual 1: Depression Distribution

**Visual Type:** Donut Chart

| Field | Usage |
|---|---|
| Depression Label | Legend |
| Total Students | Values |

This visual shows the proportion of students who are depressed and not depressed. It gives a quick overview of the depression distribution in the dataset.

Recommended formatting:

```text
Data labels: On
Detail labels: Category + Percent of total
```

---

### Visual 2: CGPA by Social Media Usage

**Visual Type:** Clustered Column Chart

| Field | Usage |
|---|---|
| Social Media Group | X-axis |
| Average CGPA | Y-axis |

This visual shows how average CGPA changes across different levels of social media usage.

This is useful because high social media usage appears to be associated with lower average CGPA.

---

### Visual 3: Stress Level by Sleep Duration

**Visual Type:** Clustered Column Chart

| Field | Usage |
|---|---|
| Sleep Group | X-axis |
| Average Stress Level | Y-axis |

This visual shows the relationship between sleep duration and stress level.

This is useful because students with low sleep generally show a higher stress level compared with students with healthy sleep.

---

### Visual 4: CGPA by Study Group

**Visual Type:** Clustered Column Chart

| Field | Usage |
|---|---|
| Study Group | X-axis |
| Average CGPA | Y-axis |

This visual shows how study time relates to academic performance.

Students with higher study hours tend to have a slightly higher average CGPA.

---

### Visual 5: Depression Risk by Stress Level

**Visual Type:** Bar Chart

| Field | Usage |
|---|---|
| Stress Group | Y-axis |
| Depression Rate | X-axis |

This is one of the most important visuals in the dashboard.

It shows that students with high stress have a much higher depression rate compared with students in low or medium stress groups.

Recommended formatting:

```text
Depression Rate format: Percentage
Decimal places: 1 or 2
```

---

### Visual 6: Lifestyle Summary by Depression Status

**Visual Type:** Matrix

| Field | Usage |
|---|---|
| Depression Label | Rows |
| Total Students | Values |
| Average CGPA | Values |
| Average Sleep Hours | Values |
| Average Study Hours | Values |
| Average Social Media Hours | Values |
| Average Physical Activity | Values |
| Average Stress Level | Values |

This matrix compares the lifestyle and academic indicators of depressed and not depressed students.

This visual is more meaningful than a department summary because it directly supports the main theme of the dashboard: student lifestyle, performance, and wellbeing.

---

## 10. Slicer

The dashboard includes a Department slicer.

The slicer allows users to filter the dashboard by department, such as:

```text
Arts
Business
Engineering
Medical
Science
```

This keeps the dashboard interactive while still keeping the main visuals focused on lifestyle and wellbeing.

Optional slicers that can also be added:

```text
Gender
Age
Depression Label
```

---

## 11. Design Choices

The dashboard uses a dark blue and white theme to create a clean academic-style design.

### Design decisions

| Design Element | Reason |
|---|---|
| Dark header | Makes the dashboard title stand out |
| Light KPI cards | Makes key numbers easy to read |
| Simple bar and column charts | Keeps the dashboard easy to understand |
| Limited slicers | Avoids unnecessary complexity |
| One-page layout | Makes the dashboard simple and fast to read |

---

## 12. Main Insights

Based on the dashboard, the following insights can be observed:

1. The dataset contains 100,000 student records.
2. The average CGPA is 2.90.
3. The average stress level is 4.13.
4. The average sleep duration is 7.00 hours.
5. The overall depression rate is 10.06%.
6. Students with high stress have a noticeably higher depression rate.
7. Students with low sleep show higher average stress.
8. Students with higher study hours tend to have slightly higher CGPA.
9. Students with high social media usage tend to have lower average CGPA.
10. Comparing depressed and not depressed students provides a useful summary of lifestyle and wellbeing differences.

---

## 13. How to Use the Dashboard

Users can interact with the dashboard by selecting a department from the slicer. Once a department is selected, all visuals update automatically.

The dashboard can be used to quickly compare lifestyle patterns and identify student groups that may need more academic or wellbeing support.

Example use cases:

```text
- Check whether high-stress students have higher depression rates.
- Compare CGPA across different study-hour groups.
- Analyze whether social media usage is related to lower CGPA.
- Compare lifestyle patterns between depressed and not depressed students.
```

---

## 14. Limitations

This dashboard shows relationships and patterns in the data, but it does not prove cause and effect.

For example, high social media usage may be associated with lower CGPA, but the dashboard cannot prove that social media directly causes lower academic performance.

The dashboard should be used for descriptive analysis and decision support, not as a final medical or psychological diagnosis tool.

---

## 15. Conclusion

The Student Lifestyle, Academic Performance and Wellbeing Dashboard provides a simple but meaningful view of student behavior and wellbeing.

The dashboard focuses on useful indicators such as CGPA, stress level, sleep duration, study hours, social media usage, and depression rate. It helps users understand how lifestyle patterns are connected with academic performance and mental wellbeing.

The final design is intentionally simple, but every visual supports the main story of the dashboard.
