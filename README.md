# FleetFit

FleetFit is a Streamlit application developed for the Universidad Complutense de Madrid Master’s thesis by Elias Alejandro Ventura. It compares aircraft models using monthly load-factor forecasts and historical operating support for a directional route.

This repository contains the approved V03 application and its fitted model, inference context and visual assets. The demonstration forecasts December 2025 from the November 2025 forecast origin. It does not train models or execute notebooks.

## Run locally

Use Python 3.12. From this repository’s root, follow the installation and verification instructions in [04_app/README.md](04_app/README.md).

## Deploy on Streamlit Community Cloud

Select this repository and branch, choose `04_app/app.py` as the entrypoint, and select Python 3.12. Application dependencies are declared in `04_app/requirements.txt` next to the entrypoint. The fitted model and inference context are included in `03_outputs/model`.

## Application scope

Predictions support comparisons; they do not certify operational feasibility or economic return. Support labels describe recent observations, not calibrated accuracy probabilities. See the application’s About page and [application documentation](04_app/README.md).

## Research materials

The complete thesis, executed notebooks, research data and reproducibility materials are delivered separately through Google Drive. They are not needed to run FleetFit.
