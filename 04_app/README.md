# FleetFit standalone application

This app consumes the frozen artifacts in `../03_outputs/model`. It never runs notebooks or retrains. The demonstration forecasts December 2025 from the November 2025 origin.

From this package root (the directory containing `04_app` and `03_outputs`), create a clean Python 3.12 environment and install:

```bash
python3.12 -m venv .venv-fleetfit
source .venv-fleetfit/bin/activate
python -m pip install -r 04_app/requirements.lock
python -m pip check
python 04_app/validate_install.py
python -m unittest discover -s 04_app/tests -v
python -m streamlit run 04_app/app.py
```

`requirements.txt` pins direct dependencies. `requirements.lock` records the complete environment tested on macOS arm64 with Python 3.12. XGBoost 3.4.1 and scikit-learn 1.6.1 match the distributed fitted pipeline. Install the application requirements here; the historical requirements bundled in the frozen model directory are not the application installation entry point. Other operating systems have not been verified by this installation check.

## Distributed model and evidence

The source is `FINAL_PREDICTION_CONTRACT.json`, model `XGB_N300_LR0P05_D6`, 40 predictors, seed 42. The verifier checks the pipeline and inference-context SHA-256 values against that contract before loading the pipeline and producing reference forecasts. It also verifies the feature count and fitted model version. No reconstruction or replacement of the model is performed.

The stored December 2025 holdout predictions contain 32,067 rows and give MAE 0.08416656880019577 (8.4167 percentage points). This is the metric calculated from the exported holdout predictions; the app smoke test is an independent loading and inference check, not a new full holdout evaluation. The previous README's reconstructed-model MAE 0.084208 did not describe this distributed artifact and has been removed.

## Recommendations and support

The operated ranking sorts by recent departures, then observed recent months and predicted LF. The projected ranking sorts by observed recent months, support tier, predicted LF and recent departures, in that order. A candidate with higher LF can therefore appear below one with more recent observations. Each ranking displays its first five candidates; exploratory combinations are eligible for display, but an exploratory forecast is not a standard recommendation.

Application support labels are calculated by `_reliability_from_recent_support()` in `04_app/load_factor_recommender.py`, using observed activity for the directional route-aircraft combination during the twelve months ending at the forecast origin. The first matching rule applies:

| Runtime label | Visible label | Observed months | Departures |
|---|---|---:|---:|
| Standard | Strong support | 12 | at least 12 |
| Moderate | Established support | at least 6 | at least 12 |
| Low | Emerging support | at least 3 | at least 3 |
| Exploratory | Exploratory | otherwise | otherwise |

`03_outputs/model/FINAL_RELIABILITY_RULES.csv` documents descriptive error evidence grouped by total observed history for each directional route-aircraft combination. Despite its filename and its reliability-label and application-treatment columns, it is not the configuration executed by the app to assign support labels. Total history remains contextual metadata and does not determine these tiers.

The labels above are operational support categories, not calibrated probabilities of forecast accuracy or prediction intervals. The internal value for the final tier is `Exploratory — abstain from standard recommendation`; its visible label is `Exploratory`.

The app provides historical operators, route context, aircraft profiles and local visual assets. Airline logos are bundled locally in `assets/airlines`; carriers without a bundled logo use a readable code badge. Logo source URLs are recorded in `assets/airlines/sources.json`.

`TFM_MODEL_ARTIFACT_DIRECTORY` can point to an unchanged model directory for isolated testing. Both the app, verifier and test suite honor it.

## V02 presentation review

This isolated version improves responsive navigation, chart labels, layout spacing, aircraft photographs and route-map rendering across the international date line. The fitted artifacts, inference logic, candidate order, support thresholds and pinned dependencies are unchanged. The original application is preserved separately for comparison.

## V03 calendar-input correction

V03 converts MONTH_OF_YEAR, QUARTER_OF_YEAR and COVID_PERIOD to the integer-string categories fitted by the distributed encoder (for example, `12`, not `12.0`). Missing values retain the existing missing category. Other predictors, fitted artifacts, candidate filters, support thresholds and sorting rules are unchanged. Corrected LF values can change candidate positions under the existing sorting rules.

The installation reference forecasts were independently checked against V02 inputs with only those three calendar categories corrected. The acceptance suite now contains 15 tests. V02 is preserved separately.

The model exported by N02 §23 was fitted in §22 with a different categorical drop setting from the §19 holdout model. This app correction does not replace or retrain that frozen artifact and does not establish equivalence with the published holdout predictions. The holdout metrics above remain evidence for the stored evaluation predictions, not a new evaluation of V03.
