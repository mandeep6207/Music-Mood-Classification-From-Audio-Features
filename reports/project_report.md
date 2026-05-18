# Music Mood Classification Project Report

## Overview
This project builds a leakage-aware classifier for four custom music moods using Spotify-style audio features.

## Dataset
- Raw rows: 114000
- Clean rows: 113549
- Classes: Balanced, Calm, Energetic, Sad

## Leakage Prevention
The direct mood-defining features `energy`, `valence`, and `acousticness` were excluded from training features. This prevents the model from seeing the same columns used to create the labels.

## Best Model
- Model: Random Forest
- Accuracy: 0.9748
- Weighted Precision: 0.9748
- Weighted Recall: 0.9748
- Weighted F1: 0.9746

## Model Comparison
| model               |   accuracy |   weighted_precision |   weighted_recall |   weighted_f1 |
|:--------------------|-----------:|---------------------:|------------------:|--------------:|
| Random Forest       |   0.974813 |             0.97475  |          0.974813 |      0.974603 |
| Gradient Boosting   |   0.969133 |             0.969175 |          0.969133 |      0.968717 |
| Logistic Regression |   0.79225  |             0.785447 |          0.79225  |      0.782744 |

## Saved Artifacts
- Model: `models/mood_classifier.pkl`
- Label encoder: `models/label_encoder.pkl`
- Classification report: `metrics/classification_report.txt`
- Metrics JSON: `reports/model_metrics.json`
- Mood distribution plot: `visuals/mood_distribution.png`
- Top genres plot: `visuals/top_genres.png`
- Correlation heatmap: `visuals/correlation_heatmap.png`
- Confusion matrix: `visuals/confusion_matrix.png`
- Feature importance: `visuals/feature_importance.png`
- Valence vs energy: `visuals/valence_vs_energy.png`
- Tempo distribution: `visuals/tempo_distribution.png`

## Key Insight
Once direct leakage features are removed, the task becomes a realistic supervised learning problem and the evaluation scores reflect genuine generalization.
