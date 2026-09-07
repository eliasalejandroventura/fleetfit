# Clean installation check — 7 September 2026

Verified on macOS arm64 with Python 3.12.14. A new empty virtual environment was created and populated directly from `requirements.lock`. Installation succeeded and `pip check` reported no broken requirements. This verifies the documented pinned installation on this platform.

- 12/12 application acceptance tests passed in the fresh environment, including the twelve-route matrix, profiles, airport search and Streamlit interaction tests.
- The installation verifier matched the model and context hashes in the frozen prediction contract, confirmed 40 predictors, and reproduced three reference forecasts within an absolute LF tolerance of 1e-7.
- A local Streamlit server using the same dependency versions returned HTTP 200 at `/_stcore/health`.
- The frozen N01/N02 sources and exports, trained model and original report files were not modified.

The encoder emitted its existing unknown-category warnings: those categories are encoded as zeros according to the fitted pipeline. The warnings did not fail inference and were not suppressed by an application change. This check does not rerun training, RFE or the full holdout evaluation. Other platforms have not been tested.

The `verification/` directory preserves the installation log, dependency check, acceptance test log and reference predictions. Run the commands in README to repeat the checks.
