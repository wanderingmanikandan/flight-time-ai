# Data sources and reproducibility

Source inspection date: 5 October 2026.

## 1. The deliberately simple dataset

**Origin:** generated classroom examples, not observed aircraft data.

**Generation:** distance_km = 200, 400, ..., 4000. For every distance,

`air_time_min = distance_km / 600 * 60`

600 km/h is an assumed constant ground speed for the lesson, not an empirical claim about any specific aircraft. The model is taught from the resulting distance/time pairs. Its prediction routine uses the fitted coefficients, not a lookup table or a hidden flight-time answer formula. For this ideal case, the physical time = distance/speed formula would be sufficient without machine learning; fitting a model is an educational demonstration.

20 rows total: 16 training rows, 4 test rows. There is no noise. Zero test error is expected and is not a claim of operational aircraft performance.

## 2. The real flight records

**Accessible source:** OpenIntro, `nycflights`.

Dataset page and original downloads:
https://www.openintro.org/data/index.php?data=nycflights

Direct CSV download:
https://www.openintro.org/data/csv/nycflights.csv

Readable tab-delimited version used for extracting the app's records:
https://www.openintro.org/data/tab-delimited/nycflights.txt

OpenIntro package documentation:
https://openintrostat.github.io/openintro/reference/nycflights.html

Original field definitions and BTS attribution:
https://nycflights13.tidyverse.org/reference/flights.html

OpenIntro's data page describes a random sample of 32,735 flights departing JFK, LGA, or EWR in 2013. It credits Hadley Wickham (2014), nycflights13. The parent package's documentation identifies the U.S. Bureau of Transportation Statistics as the original source. The app is NOT using all 32,735 records or the full parent dataset.

### Exact extract

The first 100 data rows of the publicly readable OpenIntro TSV were inspected and their `distance` and `air_time` fields extracted, retaining 1-based source data-row numbers. Header row is not counted. The original source file was not bundled in full.

No row in this 100-row extract has missing, zero, or negative values in the two selected fields. All 100 rows are retained. No outlier trimming, averaging, smoothing, random noise, or modification of recorded air-time targets was performed. Different flights can share the same distance or the same distance/time pair; these are not silently deduplicated.

Input: original `distance` = airport-to-airport distance in statute miles.
Converted input: `distance_km = distance * 1.609344`.
Output: original `air_time` = recorded time spent in the air, in minutes.

The browser uses the full-precision multiplication result. The UI rounds kilometres for readability. The CSV records kilometres to six decimal places, which retains this conversion's decimal precision for integer-mile source values. The training/test labels and original row numbers are included for reproducibility. `source_data_row`, `distance_miles`, and `source_url` are audit metadata, not additional model features.

Reference checks:
- Source data row 1: 2,475 miles, 313 minutes.
- Source data row 3: 2,475 miles, 376 minutes. The same distance can have a different time.
- Source data row 39: 94 miles, 25 minutes.
- Source data row 100: 764 miles, 130 minutes.

Recorded airborne time includes airborne phases. It is not cruise-only time. The app does not model take-off/climb/cruise/descent separately. It does not add airport waiting, taxiing, boarding, or recorded departure-delay minutes. The input distance is an airport-pair distance, not a measured aircraft trajectory length.

## 3. Train/test rule and learning method

Both lessons use the same deterministic routine implemented in the HTML:

1. Keep one minimum-distance and one maximum-distance row in training. This uses only the input values, not their time targets.
2. Create a list of other row indices in source order.
3. Apply Fisher-Yates shuffling with a 32-bit linear congruential generator, seed 42: `seed = (1664525 * seed + 1013904223) mod 2^32`.
4. Assign the first `floor(0.2 * number_of_rows)` shuffled candidate rows to the test set.
5. Use all remaining rows to fit the line. Repeat clicks never redraw the split.

A row cannot belong to both groups. This is an instructional row-level split, not a time-based split or a held-out-route evaluation. Different flights on the same route can occur on both sides. Retaining endpoint rows in training is an explicit instructional design choice, not a claim of an unbiased operational evaluation.

The model is one-feature ordinary least-squares linear regression with an intercept:

`predicted_time = intercept + slope * distance_km`

`slope = sum((x - mean_x) * (y - mean_y)) / sum((x - mean_x)^2)`

`intercept = mean_y - slope * mean_x`

All means and sums for fitting use training rows only. No scaling, hyperparameter search, neural network, or external library is used. The teacher disclosure also compares against a constant training-mean baseline.

Method reference (not a library dependency):
https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html

Test metric:
`MAE = mean(abs(recorded_test_time - predicted_test_time))`.

## 4. Reproducible numerical checks

Simple lesson:
- Slope: 0.1 min/km; intercept: 0 min.
- Held-out MAE: 0 min.
- Predictions: 500 km -> 50 min; 1,000 km -> 100 min; 2,000 km -> 200 min.

Real lesson:
- Slope: 0.0785656112366627 min/km.
- Intercept: 18.50202230941524 min.
- Held-out MAE: 9.085487131929145 min (displayed as 9.1).
- Training-mean baseline MAE: 57.01 min.
- At 500 km: 57.7848279277 min (displayed as 58).
- At 1,000 km: 97.0676335461 min (displayed as 97).
- At 2,000 km: 175.6332447827 min (displayed as 176).

These are calculations on the included extract and split, not statistics published by OpenIntro or BTS. A fitted intercept is not a separately measured ground-time or take-off-time allowance. New inputs outside the training-distance range are rejected; this does not imply that every in-range prediction is reliable.

## 5. Licenses and attribution

The current OpenIntro R package DESCRIPTION declares GPL-3:
https://github.com/OpenIntroStat/openintro/blob/main/DESCRIPTION

Original package and credit:
https://github.com/tidyverse/nycflights13

This package distributes its code and accompanying material under GNU GPL version 3; the license text is included. Retain the cited provenance and the license with redistributed copies. OpenIntro website-wide licensing statements include exceptions; the package-specific declaration is identified above rather than asserting that all OpenIntro materials use the same license.

This independent educational adaptation is not an official OpenIntro, BTS, or airline product. It is not suitable for operating an aircraft, estimating a real booking, or making safety-critical decisions.
