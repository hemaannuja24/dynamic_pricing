## Dynamic Pricing for Urban Parking Lots

A fully‑worked Google Colab notebook that builds an "adaptive, real‑time pricing engine" for 14 urban parking lots.It ingests half‑hourly sensor data, performs end‑to‑end EDA, engineers features, and streams live prices via the "Pathway" data‑streaming framework.


### Tech Stack
| Layer | Libraries / Tools | Purpose |
|-------|------------------|---------|
| Data wrangling | "Pandas", "NumPy" | load, clean, winsorise outliers |
| Visualisation | "Seaborn", "Matplotlib", "Bokeh" | static & interactive EDA plots |
| Real‑time engine | "Pathway" | table‑stream, UDF pricing, console sink |
| Notebook env. | "Google Colab" | reproducible cloud runtime |

---

### Architecture Diagram (Mermaid)

mermaid
graph TD
    A[dataset.csv<br>raw sensor data] -->|pandas read_csv| B(Clean & EDA)
    B -->|feature engineering| C[DataFrame with features]
    C -->|table_from_pandas| D[Pathway Table]
    D -->|@pw.udf Model 1/2/3| E[Price Column]
    E -->|pw.io.subscribe| F(Console / API Sink)


## Workflow Details
1.Data ingestion & EDA
--Missing/duplicate check, histograms, bar charts, correlation heat‑map with labels for easy interpretation. 
--Outliers detected by IQR, removed by winsorisation using DataFrame.clip. 
2.Feature engineering
OccupancyRate, IsSpecialDay, rounded lat/long (LatRound, LongRound) to group competitor lots.
3.Pricing models
--Model 1 (Linear) – price = $10 ± 0.5 × base × OccupancyRate.
--Model 2 (Demand) – adds queue length, traffic, event flag; smoothed with tanh.
--Model 3 (Competitive) – boosts/discounts Model 2 output by comparing to nearby‐lot average price.
-Each model caps prices to 0.5×–2× base using a shared _bound() helper.
4.Real‑time streaming with Pathway
--Cleaned DataFrame → pw.debug.table_from_pandas. 
--Stateless UDFs compute the chosen model’s price per row.
--pw.io.subscribe prints inserts; every new row yields a fresh decision stream.

## Repository Structure
dynamic_pricing.ipynb   # Colab notebook (run top‑to‑bottom)
final_report.pdf        # Detailed report (methodology & findings)
dataset.csv             # Source data
README.md               # (this file)
