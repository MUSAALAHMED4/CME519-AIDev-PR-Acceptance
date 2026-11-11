# Predicting the Acceptance of Pull Requests Opened by AI Coding Agents

**CME519 Data Mining — Midterm Project** · Musa AL AHMED (26221001014) · Istanbul Topkapı University

This project predicts whether a pull request (PR) opened by an AI coding agent (OpenAI Codex, Devin, GitHub Copilot, Cursor, Claude Code, …) will be **merged** or **closed without merge**, using only information available when the PR is opened. It follows the CRISP-DM process.

## Dataset

[AIDev](https://huggingface.co/datasets/hao-li/AIDev) (Hugging Face, `hao-li/AIDev`), curated subset of repositories with more than 100 GitHub stars.

| Table | Use |
|---|---|
| `pull_request` | target (merged / not merged), agent, title, description, creation time |
| `repository` | language, licence, stars, forks, fork flag |
| `all_user` | author followers, following, account creation date |
| `pr_commits` | number of commits per PR |
| `pr_commit_details` | file-level code changes (~12.1M rows), aggregated per PR |

After integration: **332,406 closed PRs** from **22,357 repositories** (74.2% merged, 25.8% rejected). The raw data are downloaded automatically by Notebook 1; they are not stored in this repository.

## Notebooks

| Notebook | Content |
|---|---|
| [`notebooks/01_EDA.ipynb`](notebooks/01_EDA.ipynb) | business understanding, data collection and integration, exploratory data analysis (missing values, skewness, IQR / Z-score outliers, categorical and temporal patterns, correlations, leakage check) |
| [`notebooks/02_Preprocessing_FeatureSelection.ipynb`](notebooks/02_Preprocessing_FeatureSelection.ipynb) | feature engineering, stratified train/test split, leakage-safe preprocessing pipeline (imputation, outlier treatment, one-hot encoding, scaling), comparison of 8 feature-selection methods with 3 models |
| `notebooks/03_Modeling_Evaluation.ipynb` | KNN, Logistic Regression, Decision Tree and Random Forest with 5-fold stratified cross-validation, test-set evaluation, permutation importance, leakage ablation |

## How to run

1. Open the notebooks in [Google Colab](https://colab.research.google.com/) in order (01 → 02 → 03).
2. Run all cells. Each notebook mounts Google Drive and stores intermediate files in `MyDrive/CME519_AIDev/`.

Main libraries: pandas, numpy, scikit-learn, matplotlib, seaborn, pyarrow, huggingface_hub (see `requirements.txt`).

## Evaluation

Rejected PRs are the minority class, so the main metric is the **PR-AUC of the rejected class** (average precision with "not merged" as the positive class), together with ROC-AUC, macro-F1, recall and precision.
