# Spotify genre classification

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/den-bene/spotify-genre-classification/blob/main/Spotify_10k_Tracks_Project.ipynb)

Classification of ten musical genres using 13 audio features and musical attributes from 10,000 Spotify tracks. The project compares linear models, KNN and Random Forest, with a focus on reducing overfitting through regularization.

## Dataset

[Spotify 10000 tracks (genre manually labeled)](https://www.kaggle.com/datasets/adamwu/spotify-10000-tracks-with-genre), by Adam Wu on Kaggle. Each genre contains 1,000 tracks. Track names, artist names and IDs are excluded from the predictors.

The included `10k_tracks.csv` is unchanged and distributed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). The dataset requires attribution and noncommercial use; adaptations must retain the same license. This license applies to the dataset, not the notebook code.

## Method

The data are split into 8,000 training tracks and 2,000 test tracks, stratified by genre with seed 42. Model comparison and parameter selection use five-fold cross-validation on the training set. Imputation, scaling and one-hot encoding are fitted separately within each fold.

The classifiers are Logistic Regression, Logistic Regression with SVD, LinearSVC, KNN and Random Forest. For Random Forest, the selection rule allows a validation macro-F1 loss of up to 0.04 from the best tested mean, then chooses the smallest training/validation gap. This trades some predictive performance for stronger regularization. The tolerance is a practical choice, not a significance test. Test results are excluded from model selection.

## Results

The selected Random Forest uses 300 trees, maximum depth 8, minimum leaf size 8, minimum split size 9 and a feature fraction of 0.5.

Test accuracy: **52.20%**. Test macro-F1: **0.5145**.

Mean scores across the five training-CV folds:

| Model | Validation macro-F1 | Training macro-F1 | Gap |
| --- | ---: | ---: | ---: |
| Random Forest | 0.5288 | 0.6253 | 0.0966 |
| KNN | 0.4904 | 0.9998 | 0.5093 |
| Logistic Regression | 0.4889 | 0.5032 | 0.0144 |
| Logistic Regression + SVD | 0.4855 | 0.4970 | 0.0115 |
| LinearSVC | 0.4510 | 0.4654 | 0.0143 |

The more flexible RF reference scores 0.9275 on training and 0.5619 on validation, a gap of 0.3656. Regularization reduces the gap to 0.0966. The selected validation score is 0.0347 below the grid's highest mean of 0.5634.

Distance-weighted KNN predicts its own training observations almost perfectly by construction. Its training score therefore needs cautious interpretation.

## Run

**Colab:** click Open in Colab, use a Python CPU runtime and run all cells. Download `10k_tracks.csv` from this repository and upload it when prompted.

**Locally:** install the dependencies with `pip install -r requirements.txt`, open the notebook in Jupyter or VS Code and run all cells. Keep the CSV beside the notebook; `data/10k_tracks.csv` is also supported.

## Limitations

Genres are manually labeled, and summary audio features cannot capture every distinction between them. In the track split, 57.55% of test tracks share an artist credit with training, and 34 identical audio vectors appear on both sides. The results describe track classification in this collection, not performance on unseen artists.

Scores used for parameter selection may be optimistic. The internal test does not replace external validation, and the reported results apply to the configurations tested here. Scores from other Spotify datasets or different evaluation protocols are not directly comparable.
