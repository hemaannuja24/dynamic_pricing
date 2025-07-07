# Dynamic Pricing for Urban Parking Lots

This project implements a real-time, adaptive pricing engine for city parking lots based on occupancy and environmental data.

## 🔧 Technologies Used
- Python (Google Colab)
- Pandas, NumPy, Seaborn, Matplotlib, Bokeh
- Pathway (for real-time streaming)

## Project Components

### 1. Data Overview
- Dataset includes occupancy, queue length, traffic, GPS coordinates, and time-stamps for ~14 parking lots over several weeks.

### 2. EDA (Exploratory Data Analysis)
- Checked for missing values and duplicates
- Univariate analysis with histograms and bar charts
- Correlation heatmap with annotations
- Outlier detection via boxplots (excluding IDs)
- Outlier treatment via winsorisation (1st–99th percentile)

### 3. Feature Engineering
- Derived `OccupancyRate`, `IsSpecialDay`, `LatRound`, and `LongRound` for modeling.

### 4. Pricing Models
- **Model 1 (Linear):** Pricing based on OccupancyRate.
- **Model 2 (Demand):** Adds traffic, queue length, and event impact.
- **Model 3 (Competitive):** Adjusts price based on similar nearby lots.

### 5. Real-Time Pricing (Pathway)
- Data is streamed row-by-row using `pw.debug.table_from_pandas`.
- Prices are computed using the selected model (Model 1, 2, or 3).
- Output is streamed with `pw.io.subscribe` and printed to console.

## How to Run
1. Open in Google Colab
2. Run pip install cell: `!pip install pathway bokeh seaborn`
3. Execute cells in order (EDA ➝ Modeling ➝ Streaming)

## Files
- `dynamic_pricing.ipynb`: Main notebook
- `README.txt`: This file
- `final_report.pdf`: Analysis + explanation of approach
