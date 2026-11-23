# How to interpret the results

The numeric results must be read from the generated CSVs—not invented. Primary selection metric: **mean nested CV Macro-F1**. Higher is better, but class prevalence, CV variability, and Dummy baselines should be discussed.

## Presentation materials

- Model screening: `results/figures/model_selection_6_models.png`
- 8 comparisons: `accuracy_comparison.png`, `macro_f1_comparison.png`
- Overfitting: `training_vs_validation.png`
- Final selected models: `confusion_matrix_3class.png`, `confusion_matrix_5class.png`
- Interpretability: `feature_importance_3class.png`, `feature_importance_5class.png`
- Complexity: `training_time_comparison.png`, `model_complexity.csv`

**Limitations:** only 300 self-reported survey cases; class imbalance; engineered 3-class target; prior exploratory attention to the same 20% holdout; target-proximal expanded features. Scores are not clinically validated. Related published approaches should be discussed separately in the written report, accounting for differing targets and datasets.
