# Tennis Match Prediction Chatbot

A Python machine-learning project: a command-line chatbot that returns ATP player statistics and win-probability
predictions for head-to-head matchups, backed by a logistic regression model trained on real match data.

## Data

Real ATP tour-level match statistics from Jeff Sackmann's
[tennis_atp](https://github.com/JeffSackmann/tennis_atp) dataset (licensed CC BY-NC-SA 4.0, attribution: Jeff Sackmann /
Tennis Abstract), seasons 2019-2024. The raw files at the original repository returned 404 when this was last tested,
so the notebook reads the same files from public archival mirrors and tries the original URL last.
**The project does not use simulated data.** If the dataset cannot be downloaded or is missing required columns,
the notebook stops with an error.

## Method

1. **Cleaning:** download each season, normalize player names, and drop matches without complete serve statistics.
2. **Features:** for every match, each player's *pre-match form* is the average of their statistics over their
   **earlier** matches only: first-serve %, first-serve and second-serve points won, return points won, ace rate,
   and double-fault rate. The model sees player 1 minus player 2 for each statistic, plus the difference in
   log ranking. Statistics from the match being predicted are never used.
3. **Model:** standardized features into scikit-learn `LogisticRegression`. Which player is "player 1" is
   randomized so the classes are balanced.
4. **Evaluation:** a chronological split (train on earlier matches, test on later ones), compared against two
   baselines: always picking the higher-ranked player, and always guessing one side.

## Results

Chronological holdout, 2019-2024 seasons (15,318 matches with complete serve statistics; 13,187 used after requiring
earlier matches for both players): trained on 10,489 earlier matches and tested on the 2,698 most recent.

| Predictor | Accuracy |
|---|---|
| Logistic regression (this project) | 63.5% |
| Always pick the higher-ranked player | 63.2% |
| Always guess one side | 50.5% |

The model beats the ranking baseline by only about 0.3 percentage points, which is within noise for a test set this
size. Rankings already carry most of the predictive signal; the serve and return features add little on top.

## Run it

Open `tennis_match_prediction.ipynb` in Google Colab, choose **Runtime > Run all**, then type `train model` in
the chatbot to see the held-out evaluation. Other commands: `stats for [player]`, `list players`,
`predict [Player 1] vs [Player 2]`, `help`.

## Limitations

- A player's "form" is their average over the loaded seasons, not a recency-weighted or surface-specific estimate.
- Surface, injuries, head-to-head history, and tournament context are not modeled.
- The live ATP rankings scraper is best-effort and is used only to display a current rank. If scraping fails it falls
  back to a fixed list of player names. Predictions use the ranking recorded in the dataset.
