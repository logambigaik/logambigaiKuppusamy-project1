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
  
