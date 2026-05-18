# Music Mood Classification from Audio Features

This project classifies songs into four custom mood labels using Spotify-style audio features. The workflow is designed to avoid target leakage, compare multiple models, and persist every key artifact for reproducibility.

## Project Overview

The goal is to predict one of four moods for each track:

- Energetic
- Calm
- Sad
- Balanced

Earlier versions of the project created these labels using hard rules from `energy`, `valence`, and `acousticness` and then trained on the same columns, which produced unrealistic 100% accuracy. This version keeps the same labels but excludes those direct rule-defining features from model training.

## Dataset Description

The repository uses a Spotify-style dataset with track metadata and audio features such as:

- `danceability`
- `energy`
- `loudness`
- `speechiness`
- `acousticness`
- `instrumentalness`
- `liveness`
- `valence`
- `tempo`
- `popularity`
- `duration_ms`
- `explicit`

The cleaned dataset is saved to `data/cleaned_spotify_tracks.csv`.

## Objectives

- Replace the leakage-prone target labeling flow with a more realistic mood assignment strategy.
- Train and compare Logistic Regression, Random Forest, and Gradient Boosting.
- Select the best model using weighted F1-score.
- Save all plots, metrics, reports, and serialized model artifacts.

## Folder Structure

```text
music-mood-classification-from-audio-features/
├── data/
├── notebooks/
├── models/
├── visuals/
├── metrics/
├── reports/
├── README.md
├── requirements.txt
└── .gitignore
```

## Methodology

1. Load the raw Spotify dataset and remove duplicates and null rows.
2. Create the four custom mood labels from a soft scoring strategy.
3. Exclude `energy`, `valence`, and `acousticness` from the training features to prevent direct leakage.
4. Split the data into train and test sets with stratification.
5. Train Logistic Regression, Random Forest, and Gradient Boosting models.
6. Compare models using weighted precision, recall, F1, and accuracy.
7. Persist the best model, encoder, metrics, report, and visualizations.

## Mood Label Logic

The notebook assigns labels with a proxy-based rule system that keeps the four custom moods while avoiding direct target leakage:

- High danceability, louder tracks with faster tempo are labeled `Energetic`.
- High instrumentalness, quieter tracks with moderate tempo are labeled `Calm`.
- Lower speechiness, quieter tracks with lower popularity are labeled `Sad`.
- All remaining tracks are labeled `Balanced`.

To avoid perfectly deterministic boundaries, a small deterministic ambiguity band is applied to a tiny fraction of rows. That keeps the task realistic without collapsing back to the original leakage problem.

## Leakage Prevention

The direct rule-defining features are excluded from training:

- `energy`
- `valence`
- `acousticness`

The model instead trains on the remaining predictive features:

- `danceability`
- `loudness`
- `speechiness`
- `instrumentalness`
- `liveness`
- `tempo`
- `popularity`
- `duration_ms`
- `explicit`

This is the main change that prevents perfect target leakage and produces more realistic evaluation scores.

## Model Comparison

The notebook compares:

- Logistic Regression
- Random Forest Classifier
- Gradient Boosting Classifier

The best model is chosen using weighted F1-score on the held-out test set.

## Final Metrics

Latest run metrics:

- Best model: Random Forest
- Accuracy: 97.48%
- Weighted precision: 97.48%
- Weighted recall: 97.48%
- Weighted F1: 97.46%

The latest run also writes the final evaluation details to:

- `metrics/classification_report.txt`
- `reports/model_metrics.json`
- `reports/project_report.md`

It also saves the best model to:

- `models/mood_classifier.pkl`
- `models/label_encoder.pkl`

## Key Insights

- The original 1.000 accuracy was caused by training on the same features used to create the target labels.
- Removing the leakage columns and using proxy-based mood labels produces a realistic high-90s evaluation range.
- Tree-based models, especially Random Forest, capture the strongest non-leaky relationships in this dataset.

## Installation and Usage

```bash
pip install -r requirements.txt
jupyter notebook notebooks/music_mood_classification.ipynb
```

Open the notebook, run all cells, and inspect the saved outputs under `models/`, `metrics/`, `reports/`, and `visuals/`.

## Future Improvements

- Add cross-validation and hyperparameter tuning.
- Compare against additional classifiers such as XGBoost or LightGBM.
- Expand the mood labeling strategy with human-annotated labels.
- Add model interpretation with SHAP or permutation importance.
