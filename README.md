# Google Play Store — Exploratory Data Analysis

**Finding market opportunity in the Google Play Store.**

An end-to-end exploratory data analysis of ~10,800 Google Play Store apps: loading and auditing messy scraped data, cleaning it honestly, and using SQL and a single scatter plot to find app categories with high demand and low competition.

**Author:** Reuben Cervantes

---

## The question

App stores are winner-take-all markets. A team deciding what kind of app to build wants categories with **high demand but low competition** — lots of installs per app, without a crowd of existing apps fighting for the same users. This project measures every category on those two axes and maps where the opening is.

## What's inside

The analysis runs in one notebook, in order:

1. **First look** — inspect the table with `head()`, `info()`, and `describe()`, watching the `dtype` column for problems.
2. **Data-quality audit** — count missing values (many apps have no `Rating` because nobody has rated them yet) and flag duplicate rows and duplicate app names left behind by scraping.
3. **The corrupted row** — filtering for impossible ratings above 5 surfaces one row where a missing `Category` shifted every field one column to the left.
4. **Cleaning** — done on a copy of the raw data so the original stays intact. Drops the corrupted row, removes exact duplicates, converts text columns like `"10,000+"` and `"$4.99"` into real numbers, rebuilds `Type` from `Price` so they can't contradict, and de-duplicates app names by keeping the record with the most reviews.
5. **Opportunity analysis (DuckDB SQL)** — aggregates each category by demand (average installs per app) and competition (number of apps), then uses window functions (`PERCENT_RANK`, `MEDIAN`) to score each category and label it *Opportunity*, *Saturated*, or *Mixed*.
6. **The opportunity map** — a scatter plot with competition on the x-axis, demand on the y-axis, and bubble size showing average rating.

A deliberate choice worth calling out: the ~1,400 missing `Rating` values are **kept, not imputed**. Filling them with the mean would fabricate quality signal that doesn't exist, so rows are simply excluded where a rating is required.

## Key findings

- **Communication, Social, and Video Players** sit high on demand with relatively few apps — concentrated, winner-take-all markets. Communication averages roughly **35M installs each from only ~315 apps**.
- **Family** is the saturation warning: **1,876 apps** (the most of any category), yet the typical Family app is small — a ~3.3M average dragged up by a handful of hits. A newcomer there fights the most competitors for a shrinking slice of traffic.

## Tools used

- **Python** — pandas, NumPy
- **DuckDB** — SQL (including window functions) run directly against the DataFrame
- **matplotlib** and **seaborn** — visualization
- **Jupyter Notebook**

## Dataset

https://www.kaggle.com/datasets/lava18/google-play-store-apps
