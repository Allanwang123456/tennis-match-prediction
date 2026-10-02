# Tennis Match Prediction Chatbot

A Python machine learning project I built to explore whether player statistics could be used to predict ATP tennis match outcomes.

## Overview

The project collects and processes tennis data, creates matchup features from player statistics, and trains a logistic regression model to predict match outcomes. I also built a simple command-line chatbot so users can compare players, view stats, and generate matchup predictions.

## How it works

The project uses Requests and BeautifulSoup to retrieve ATP ranking information and Pandas and NumPy to process the data. Matchup features are created using differences between player statistics such as first-serve percentage, first- and second-serve points won, return points won, aces, and unforced errors.

The prediction model uses logistic regression through scikit-learn. I used an 80/20 train-test split and evaluated the model using accuracy on the held-out test set. The trained model is then used to generate win probabilities for matchups.

## Chatbot commands

- stats for [Player Name]
- list players
- train model
- predict [Player1] vs [Player2]
- help
- exit

## Tools used

Python, Pandas, NumPy, scikit-learn, Requests, and BeautifulSoup.

## Data and limitations

The notebook is set up to retrieve ATP rankings from the ATP Tour website and historical match data from Jeff Sackmann's public tennis_atp dataset.

The current version expects several historical statistics that may not always be available in the dataset. If the required data cannot be loaded, the notebook uses simulated match data so the rest of the pipeline and chatbot can still run. Because of this, a run should not automatically be interpreted as being trained entirely on real historical ATP match data.

## Running the project

Open tennis_match_prediction.ipynb in Google Colab or Jupyter and run the cells in order. Once the chatbot starts, enter train model, then use commands such as predict [Player1] vs [Player2] to test a matchup.
