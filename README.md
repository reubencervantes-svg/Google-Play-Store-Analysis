# Google Play Store — Exploratory Data Analysis

**Finding market opportunity in the Google Play Store.**

An end-to-end exploratory data analysis of ~10,800 Google Play Store apps: loading and auditing messy scraped data, cleaning it honestly, and using SQL and a single scatter plot to find app categories with **high demand and low competition**.

**Author:** Reuben Cervantes

## The question

App stores are winner-take-all markets. A team deciding what kind of app to build wants categories with **high demand but low competition** — lots of installs per app, without a crowd of existing apps fighting for the same users. This project measures every category on those two axes and maps where the opening is.

## Key findings

- **Communication, Social, and Video Players** rank high on demand. Communication averages roughly **35M installs each from ~315 apps** — but with that many apps it scores as a high-demand *and* crowded market, not a clear opening.
- The categories that land in the **Opportunity** quadrant (high demand, below-median app count) are **Video Players, Entertainment, Weather, Shopping, and Maps & Navigation**.
- **Family** is the saturation warning: **1,876 apps** (the most of any category), yet the typical Family app is small — a ~3.3M average dragged up by a handful of hits. A newcomer there fights the most competitors for a shrinking slice of traffic.

## What's inside

The analysis runs in one notebook, in order:

1. **First look** — inspect the table with `head()`, `info()`, and `describe()`, watching the `dtype` column for problems.
2. **Data-quality audit** — count missing values (many apps have no `Rating` because nobody has rated them yet) and flag duplicate rows and duplicate app names left behind by scraping.
3. **The corrupted row** — filtering for impossible ratings above 5 surfaces one row where a missing `Category` shifted every field one column to the left.
4. **Cleaning** — done on a copy of the raw data so the original stays intact. Drops the corrupted row, removes exact duplicates, converts text columns like `"10,000+"` and `"$4.99"` into real numbers, rebuilds `Type` from `Price` so they can't contradict, and de-duplicates app names by keeping the record with the most reviews.
5. **Opportunity analysis (DuckDB SQL)** — aggregates each category by demand (average installs per app) and competition (number of apps), then uses window functions (`PERCENT_RANK`, `MEDIAN`) to score each category and label it *Opportunity*, *Saturated*, or *Mixed*.
6. **The opportunity map** — a scatter plot with competition on the x-axis, demand on the y-axis, and bubble size showing average rating.

A deliberate choice worth calling out: the ~1,400 missing `Rating` values are **kept, not imputed**. Filling them with the mean would fabricate quality signal that doesn't exist, so rows are simply excluded where a rating is required.

## Tools used

| Tool | Role in the project |
| --- | --- |
| **pandas / NumPy** | Loading, inspecting, auditing, and cleaning the data |
| **DuckDB** | SQL (aggregations + window functions) run directly against the DataFrame |
| **matplotlib / seaborn** | The opportunity-map scatter plot |
| **Jupyter Notebook** | Narrative + code + output in one document |

## How to run

```bash
# 1. clone the repo
git clone https://github.com/reubencervantes-svg/Google-Play-Store-Analysis.git
cd Google-Play-Store-Analysis

# 2. install dependencies
pip install -r requirements.txt

# 3. open the notebook
jupyter notebook Playstore_app_analysis.ipynb
```

Then **Run All**. The notebook reads `google_play_store_dataset.csv` from the repo root, so it runs top-to-bottom with no setup.

## Files

| File | Description |
| --- | --- |
| `Playstore_app_analysis.ipynb` | The full analysis, from raw load to the opportunity map |
| `google_play_store_dataset.csv` | The dataset (see source below) |
| `opportunity_map.png` | Exported chart shown above |
| `requirements.txt` | Python dependencies |

## Dataset

[Google Play Store Apps — Kaggle](https://www.kaggle.com/datasets/lava18/google-play-store-apps)
