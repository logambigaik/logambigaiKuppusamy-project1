### PRODUCT SALES INSIGHTS

### 4.1. Section A: SQL 

1. Write a query to find the top 3 products with the highest total sales (based on units 
sold) from the train table.

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
<img width="814" height="314" alt="image" src="https://github.com/user-attachments/assets/0fd93520-fdb5-4858-a8d9-f3060944d4ae" />

2. Using the key table, write a query to join the sales data (train) with the correct 
weather station using store_nbr.

3. Write a query to return daily sales and average temperature (tavg) for one of the top 3 
products.


### 4.2. Section B: R Programming 
1. Load the joined dataset into R using DBI and RSQLite; and convert date columns to Date
format.

2. Clean the data by:
    ● Handling missing weather values (e.g., preciptotal, tavg). 
    o Explain how you dealt with special characters like "T" or empty strings.
    o Avoid dropping rows blindly — consider imputation where appropriate 
      (e.g., mean/median for tavg).
    ● Removing or flagging outliers in units sold. 
    o Do not rely solely on IQR if the data is zero-inflated (e.g., many zeros 
      in units).
    o Justify your approach clearly (percentile trimming, log transform, 
      domain-informed thresholds, etc.).

3. Create at least two new features (and up to several if helpful) that could improve your 
model. (E.g., is_weekend, is_rainy_day from codesum) 
    • Explain why you created each feature and how it may influence product sales.

4. Build multiple predictive models (e.g., linear regression, decision tree or other 
interpretable model) to predict units sold is affected by weather-related features.
    ● You may choose to build a separate model for each product or include product 
      as a feature in a combined model.
    ● Focus on interpretability over complexity — models should help explain how 
      weather affects sales.

● If you build multiple models, compare them using simple metrics (e.g., R²,RMSE, MAE etc.) and explain which is most appropriate and why.
o Clearly state which model performs better and why (e.g., better generalisation, interpretability, performance on specific products).
● Interpret key model outputs:
o For Linear Regression, explain what each coefficient means in the context of sales (e.g., how much do units sold increase per 1°F rise in 
temperature?).
o For Decision Trees, include feature importance or a visualisation of the tree.


### 4.3. Section C: Interpretation & Visualization 
1. Write 3 short paragraphs (one per product), summarizing how weather impacts sales 
for each.
    ● Which weather variable has the strongest influence on sales?
    ● Use evidence from your model output (e.g., R² scores, coefficients, variable importance) to support your analysis.
    ● Is the relationship positive or negative?
Be concise but insightful. Your explanation should connect back to the real-world business context

2. Visualizations:
    Produce 3–6 visualizations that support your analysis and illustrate your 
    interpretations. These may include:
    ● Scatter plots showing correlation (e.g., temperature vs. sales)
    ● Line plots over time
    ● Bar charts grouped by weather events
    ● Optionally: embed or link to an interactive dashboard (e.g., via R Shiny or 
Tableau Public)
● All plots must:
  o Include titles, labels, and legends.
  o Be accompanied by 1–2 sentences explaining what the chart shows.

3. Business Recommendations: 
    Provide 3–6 actionable business recommendations based on your findings. These should:
    ● Be based on evidence from your model interpretations or visualisations.
    ● Be product-specific — at least one or two per product.
    ● Link to weather-based insights (e.g., “Increase stock of Product 9 in summer months due to temperature-driven demand rise.”)
    ● Be practical and show understanding 

Note: Good recommendations are specific, justified, and data-driven — not general or vague
The source code should be fully reproducible, e.g. another data scientist with access to the 
data could run the code from end-to-end to produce the results shown in the report/answer 
sheet. It should contain the following sections:

    ● Data Preparation (SQL): Join the weather and the station data, filter it to the top three best-selling products.
    ● Data Cleaning and Feature Creation (R): Check for data inconsistences and missing data, address as needed. Create any features needed for modelling. 
    ● Predictive Model(s) (R): Model or models which predict product sales based on weather features for the top three best selling products. You may also include non-weather 
      features if they improve model performance. Model(s) should focus on simplicity and interpretability rather than accuracy. Use data science best practice for model building, 
      such as a train-test-validate split.
    ● Model Outputs (R): For each model built, output the model summary and an appropriate performance measure (such as R2 or root mean square error) on a validation split at a minimum.       
      Optionally, include any additional outputs to support your written findings.
    ● Visualizations (R or dashboarding tool): source code for static visualizations or a programmed dashboard. If a non-programming tool such as Tableau was used, include a short paragraph on how you created the dashboard.


