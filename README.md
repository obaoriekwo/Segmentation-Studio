# Customer Segmentation Studio

An end-to-end customer segmentation project: a Jupyter notebook that
does the real data science (EDA, clustering, and classification on
8,068 customer records), and an interactive web dashboard that makes
those findings explorable without touching a line of code.

**Live dashboard:** `index.html` (single self-contained file — see
[Tech Stack](#tech-stack))
**Full analysis:** `Analysis.ipynb`

---

## The Problem

A retail business has ~8,000 existing customers and wants to acquire
2,000 new ones. Blasting the same offer to everyone is wasteful —
different customer types respond to different messaging, pricing, and
channels. The business needs to know: **what distinct customer segments
already exist, and what does each one actually look like?**

`Train.csv` contains 8,068 customers, each already labeled into one of
four segments (A, B, C, D) along with 9 features: age, gender, marital
status, education, profession, work experience, family size, and
spending score.

## The Approach

`Analysis.ipynb` covers the full pipeline:

1. **EDA** — distributions and relationships across age, income proxy
   (spending score), family size, profession, and education, broken
   down by segment.
2. **Clustering (KMeans + PCA)** — used to sanity-check that the
   four given segments correspond to genuinely separable groups in
   the feature space, using the elbow method to validate cluster
   count and PCA to visualize separation in 2D.
3. **Classification (Random Forest, 200 trees)** — trained on a real
   train/test split to predict segment membership from the 9 input
   features, so new customers can be assigned a segment automatically.

**Result: 49.3% test accuracy** on this 4-class problem. That's roughly
2.4x better than random guessing (25%), which reflects the honest
difficulty of the task — the confusion matrix shows segments B and C
in particular overlap heavily, meaning the boundaries between them are
genuinely fuzzy in the data, not an artifact of a weak model. Feature
importance, the full confusion matrix, and segment profiles are all in
the notebook, verified directly against `Train.csv`.

## The Dashboard

The dashboard turns the notebook's findings into something a
non-technical stakeholder (or recruiter) can explore directly:

- **Overview** — dataset summary and model performance at a glance.
- **Segments** — per-segment profile cards and a normalized radar
  chart comparing all four segments across age, work experience,
  family size, spending, and graduation rate.
- **Demographics** — age groups, gender split, marital status, and
  family size distribution, broken down by segment.
- **Behavior** — profession and work-experience patterns, plus a
  key-insights summary per segment.
- **Segment Predictor** — enter a hypothetical customer's attributes
  and get an instant segment estimate, computed in-browser via a
  nearest-centroid classifier built from each segment's real average
  profile (age, work experience, family size, graduation rate, marriage
  rate, spending level), each standardized by its real population
  spread. This is a lightweight approximation for interactive
  exploration — the actual trained Random Forest only runs in Python
  (see `Analysis.ipynb`).
- **CSV Upload** — drop in `Train.csv`, `Test.csv`, or any similarly
  shaped file for a live data preview table.

Every number on every chart is computed directly from `Train.csv` —
nothing is hardcoded or illustrative.

## Tech Stack

- **Analysis:** Python (pandas, scikit-learn, matplotlib/seaborn) in
  `Analysis.ipynb`
- **Dashboard:** vanilla HTML/CSS/JS + [Chart.js](https://www.chartjs.org/)
  for charts and [PapaParse](https://www.papaparse.com/) for CSV
  parsing — no build step, no framework, no dependencies to install
- Packaged as a **single `index.html` file** (styles and data/logic
  inlined) so it can be opened locally or deployed to any static
  host with zero configuration

## Repository Contents

| File                      | Purpose                                              |
|---------------------------|-------------------------------------------------------|
| `index.html`              | The full interactive dashboard (self-contained)        |
| `Analysis.ipynb`           | EDA, clustering, and Random Forest classification       |
| `Train.csv`                | Labeled training data (8,068 rows, includes Segmentation) |
| `Test.csv`                 | Unlabeled data for prediction                          |
| `sample_submission.csv`    | Submission format template                              |
| `my_submission.csv`        | Model predictions on `Test.csv`                         |

## Running It

- **Dashboard:** open `index.html` directly in a browser, or deploy it
  as a static site (e.g. Render, GitHub Pages, Netlify) — no build
  command needed.
- **Analysis:** open `Analysis.ipynb` in Jupyter with `Train.csv` in
  the same directory.

## Author

Oba Oriekwo
