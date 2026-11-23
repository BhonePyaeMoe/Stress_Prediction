# Student Stress Prediction — Reproducible Machine Learning Project

This repository contains **real** CSV survey records, three exploratory data analysis (EDA) notebooks, data-quality/preprocessing work, a six-algorithm model screening, two shortlisted models, eight tuned comparison configurations, final testing on reserved students, and a prediction demo notebook. **No generated/synthetic student cases or fabricated model scores.**

## Inputs

| Dataset | Rows | Columns | Role |
|---|---:|---:|---|
| 1: University Student Stress | 3,000 | 18 | EDA, compare source suitability; constructed stress target is a concern |
| 2: Academic Stress, Sleep Quality and Lifestyle Factors | 300 | 31 | **Final model-training dataset** |
| 3: Student Insomnia and Educational Outcomes | 996 | 16 | EDA, compare source suitability; severely imbalanced stress target |

Dataset 2 original outcome: `Current_stress_level` in 1–5. The separate three-class task maps 1–2 = Low, 3 = Medium, 4–5 = High. Predictor sets contain 7 original factors and the expanded 15 (as listed in `data/processed/experiment_config.json`). The raw datasets are preserved unchanged in `data/raw/`.

## Setup and reproduction

Python 3.11+ recommended (scikit-learn >= 1.4 for `OneHotEncoder(sparse_output=False)`). From the project root:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open notebooks **from the `notebooks/` folder**, then run in this sequence:

1. `01_dataset1_eda.ipynb` to `03_dataset3_eda.ipynb` — research/EDA
2. `04_preprocessing.ipynb` — audit and stable 80/20 split
3. `05_model_selection.ipynb` — compare 6 candidates (plus Dummy) with Stratified 5-Fold CV; select 1 simple and 1 complex
4. `06_hyperparameter_tuning.ipynb` — tune the two candidates for 7/15 inputs and 3/5 classes; evaluate all 8 with nested CV; choose the demo configuration for each task using **nested Macro-F1 and a 0.02 simplicity tolerance**
5. `07_testing_and_comparison.ipynb` — holdout evaluation on 60 students, comparison tables, confusion matrices, permutation importance and model saving
6. `demo.ipynb` in the project root — load saved fitted pipelines and demonstrate real test-row predictions

Note: `demo.ipynb` uses paths relative to the **project root**, unlike the other notebooks which use paths relative to `notebooks/`. Run them from their respective directories as shown. If using VS Code/Jupyter with another working directory, change the notebook working directory accordingly.

## Design choices and caveats

- Six candidates: Logistic Regression, Decision Tree, Random Forest, RBF SVM, KNN, Gradient Boosting; baseline = `DummyClassifier(strategy='most_frequent')`.
- The six-algorithm screening uses **original 7 inputs, 3 classes**. One simple and one complex algorithm are selected from their groups using mean 5-fold Macro-F1. This is not a proof they dominate every possible combination.
- For each of the eight configurations (two finalists × two feature sets × two target definitions), `GridSearchCV` tunes on training records only. **Nested outer 5-fold / inner 2-fold** scores estimate tuned model performance more honestly than grid-search `best_score_` alone.
- We select one demo configuration **per target** based on nested CV Macro-F1. If an alternative is within **0.02** of the highest, we prioritize fewer inputs then the simpler algorithm. The tolerance is practical, not a significance test. These choices are made before any final heldout metrics are calculated in Notebook 07.
- Single preselected 80/20 test membership (240/60) stratified on original five levels; test records excluded from fitting and CV. However, this same dataset/test split was examined in previous exploratory work, and feature decisions were informed by earlier results. **Do not describe the final test scores as fully independent or externally validated.**
- One-hot encoding and numerical scaling occur **inside** scikit-learn Pipelines fitted within CV folds, avoiding preprocessing leakage.
- Predictor features such as `Financial_stress_level` and `Overwhelmed_by_academic_workload` may be close to the subjective target (proxy leakage risk), and this must be discussed.
- Predictions are investigational educational estimates, **not stress diagnoses**. Low or below-baseline performance must be reported candidly.
- Existing-published-work comparisons are intentionally left to the final written report, as planned by the project team.

## Results

- `results/tables/` contains original CV scores, per-fold scores, GridSearchCV selections, tuned nested CV scores, holdout metrics, actual heldout predictions, fit-time measurements and permutation importance.
- `results/figures/` contains **actual plots**, not the illustrative mockups used in earlier planning.
- `models/selected_algorithms.json` records the first-stage shortlist. `models/demo_model_selection.json` records the 3/5-class demo choices. `models/best_model_3class.pkl` and `best_model_5class.pkl` are fitted preprocess-and-classify Pipelines.

### Security

`.pkl` files (joblib/Pickle) can execute arbitrary code when loaded. **Only load artifacts produced by your own trusted project code**; never load unknown model files from the internet.
