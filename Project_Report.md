# Fake News Detection — Project Report

**Industry:** Media & Publishing  |  **Type:** ML Classification (NLP)  |  **Author:** _your name_  |  **Date:** _date_

> Sections marked **[FILL IN]** need numbers/observations from your run of `notebooks/Fake_News_Detection.ipynb` on the full Kaggle dataset. No results are pre-filled so that nothing in this report is invented.

## 1. Executive Summary
**[FILL IN]** 3–4 sentences: the problem, best model, its test F1/recall, the recommended threshold policy, and the main limitation.

## 2. Problem Statement & Objectives
Build a classifier that labels articles Real or Fake from headline and body text, compare several algorithms, explain what separates the classes, and advise how to deploy the model to flag articles before publication.

## 3. Dataset
Source: Kaggle *Fake News Detection Datasets* (`Fake.csv`, `True.csv`). Fields: title, text, subject, date, label.
- Rows / columns: **[FILL IN]**
- Class balance: **[FILL IN]**
- Missing values / duplicates removed: **[FILL IN]**

## 4. Statistical Analysis
Descriptive statistics on derived numeric features (characters, words per article, words per title).

| Feature | Mean | Median | Mode | Std | Min | Max | Q1 | Q3 | IQR |
|---|---|---|---|---|---|---|---|---|---|
| text_words | | | | | | | | | |
| text_chars | | | | | | | | | |
| title_words | | | | | | | | | |

**Business interpretation [FILL IN]:** Is the length distribution skewed (mean vs median)? Do fake and real articles differ in typical length/spread? What does the correlation heatmap imply for feature selection?

## 5. Exploratory Data Analysis
Figures are saved in `images/`.
- Class distribution — `class_subject_distribution.png`
- Length distribution — `length_distribution.png`
- Subject vs label — `subject_vs_label.png`
- Top words per class — `top_words.png`
- Correlation heatmap / pairplot — `correlation_heatmap.png`, `pairplot.png`

**Key insights [FILL IN]:** class balance; whether subject categories overlap between classes (and why that makes `subject` unsafe as a feature); distinctive vocabulary per class; length differences.

## 6. Preprocessing
Missing/empty text handling, duplicate removal, label encoding (Fake = 1), 99th-percentile cap on article length, text cleaning (URLs, e-mails, punctuation, digits, lowercase), removal of news-agency datelines, stratified 80/20 split, TF-IDF fitted on training data only.

## 7. Feature Engineering
| Feature | Why it may help |
|---|---|
| Exclamation / question counts | Sensational tone is common in fabricated content |
| Caps ratio, ALL-CAPS word ratio, title caps ratio | Emphasis and clickbait style |
| Average word length, digit ratio | Formality and presence of figures |
| Quote count | Real reporting attributes statements |
| Title / body length | Structural differences |
| TF-IDF (1–2 grams) | Captures vocabulary and phrasing |

**Observed direction of each feature [FILL IN from the notebook's class-mean table].**

## 8. Models and Results
Models: Logistic Regression, Decision Tree, Random Forest, AdaBoost, KNN (cosine).

| Model | Train Acc | Test Acc | Precision | Recall | F1 | ROC-AUC | Train–Test gap |
|---|---|---|---|---|---|---|---|
| Logistic Regression | | | | | | | |
| Decision Tree | | | | | | | |
| Random Forest | | | | | | | |
| AdaBoost | | | | | | | |
| KNN | | | | | | | n/a |

Confusion matrices: `images/confusion_matrices.png`. 5-fold CV for the top three models: **[FILL IN]**.

| Model | Strengths | Weaknesses |
|---|---|---|
| Logistic Regression | Fast, strong on sparse text, interpretable | Linear only |
| Decision Tree | Easy to explain | Overfits on high-dimensional text |
| Random Forest | Robust, non-linear | Heavy, hard to interpret |
| AdaBoost | Focuses on hard cases | Noise-sensitive, slower |
| KNN | Simple, no training | Slow inference, weak in high dimensions |

## 9. Evaluation Metric Choice
Fake is the positive class. **Recall** measures fake articles caught (a miss lets misinformation through); **precision** measures how many flags are correct (a false alarm delays a legitimate article). **F1** balances them. For pre-publication flagging with human review, recall is prioritised while keeping precision high enough for editors to trust the tool.

## 10. Validation and Model Selection
**[FILL IN]** Compare all models on test metrics, train–test gap (over/under-fitting), CV stability, inference speed and interpretability; state the selected model and why accuracy alone was not the criterion. Report the **leakage test** (with vs without the agency dateline) and what it shows about generalisation.

## 11. Threshold Recommendation
*"If a news platform deployed your model to flag articles before publishing, what confidence threshold would you recommend, and why?"*

Proposed three-band policy (confirm cut-offs against `reports/threshold_analysis.csv`):
- **P(fake) ≥ ~0.80:** hold for editorial review (high precision).
- **~0.40–0.80:** soft flag / second-opinion queue (protects recall).
- **< ~0.40:** publish normally.

Rationale: missed fakes are costly, so a lower band preserves recall; a high band limits false alarms to a volume editors can handle. The model supports triage and should never auto-block content. **[FILL IN]** the precision/recall/false-positive numbers at each cut-off.

## 12. Business Insights
- **Linguistic patterns of fake news [FILL IN]:** top terms from `top_coefficients.png`.
- **Subjects with higher misinformation [FILL IN]:** from the subject-vs-label table, noting limited overlap.
- **Writing-style differences [FILL IN]:** from the style-feature table.
- **Limitations of text-only detection:** topic/time drift (mostly US politics, narrow period); publisher artefacts; satire and opinion; adversarial rewriting; no real-world fact-checking.
- **Signals that would improve detection:** source/domain credibility, author history, publication date and timing, social-sharing patterns, claim verification against trusted sources, image/metadata checks.

## 13. Recommendations
1. Deploy as a triage assistant with human review, not an auto-publisher.
2. Use the three-band threshold policy and re-tune it quarterly.
3. Retrain regularly on fresh, multi-publisher data.
4. Add source-credibility and metadata features before production.
5. Track false-positive complaints and missed fakes as live KPIs.

## 14. Conclusion
**[FILL IN]** Summarise the outcome and next steps.

## 15. Reproducibility
See `README.md`. Random seed 42; dependencies in `requirements.txt`.
