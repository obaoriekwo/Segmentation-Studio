# What was fixed before this went to a recruiter

## The good news
`Analysis.ipynb` was already solid, real work: proper EDA, KMeans
clustering (elbow method + PCA), a Random Forest classifier with a real
train/test split, and honestly-measured results — **49.3% accuracy** on a
genuinely hard 4-class segmentation problem. The confusion matrix, feature
importance, and segment profile numbers in the notebook are all real and
were verified against `Train.csv` directly.

## The problem
The polished dashboard (`index.html` + `app.js`) was **not connected to
any of that real analysis**. Every chart used hardcoded placeholder
numbers, and several were directionally backwards from the real data —
e.g. Segment D was labeled the oldest, most-graduated, highest-spending
segment ("Affluent Senior"), when the real data shows D is actually the
**youngest**, **least-graduated**, and **lowest-spending** segment,
dominated by Healthcare professionals. The "Segment Predictor" tool also
claimed to mirror the trained Random Forest, but its rules didn't reflect
the real per-segment patterns at all.

The "Key Insights" cards also included **Channel** ("Social media",
"Mobile app", etc.) and **Priority** ("Value deals", "Premium", etc.)
fields with zero basis in the dataset — there's no channel or priority
column anywhere in `Train.csv`. These were pure invention.

## What changed
- `data.js` (new file) — every chart value is now computed directly from
  `Train.csv` (segment counts, age/work-exp/family-size means, gender/
  spending/graduation/marriage rates, age-group and profession
  breakdowns, all cross-checked against the notebook's own output).
- `app.js` — rewritten to read from `data.js` instead of hardcoded arrays.
- `index.html` — the profile cards and insight cards now show real
  numbers; the fabricated "Channel"/"Priority" fields were replaced with
  real facts (modal spending tier, married %, graduated %, top
  profession %); a short, honest model-performance note (49.3% accuracy,
  pointer to the notebook) was added to the Overview tab.
- The **Segment Predictor** now uses a nearest-centroid classifier built
  from the real per-segment averages (age, work experience, family size,
  graduation rate, marriage rate, spending level), standardized by each
  feature's real population spread. It's clearly labeled as a simplified
  in-browser approximation of the real analysis, not the trained Random
  Forest itself (that only runs in Python — see `Analysis.ipynb`).

## Bottom line
The real project — the notebook, the modeling, the 49.3% accuracy — was
worth showing to a recruiter as-is. The dashboard just wasn't telling the
same story as your own analysis. It does now.
