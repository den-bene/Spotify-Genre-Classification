# Spotify genre classification

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/den-bene/spotify-genre-classification/blob/main/Spotify_10k_Tracks_Project.ipynb)

A university machine-learning project classifying ten musical genres from 13 audio descriptors and musical attributes. The analysis covers exploration, preprocessing, baseline comparison, training cross-validation, Random Forest regularization and final test evaluation. The emphasis is on a consistent evaluation procedure and controlling overfitting, allowing a modest loss of validation performance.

**Data:** [Spotify 10000 tracks (genre manually labeled), Adam Wu, Kaggle](https://www.kaggle.com/datasets/adamwu/spotify-10000-tracks-with-genre): 10,000 tracks, 1,000 per genre. Artist names, track names and IDs are excluded as predictors.

The included `10k_tracks.csv` is attributed to Adam Wu and redistributed unchanged under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/), as listed in the Kaggle dataset metadata. This dataset license requires attribution, noncommercial use and ShareAlike for adaptations. It applies to the dataset; it does not license the notebook code. CSV SHA-256: `be5713694114eab3372228bf925fe412fa676e9af29f54a535fdf80ab2778d80`.

## Method

Use a stratified 80/20 split with seed 42. Fit imputation, scaling and one-hot encoding inside each training-CV fold. Compare Logistic Regression with and without SVD, LinearSVC, KNN and Random Forest using the same five stratified folds.

For Random Forest, select the smallest mean training/validation macro-F1 gap among configurations within **0.04** of the highest mean validation macro-F1. This declared tolerance prioritizes stronger regularization while preserving useful predictive performance; it is a practical rule, not a significance test. Compare the chosen forest with the other classifiers before final evaluation. The test does not determine parameters or model choice.

Selected search scores are development estimates and may be optimistic after hyperparameter selection. Test diagnostics describe the fixed model and do not drive retuning.

## Results

Selected model: **Random Forest**, with 300 trees, maximum depth 8, minimum leaf size 8, minimum split size 9 and feature fraction 0.5.

On the 2,000-track test set: accuracy **0.5220**, macro-F1 **0.5145**.

Training-only five-fold CV:

| Model | Validation macro-F1 | Training macro-F1 | Gap |
| --- | ---: | ---: | ---: |
| Random Forest | 0.5288 | 0.6253 | 0.0966 |
| KNN | 0.4904 | 0.9998 | 0.5093 |
| Logistic Regression | 0.4889 | 0.5032 | 0.0144 |
| Logistic Regression + SVD | 0.4855 | 0.4970 | 0.0115 |
| LinearSVC | 0.4510 | 0.4654 | 0.0143 |

The flexible RF reference gives validation macro-F1 0.5619, training macro-F1 0.9275 and gap 0.3656. The selected forest reduces that gap to 0.0966, with validation macro-F1 0.5288. The highest RF validation mean in the grid is 0.5634; the chosen regularization sacrifices 0.0347 macro-F1 relative to it. The remaining gap is reported rather than treated as eliminated.

For distance-weighted KNN, predicting its own training observations yields near-perfect training scores by construction; that score requires cautious interpretation.

## Run

**Colab:** use the Open in Colab button above, choose a Python CPU runtime and run all cells. Download `10k_tracks.csv` from this repository and upload it when prompted. Save the executed notebook and download `spotify_results.zip` from the Files sidebar.

**VS Code:** open this folder, install the Microsoft Python/Jupyter extensions, create a Python 3.11+ virtual environment, install `requirements.txt`, select that environment as the notebook kernel and run all cells. The included CSV can stay in the project root; `data/10k_tracks.csv` is also supported. Detailed Italian steps: [RUN_IT.md](RUN_IT.md).

## Contents

- `Spotify_10k_Tracks_Project.ipynb`: complete analysis with outputs, model comparison, RF search, metrics, confusion matrix, permutation importance and learning/validation curves.
- `10k_tracks.csv`: attributed dataset, included under CC BY-NC-SA 4.0.
- `requirements.txt`: compatible dependency ranges for local execution.
- `RUN_IT.md`: concise Italian instructions for Colab and VS Code.
- `.gitignore`: excludes generated reports, saved models, archives and local environments.

The notebook generates `reports/`, `models/` and `spotify_results.zip` during execution. They are not required in the repository: outputs are already visible in the notebook. The saved execution used Python 3.13.1, NumPy 2.5.4, pandas 3.0.6, SciPy 1.18.1, scikit-learn 1.9.1, Matplotlib 3.11.2 and joblib 1.6.0.

The notebook executed successfully from a fresh local kernel, including prediction equality after model save/reload. Runtime on this machine was 362 seconds. Colab timing and results may vary with runtime and package versions; the upload path was checked with a simulated Colab upload.

## Limitations

Genre labels are manually curated and summary audio descriptors have limited information. Artist-credit overlap affects 57.55% of test tracks, and 34 exact audio vectors occur on both sides of the split. This protocol describes track classification in this collection, not generalization to unseen artists. The internal test does not replace external validation.

The results describe the configurations tested here, not a proven maximum for this dataset. Scores from other Spotify collections or different labels, features and splits are not directly comparable.
