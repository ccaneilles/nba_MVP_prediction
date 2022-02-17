# Predicting NBA 2021-22 Most Valuable Player

*Oscar Fossey (ENSAE) and Charles Caneilles (ENSAE).*

In this project, the objective is to build a NBA MVP prediction model and forecast the upcoming MVP for the current (2022) season.

Their is two notebooks for this project : 
- Scrapping notebook (creation of the dataset, scrapping.ipynb)
- Predicting notebook (building the model, predicting_NBA_MVP.ipynb)

## OBJECTIVES
At the end of each season the National BasketBall Association (NBA) has to pick a player and name him the Most Valuable Player (MVP) of the season. The idea is to predict the next NBA MVP using Machine Learning and Statistical Data from NBA players between 1979 (first season of 3pts line era) and 2021.

## MOTIVATION

Oscar and I are two big fans of the NBA and its players. The NBA tracks players and team statistics almost since its debut. Advanced statics have been created over the years and statistics are in center of any discussion about the NBA and its players. We wanted to use those statitics to determine mid-season who is currently the most likely to have the MVP trophy at  end of the season.


## DATA AND SCRAPPING

Our final dataset is an aggregation of 5 differents datasets from the site [BasketBall Reference](https://www.basketball-reference.com)

Dataset content  | Reference
------------- | -------------
Classic player statistics per match  | https://www.basketball-reference.com/leagues/NBA_2021_per_game.html
Advanced player statistics per match   | https://www.basketball-reference.com/leagues/NBA_2021_advanced.html
History of the winners of the MVP   | https://www.basketball-reference.com/awards/mvp.html
MVP Award Voting History   | https://www.basketball-reference.com/awards/awards_2021.html#mvp
Collective statistics (Team wins,etc ...) | https://www.basketball-reference.com/leagues/NBA_2021_standings.html

A row in the dataset corresponds to the performance of a player in a given season. Thus the "primary key" of the dataset is the couple "Player - Season". With this key we could couple the different datasets extracted (classic statistics of the players per match, advanced statistics of the statistics per game, trophy winner, trophy vote, etc ...).To add the collective statistics, we have coupled on the "Team - Season" key.

## PREDICTOR ARCHITECTURE AND STRATEGY

### Strategy
The major problem of our project is that there is only one MVP per season, so 40 positive labels out of more than 17,000 candidates: this is far too few to be able to build a model on this label.

We have created another label: ShareYN (received votes or not), which determines whether the player is in the race or not. We create this label with the results of the votes of each season. This way we can can try to determine which players will possibly receive a vote at the end of the season.

We will call a contestant a player who has received at least one vote for his season.

Thus to determine our MVP, we built a two-step solution:
- Identify the candidates (via a classification on the lablel ShareYN)
- To separate the candidates (via a regression on the current results of the votes: MVP_share)

### Model architecture
![alt text](https://github.com/ccaneilles/nba_mvp_prediction/blob/main/images/strategy.png)

## RESULTS

For the classification stage we end up using a Ridge classifier. For the regressor we tried differents models :
- Linear regression
- Random forest
- kNN
- Multilayer perceptron

Note : For all the models the selections of the hyperparameters has been made by cross validation

**Results for the season 2021-2022 (prediction with stats before  January):**
![alt text](https://github.com/ccaneilles/nba_mvp_prediction/blob/main/images/results.png)

$R^2$ is the score of each regression calculated on the test sample. The poor results can be explained by the fact that voters have to choose the top 3 players and rank them, while our model does not have the ability to do this since it takes each candidate independently of the others. Our models do not decide while the voters do.

Looking at the results from our fan eyes, the outcome seems consistent both for podium and the winners.

The 10 contenders selected by the BasketBallReference "MVP Tracker", are all part of the 20 candidates selected by our prediction model.

## PACKAGES

![alt text](https://github.com/ccaneilles/nba_mvp_prediction/blob/main/images/packages.jpeg)

## CREDITS

The project has been made and lead by : Charles Caneilles and [Oscar Fossey](https://github.com/oscarfossey)

All data is from [Sport Reference](https://www.sports-reference.com/termsofuse.html) : *“SRL believes in data democratization—open access to data and the tools that use it.”*