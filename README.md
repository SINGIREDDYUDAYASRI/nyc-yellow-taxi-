# NYC Yellow Taxi – Exploratory Data Analysis

Exploratory data analysis of NYC Yellow Taxi trip records (March 2016), focused on data cleaning, fare-distance relationships, hourly demand/revenue patterns, and payment behavior — aimed at surfacing operational insights for fleet management and revenue optimization.

## Dataset

- **Source:** NYC Yellow Taxi trip data — `yellow_tripdata_2016-03.csv`
- **Raw size:** 12,210,952 rows × 19 columns
- **Key fields:** pickup/dropoff timestamps, trip distance, fare amount, payment type, vendor ID

## Data Cleaning

The raw data required several cleaning steps before analysis:

1. **Datetime parsing** — converted `tpep_pickup_datetime` and `tpep_dropoff_datetime` to proper datetime objects.
2. **Trip duration** — engineered `trip_duration_minutes` from pickup/dropoff timestamps; removed negative durations and trips longer than 24 hours (1,440 minutes).
3. **Trip distance & fare filtering** — removed records with zero/negative trip distance, non-positive fares, and extreme fare outliers (≥ $500).
4. **Distance outliers** — capped trips at under 100 miles based on percentile analysis (90th/95th/99th).
5. **Fare-per-mile check** — engineered `fare_per_mile` to sanity-check pricing consistency and filtered out near-zero-distance trips (< 0.1 miles) that distorted the ratio.

After cleaning, the dataset retained **12,133,587 rows (~99% of original data)**.

## Feature Engineering

- `trip_duration_minutes` — derived from pickup/dropoff timestamps
- `fare_per_mile` — fare amount normalized by trip distance
- `pickup_hour` — extracted from pickup timestamp, used for time-of-day analysis

## Key Findings

- **Fare–distance relationship:** Trip distance and fare amount are strongly correlated (**r = 0.946**), confirming distance-based pricing integrity.
- **Peak revenue hours:** Evening hours (6 PM–11 PM) generate the highest per-trip fares, with **6 PM topping the list (~$5.98 average fare)**, followed by 7 PM and 9 PM.
- **Payment methods:** Credit cards account for **~67% of trips** and **~69% of total revenue**, with cash making up most of the remainder (~33% of trips, ~30% of revenue).
- **Demand patterns:** Trip volume and revenue both show clear hourly cyclicality, with late-night hours (2 AM–5 AM) seeing the lowest activity.

## Visualizations

- Distribution plots: trip duration, trip distance, fare amount
- Scatter plot: trip distance vs. fare amount (linear relationship)
- Bar chart: trip volume by hour of day
- Line charts: total revenue by hour, average fare by hour, revenue vs. average fare (dual-axis)
- Bar charts: payment method distribution (by trip share and by revenue share)

## Tech Stack

- **Python** — Pandas, NumPy
- **Visualization** — Matplotlib, Seaborn

## Project Structure

```
nyc-taxi-eda/
├── nyc_taxi_.ipynb      # Main analysis notebook
└── README.md            # Project documentation
```

## How to Run

1. Clone the repository and install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn
   ```
2. Download the NYC Yellow Taxi trip data (March 2016) from the [NYC TLC Trip Record Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) and update the file path in the notebook.
3. Run `nyc_taxi_.ipynb` cell by cell to reproduce the cleaning, analysis, and visualizations.

## Business Impact

The analysis supports fleet management and revenue optimization by identifying high-revenue time windows for driver allocation, confirming pricing integrity through the distance-fare correlation, and highlighting payment method trends relevant to transaction processing strategy.
