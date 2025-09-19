# PRODUCT SALES INSIGHTS

## 4.1 Section A: SQL

### 1. Find the Top 3 Products with the Highest Total Sales
```sql
SELECT item_nbr, SUM(units) AS total_units
FROM train
GROUP BY item_nbr
ORDER BY total_units DESC
LIMIT 3;

-- Explanation:
-- SUM(units) → calculates total units sold for each product.
-- GROUP BY item_nbr → groups the sales by each product.
-- ORDER BY total_units DESC → sorts products from highest to lowest total units sold.
-- LIMIT 3 → returns only the top 3 products.
```
![Top 3 Products](https://github.com/user-attachments/assets/0fd93520-fdb5-4858-a8d9-f3060944d4ae)

---

### 2. Join Sales Data with Weather Stations
```sql
SELECT t.date,
       t.store_nbr,
       t.item_nbr,
       t.units,
       k.station_nbr
FROM train t 
JOIN key k 
  ON t.store_nbr = k.store_nbr;

-- Explanation:
-- JOIN maps each store in train to its corresponding weather station.
-- Columns selected:
-- t.date, t.store_nbr, t.item_nbr, t.units → original sales data
-- k.station_nbr → the weather station associated with each store
```
![Join Train with Weather](https://github.com/user-attachments/assets/dbbe893f-0f46-4581-a198-0f6c6b7bb5cd)

---

### 3. Daily Sales and Average Temperature for Top 3 Products
```sql
SELECT 
    t.date,
    t.item_nbr,
    SUM(t.units) AS daily_units,
    AVG(w.tavg) AS avg_temperature
FROM train t
JOIN key k 
    ON t.store_nbr = k.store_nbr
JOIN weather w 
    ON k.station_nbr = w.station_nbr 
   AND t.date = w.date
WHERE t.item_nbr IN (
    SELECT item_nbr 
    FROM train
    GROUP BY item_nbr
    ORDER BY SUM(units) DESC
    LIMIT 3
)
GROUP BY t.item_nbr, t.date
ORDER BY t.date DESC, t.item_nbr DESC;
```
![Daily Sales & Avg Temp](https://github.com/user-attachments/assets/46581779-c632-4566-a149-2f445b151ab7)

---

## 4.2 Section B: R Programming

### 1. Load and Prepare Data
```r
library(DBI)
library(RSQLite)

# Connect to SQLite
con <- dbConnect(SQLite(), "database.sqlite")

# Load joined dataset
df <- dbGetQuery(con, "
  SELECT t.date, t.store_nbr, t.item_nbr, t.units, k.station_nbr, w.tavg, w.preciptotal, w.codesum
  FROM train t
  JOIN key k ON t.store_nbr = k.store_nbr
  JOIN weather w ON k.station_nbr = w.station_nbr AND t.date = w.date
")

# Convert date column
df$date <- as.Date(df$date)
```

---

### 2. Data Cleaning
```r
# Handle missing values
df$preciptotal[df$preciptotal == "T"] <- 0.001
df$preciptotal[df$preciptotal == ""] <- NA
df$preciptotal <- as.numeric(df$preciptotal)

# Impute missing average temperatures with median
df$tavg <- as.numeric(df$tavg)
df$tavg[is.na(df$tavg)] <- median(df$tavg, na.rm = TRUE)

# Outlier handling (example domain-informed threshold: >500 units)
df$outlier_flag <- df$units > 500
```

---

### 3. Feature Creation
```r
# Weekend flag
df$is_weekend <- weekdays(df$date) %in% c("Saturday", "Sunday")

# Rainy day flag
df$is_rainy_day <- grepl("RA", df$codesum)
```

---

### 4. Predictive Modeling
```r
# Linear regression example
lm_model <- lm(units ~ tavg + preciptotal + is_weekend + is_rainy_day, data = df)
summary(lm_model)

# Decision tree example
library(rpart)
tree_model <- rpart(units ~ tavg + preciptotal + is_weekend + is_rainy_day, data = df)
```

---

### 5. Model Evaluation
```r
# Train-test split
set.seed(123)
train_idx <- sample(seq_len(nrow(df)), size = 0.7 * nrow(df))
train_data <- df[train_idx, ]
test_data <- df[-train_idx, ]

# Evaluate linear regression
lm_pred <- predict(lm_model, newdata = test_data)
lm_rmse <- sqrt(mean((test_data$units - lm_pred)^2))
```

---

## 4.3 Section C: Interpretation & Visualization

### 1. Weather Impact Summaries
- **Product 1**: Strong positive relationship with temperature — sales rise during warmer days.  
- **Product 2**: Negative correlation with rainfall — demand drops during rainy days.  
- **Product 3**: Mixed effect — both temperature and rain influence sales, but weekends amplify demand.

---

### 2. Visualizations
```r
# Scatter plot: Temperature vs Units
plot(df$tavg, df$units, main="Temperature vs Units", xlab="Avg Temp", ylab="Units")

# Time series: Units over time
plot(df$date, df$units, type="l", main="Units Sold Over Time", xlab="Date", ylab="Units")
```

---

### 3. Business Recommendations
- **Product 1**: Increase stock during summer — strong temperature-driven demand.  
- **Product 2**: Reduce stock during rainy weeks — demand decreases with rainfall.  
- **Product 3**: Weekend promotions can boost sales further, especially when combined with favorable weather.  

---

## Deliverables
- **Data Preparation (SQL)**: Joined weather and station data, filtered to top 3 products.  
- **Data Cleaning and Feature Creation (R)**: Missing values handled, features engineered.  
- **Predictive Models (R)**: Linear regression & decision tree for interpretability.  
- **Model Outputs (R)**: Summaries, RMSE, interpretation of coefficients.  
- **Visualizations (R)**: Plots with insights; option for dashboard (Shiny/Tableau).  

---
