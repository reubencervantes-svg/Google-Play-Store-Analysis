

Readme · MD
# Google Play Store — Exploratory Data Analysis
 
Finding where the *opportunity* is in the Play Store: categories with high real demand but low competition. A full EDA workflow from messy scraped data to a clear, decision-ready chart.
 
**Author:** *Reuben Cervantes*

**Stack:** Python · pandas · DuckDB (SQL) · matplotlib · seaborn
 
---
 
## The question
 
**Which categories actually have room to win?** — High typical demand, but not already crammed with competitors?
 
## Approach
 
1. **Audit** — Inspected 10,841 rows for missing values, duplicates, and impossible records.
2. **Clean** — Removed a corrupted row (a shifted column produced a rating of 19 on a 1–5 scale), dropped exact and app-name duplicates, and converted text fields (`"10,000+"`, `"$4.99"`) into real numeric types. Ran integrity checks (e.g. *reviews can't exceed installs*) and documented every decision. **10.9% of rows removed.**
3. **Analyze** — Scored each category on two axes using **DuckDB SQL** 
   - **Demand** — Average *vs.* median installs. When average ≫ median, the category is propped up by a few giants, not broad demand.
   - **Competition** — Number of apps in the category.
4. **Visualize** — A single scatter "opportunity map" (competition vs. typical demand) with quadrant labeling.

## Key insight
 
The **median** is what separates real opportunity from illusion. **Communication, Social, and Tools** look huge on *average* installs but collapse on the median — their pull comes from a handful of billion-install apps, not demand a newcomer can tap. The genuine openings (**Weather, Entertainment, Video Players, Shopping, Education**) pair healthy typical demand with relatively few rivals. **Family** is the saturation warning: the most apps of any category, yet a low median payoff.

## What this project demonstrates
 
- Data cleaning with **documented, defensible decisions** (kept missing ratings rather than fabricating them; kept "impossible" rows once bucketing was understood).
- Comfort with **both pandas and SQL** for the same analysis.
- **Statistical judgment** — using median vs. mean to avoid a misleading conclusion.
- Honest framing: results are treated as **hypotheses to validate, not recommendations**.

## Data
 
Google Play Store Apps — a **2018 snapshot** (10,841 rows, 13 columns). The specific categories reflect that market; the *method* is what transfers to current data.

## Files

| File | Description |
| --- | --- |
| `Playstore_app_analysis.ipynb` | The full analysis, from raw load to the opportunity map |
| `google_play_store_dataset.csv` | The dataset (see source below) |
| `opportunity_map.png` | Exported chart shown above |
| `requirements.txt` | Python dependencies |

## Dataset

[Google Play Store Apps — Kaggle](https://www.kaggle.com/datasets/lava18/google-play-store-apps)
