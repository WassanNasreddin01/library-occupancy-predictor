# Library Occupancy Prediction Model

A machine learning project that predicts occupancy levels across four Cornell University libraries, Olin, Uris, Mann, and Math, to help students find study spaces more efficiently, especially during high-pressure academic periods like prelims and finals.

Built as part of INFO 4125: Project Management at Cornell University (Fall 2025).

---

## The Problem

Finding available group study space at Cornell is genuinely difficult, especially during prelims. Students waste time walking between libraries with no way to know how crowded a space will be before they arrive. Existing tools like BearTrak and Booked show hours or reservations, but offer no real-time or predictive availability insights.

---

## What We Built

A Random Forest regression model that forecasts library occupancy by hour, trained on real foot traffic data collected manually across four libraries over five weeks (October–November 2025).

The model outputs predicted occupancy counts for each library at any given hour and day, accounting for patterns like day of week, time of day, closing hours, and prelim periods.

A companion Figma prototype (designed by a separate UI subteam) translates the model's outputs into a student-facing dashboard with hour-by-hour occupancy forecasts, a room booking interface, and a date picker for planning ahead.

---

## My Role

I was part of the **data subteam**, responsible for:
- Data collection, cleaning, and preprocessing
- Feature engineering (temporal and operational features)
- Model development, training, and evaluation
- Cross-validation strategy and performance analysis

---

## Data

| Field | Description |
|---|---|
| `Date` | Observation date (Oct 5 – Nov 13, 2025) |
| `Time` | Time of observation |
| `Mann`, `Olin`, `Uris`, `Math` | Occupancy count per library |
| `DayOfWeek` / `DayOfWeek_num` | Day of week (string and numeric) |
| `Hour` | Hour of observation (24h float) |
| `IsPrelimDay` | Binary flag for Cornell prelim exam days |
| `IsPrelimMinus1` | Binary flag for the day before a prelim |

**297 observations** collected manually across 5 weeks, with 91 prelim-period data points.

> **Note on data:** Due to time constraints, we augmented our real dataset with synthetic data generated to follow observed patterns using ChatGPT. Model accuracy (~75%) should be interpreted as a proof-of-concept benchmark, not a deployment-ready performance guarantee.

---

## Model

- **Algorithm:** Random Forest Regressor (scikit-learn)
- **Features:** Hour, day of week, library closing hours, prelim day indicator, day-before-prelim indicator
- **Validation:** Standard K-Fold CV and Date-Based GroupKFold CV (testing generalization to entirely unseen days)
- **Performance:** ~75% accuracy (predictions within acceptable deviation threshold); MAE of approximately 25–65 people depending on the library

---

## Key Findings

- **Hour of day** and **day of week** are the strongest predictors of occupancy across all four libraries
- **Mann Library** shows the most predictable occupancy patterns; **Math Library** is the least predictable due to its longer operating hours and lower late-night activity
- Adding operational features (closing hours, prelim flags) produced incremental accuracy gains over temporal features alone
- The model generalizes reasonably well to unseen days, though variance increases during peak hours

---

## Repository Structure

```
library-occupancy-predictor/
├── README.md
├── library_occupancy_MS3.csv
└── Model.ipynb
```

---

## Tools and Libraries

- Python, pandas, NumPy
- scikit-learn (RandomForestRegressor, KFold, GroupKFold, permutation_importance)
- Matplotlib
- Figma (UI prototype, separate subteam)

---

## Future Work

- Integrate real-time occupancy data via Cornell IT APIs or Wi-Fi device counts
- Expand to additional campus libraries and study spaces
- Conduct formal user testing against the Figma prototype
- Deploy as a live web application with the predictive model powering the dashboard
