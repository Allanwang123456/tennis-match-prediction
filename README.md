# Tennis Match Prediction Chatbot

A Python machine-learning project: a command-line chatbot that returns ATP player statistics and win-probability
predictions for head-to-head matchups, backed by a logistic regression model trained on real match data.

## Data

Real ATP tour-level match statistics from Jeff Sackmann's
[tennis_atp](https://github.com/JeffSackmann/tennis_atp) dataset (licensed CC BY-NC-SA 4.0), seasons 2019-2024.
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

## Run it

Open `tennis_match_prediction.ipynb` in Google Colab, choose **Runtime > Run all**, then type `train model` in
the chatbot to see the held-out evaluation. Other commands: `stats for [player]`, `list players`,
`predict [Player 1] vs [Player 2]`, `help`.

## Limitations

- A player's "form" is their average over the loaded seasons, not a recency-weighted or surface-specific estimate.
- Surface, injuries, head-to-head history, and tournament context are not modeled.
- The live ATP rankings scraper is best-effort and is used only to display a current rank. If scraping fails it falls
  back to a fixed list of player names. Predictions use the ranking recorded in the dataset.
