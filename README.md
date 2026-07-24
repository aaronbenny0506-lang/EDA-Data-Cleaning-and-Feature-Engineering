# Used Car Price Prediction - EDA, Cleaning & Feature Engineering

## 📁 Dataset Overview

This project uses the **Used Car Price Prediction Dataset** (source: [Kaggle](https://www.kaggle.com/datasets/taeefnajib/used-car-price-prediction-dataset)).

- **Rows:** 4,009
- **Columns:** 12
- **Target variable:** `price`

| Column | Description |
|---|---|
| `brand` | Car manufacturer (e.g. Ford, BMW) |
| `model` | Specific model name |
| `model_year` | Year the car model was released |
| `milage` | Distance driven (originally text, e.g. "51,000 mi.") |
| `fuel_type` | Type of fuel (Gasoline, Hybrid, E85 Flex Fuel, etc.) |
| `engine` | Engine specification (horsepower, displacement, cylinders) |
| `transmission` | Transmission type |
| `ext_col` | Exterior color |
| `int_col` | Interior color |
| `accident` | Accident/damage history |
| `clean_title` | Whether the car has a clean title |
| `price` | Listing price (originally text, e.g. "$10,300") |

---

## 🔍 Data Quality Issues Identified

1. **`milage` and `price` stored as text**, with units and symbols ("51,000 mi.", "$10,300") instead of numeric values - blocking any statistical or numerical analysis.
2. **Missing values** in three columns:
   - `fuel_type`: 170 missing (4.24%)
   - `accident`: 113 missing (2.82%)
   - `clean_title`: 596 missing (14.87%) - and the only non-null value present is "Yes," suggesting missing likely means the title is *not* clean.
3. **No duplicate rows** found (0 duplicates).
4. **Outliers in `model_year`**: 67 outliers identified via the IQR method - mostly older cars (some dating back to 1974) sitting far below the typical 2012–2024 cluster of listings.

---

## 🧹 Cleaning Techniques Applied

- **Type correction:** Stripped `"mi."` and `"$"`/commas from `milage` and `price`, converting both to numeric (float) types.
- **Missing value handling:**
  - `clean_title`: missing filled with `"No"` (since missing values likely indicate the car lacks a clean title, rather than a random gap).
  - `accident` and `fuel_type`: missing values filled with the column mode (`"None reported"` and `"Gasoline"` respectively).
- **Duplicate removal:** Verified and dropped any duplicate rows (none were present).
- **Outlier handling:** Applied the IQR method to cap extreme values in numeric columns (`model_year`, `milage`, `price`) at their lower/upper bounds rather than deleting rows, to preserve dataset size.

---

## 🛠️ Feature Engineering Performed

Six new features were engineered to support future ML modeling:

1. **`car_age`** - Current year minus `model_year`; captures vehicle age as a continuous variable, often more predictive of price than raw year.
2. **`price_per_mile`** - `price / milage`; a value-density metric useful for spotting under/over-priced listings.
3. **`has_clean_title`** - Binary flag (1/0) derived from `clean_title`; title status strongly affects resale value.
4. **`had_accident`** - Binary flag (1/0) derived from `accident`; accident history is a major price-driving factor.
5. **`high_mileage`** - Binary flag (1/0) indicating whether a car's mileage is above the dataset median.
6. **`is_luxury_brand`** - Binary flag (1/0) indicating whether `brand` belongs to a defined luxury set (Porsche, Mercedes-Benz, Audi, Lexus, BMW, Land Rover).

---

## 💡 Five Key Insights from the Analysis

1. `milage` and `price` were stored as text with embedded units/symbols, requiring cleaning before any numeric analysis or modeling could be performed.
2. `clean_title` was missing in ~14.9% of rows, and every non-missing value was "Yes", indicating missing values likely represent cars *without* a clean title rather than random gaps.
3. `fuel_type` was missing in 4.24% of rows, but Gasoline made up ~86% of known values, making mode imputation a reasonable choice.
4. Outlier detection on `model_year` (IQR method) flagged 67 vintage cars (some from the 1970s–90s) sitting well outside the typical 2012–2024 range of most listings.
5. The `brand` distribution showed a mix of mainstream and premium brands (Ford, BMW, Mercedes-Benz, Chevrolet, Porsche in the top 5), indicating the dataset spans both budget and luxury used car segments.

---

## 📦 Repository Contents

- `task-3.ipynb` - Complete EDA, data cleaning, and feature engineering workflow.
- `cleaned_used_cars.csv` - Final cleaned dataset.
- `README.md` - This file.

---


