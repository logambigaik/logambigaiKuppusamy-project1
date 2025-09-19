### Justification:

#### Sales Distribution (Top-Selling Item Across All Stores)
Total records: 41,600 (daily sales per store)

Units sold per day (summary):

  Min: 0

  1st percentile: 0

  Median (50%): 0

  75th percentile: 50

  99th percentile: 129

  Max: 392

  Mean: ~24 units/day

  Std. deviation: ~36

✅ Justification with Your Data

Log Transform

Because the median is 0 and the 75th percentile is 50, the distribution is highly right-skewed.

A log transform is useful here: it reduces the effect of the tail (up to 392) and makes the differences between low and mid values more interpretable.

Percentile Trimming

99th percentile = 129 units, but maximum = 392 units.

That means the top 1% of observations are more than 3× larger than the 99th percentile.

Trimming at the 99th percentile would remove those rare extremes while preserving the bulk of the data.

Domain-Informed Threshold

No values >500, but still some extremely high days (200–392 units).

In practice, you might decide:

“>300 units/day” = promotional/bulk events → flag separately.

Keep ≤300 as “normal demand.”

👉 So for this dataset:

Log transform handles skewness.

Percentile trimming (e.g., capping at 129) prevents the top 1% from dominating.

Domain-informed rule (say, flag >300 units) provides a business check even if no 500+ values exist.
Percentile trimming (e.g., capping at 129) prevents the top 1% from dominating.
  
📄 Report-Style Justification

The sales data for the top-selling item shows a highly skewed distribution, with a median of 0 units/day, 75th percentile of 50 units, 99th percentile of 129 units, and a maximum of 392 units. This indicates that most days have low or no sales, while a few days exhibit unusually high sales volumes.

To handle this imbalance, several preprocessing approaches were applied:

Log Transformation:
A log1p transformation compresses the long right tail and stabilizes variance, making the distribution more symmetric. This is appropriate since the original data is zero-inflated and heavily skewed.

Percentile Trimming:
Values above the 99th percentile (>129 units) were trimmed, affecting ~401 rows (≈0.96% of data). This removes extreme high outliers while preserving 99% of the observations, ensuring that rare spikes do not dominate model training.

Domain-Informed Threshold:
From a business perspective, sales above 300 units/day are implausibly high for a single store-day. Only 1 such case (0.002% of data) was found, and flagging it provides a safeguard against data errors or extraordinary events (e.g., bulk purchases, promotions).

Together, these techniques ensure the dataset is robust for modeling: the log transform addresses skewness, percentile trimming removes statistical extremes, and domain thresholds incorporate business logic to handle anomalies.
