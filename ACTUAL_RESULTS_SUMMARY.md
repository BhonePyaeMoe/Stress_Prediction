# Actual Experimental Results — Dataset 2 (300 students)

All scores below were produced by executing the Jupyter notebooks against the supplied **real CSV**. No synthetic or placeholder predictions were used.

## Six-algorithm screening

All six candidates were evaluated on **7 features, 3 classes**, with Stratified **5-Fold** cross-validation on the 240 training records. The most-frequent Dummy baseline was evaluated too.

| Algorithm | Group | Mean CV Macro-F1 | Mean CV accuracy | Training accuracy |
|---|---|---:|---:|---:|
| Gradient Boosting | complex | 0.308 | 40.8% | 78.3% |
| Decision Tree | simple | 0.307 | 35.4% | 85.3% |
| Logistic Regression | simple | 0.300 | 44.2% | 56.0% |
| KNN | complex | 0.295 | 39.2% | 60.7% |
| Random Forest | complex | 0.288 | 38.3% | 85.3% |
| SVM (RBF) | complex | 0.268 | 49.6% | 60.2% |
| Dummy baseline | baseline | 0.223 | 50.4% | 50.4% |

**Simple finalist:** Decision Tree (CV Macro-F1 0.307).
**Complex finalist:** Gradient Boosting (CV Macro-F1 0.308).

## Eight tuned comparisons

GridSearchCV was fitted on training cases; the following performance estimates are from **5-fold outer / 2-fold inner nested CV** (training cases only).

| Target | Features | Model | Nested CV Macro-F1 | Nested CV accuracy | Nested CV balanced accuracy |
|---|---:|---|---:|---:|---:|
| 3class | Baseline | Dummy (most frequent) | 0.223 | 50.4% | 33.3% |
| 3class | 7 | Gradient Boosting | 0.270 | 46.2% | 32.8% |
| 3class | 7 | Decision Tree | 0.313 | 39.2% | 32.7% |
| 3class | 15 | Gradient Boosting | 0.302 | 42.5% | 32.2% |
| 3class | 15 | Decision Tree | 0.265 | 38.3% | 30.6% |
| 5class | Baseline | Dummy (most frequent) | 0.104 | 35.0% | 20.0% |
| 5class | 7 | Gradient Boosting | 0.166 | 27.9% | 19.0% |
| 5class | 7 | Decision Tree | 0.126 | 22.9% | 15.1% |
| 5class | 15 | Gradient Boosting | 0.169 | 27.5% | 19.5% |
| 5class | 15 | Decision Tree | 0.203 | 28.7% | 21.7% |

## Demo model selections (chosen before final holdout evaluation)

Predefined preference: among configurations within **0.02** of the best nested CV Macro-F1 for a given target, prefer fewer features, then the simpler algorithm. This is a pragmatic tolerance, not a statistical test.

- **3class:** Decision Tree, **7 features**; nested CV Macro-F1 **0.313**; best parameters: `{'model__max_depth': None, 'model__min_samples_leaf': 3}`.
- **5class:** Decision Tree, **15 features**; nested CV Macro-F1 **0.203**; best parameters: `{'model__max_depth': None, 'model__min_samples_leaf': 3}`.

## Reserved 20% test set (60 actual student records)

| Task | Model | Correct / 60 | Accuracy | Macro-F1 | Balanced accuracy |
|---|---|---:|---:|---:|---:|
| 3class | Decision Tree | 25/60 | 41.7% | 0.339 | 0.349 |
| 3class | Dummy (most frequent) | 30/60 | 50.0% | 0.222 | 0.333 |
| 5class | Decision Tree | 12/60 | 20.0% | 0.131 | 0.139 |
| 5class | Dummy (most frequent) | 21/60 | 35.0% | 0.104 | 0.200 |

**Critical result:** The selected models do **not** beat the most-frequent Dummy classifier in overall test accuracy for either classification task. Three-class selected model: 41.7% vs baseline 50.0%; five-class selected model: 20.0% vs baseline 35.0%. Both selected models improve test Macro-F1 slightly over their baselines, but there is insufficient evidence to offer reliable individual stress assessments.

## Interpretation and validity notes

- Training scores often greatly exceed validation scores; possible overfitting and poor generalization.
- Only 300 self-reported survey responses; the class labels may be noisy and the 5-class task is challenging.
- The 15-feature set includes measures (financial stress, overwhelmed by academic workload) close to subjective stress, requiring careful proxy leakage discussion.
- **Previous exploratory experiments looked at the same dataset/test split.** Even though this run selected models without test-set input, the final test scores are not guaranteed to be a fully independent/unbiased evaluation.
- We did not use synthetic data, resample the dataset, or delete students to increase reported accuracy.
- Three-class accuracy is not directly comparable to five-class accuracy. Each has its own majority baseline.
- Related research comparisons should be written separately in the final report by the project team.
