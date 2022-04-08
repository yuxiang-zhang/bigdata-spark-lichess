# Project Summary
## Final Presentation [slides](https://docs.google.com/presentation/d/1SQhSIBqL-xoDLLsmSZWcP5XWd1d33-aB5xgpTvF6SSs/edit?usp=sharing)
## Team #6
|Nafisa Shamsuzzaman| Cheikh Idrissa Diagne| Yu Xiang Zhang| Karim Rhoualem|
|-|-|-|-|
|40095391|40094098|40009567|26603157|
|nafisa.shamsuzzaman@gmail.com|cheikh.diagne76@gmail.com|yuxiang.zhang@mail.concordia.ca|karim.rhoualem@gmail.com|

## Research Questions
Based on a set of chess matches:
1. In a new match between two players, who is most likely to win?
2. Can a player's rating be determined from a single match record with another player?

## Model Design
The questions entail two supervised learning problems, where our dataset is labeled with match winners and player ratings respectively.

Predicting the `winner` of a match is a multiclass classification problem, since the column has 3 unique values {`black`, `white`, `draw`}. Apache Spark implements a limited set of multiclass classifiers.

The decision tree methods generally perform extremely well and are highly valued for their interpretability. They allow ranking features which provide insights about their relative importance for the prediction. Finally, their training time are among the lowest. Logistic regression is a discriminative model that optimizes parameters using gradient descent and generalizes better with larger datasets. 

Predicting the `black_rating` is a regression problem; the target value is an integer ranging from 789 to 2723. Spark only supports the following regressors for our case: linear regression, decision trees and ensembles. If time permits, we may consider ScikitLearn/Dask because it supports more models like SVM.

Our final choices are logistic regression and decision tree for multiclass classification, and linear regression and decision tree for regression. 

## Dataset Summary 
The selected dataset from Kaggle contains 20,058 samples of chess matches, with no missing data. There are 7 numerical and 9 categorical features. For both questions, there exists class imbalance for the labels. For classification, 95% of `winner` fall into `black`/`white`, with 5% `draw`. Because predicting the correct winner is as important as reducing number of incorrect predictions, we chose the F1 score given our class imbalance. For regression, the predicted value will be the 'black_rating'. It follows a normal distribution, with the majority being "mid-ranking". The Root Mean Squared Error (RMSE) is chosen to evaluate the regressor because it serves as a single measure of predictive power for the dataset. The games are timestamped with ~70% occurring from January to September 2017. We will try to sample evenly across time. We observed that some players appear more than once in our dataset. To generalize the model prediction for new players, we consider grouping the samples by player_id when splitting the data to avoid introducing bias from some players. We aim for a dataset split of 60-20-20. Given that the dataset is not large, we need at least 60% of the datapoints for sufficient training. Given that our models include hyperparameters, we need a further 20% for validation. And lastly, we need 20% for testing.

For preprocessing, the numerical features do not require normalization under the selected algorithms. Categorical features will require encoding. For `opening_move_eco`, we will extract the letters as a new feature and encode them sequentially. The `list_of_moves` feature will be fed into an encoder to generate the word embeddings. For the enum and boolean categorical features, one hot encoding will be used.

## Appendix: Features of Dataset
### Categorical
* `id` Game ID (string)
* `white_id` White Player ID (string)
* `black_id` Black Player ID (string)
* `victory_status` (enum) 
* `winner` (enum)
* `rated` Rated games affect player rating  (boolean)
* `opening_eco` ECO classification for opening stategies ([Read more](https://www.365chess.com/eco.php)) (string)
* `opening_name` Name of opening move (string)
* `moves` List of moves (space-separated string)

### Numerical
* `created_at` Game start time (UTC timestamp)
* `last_move_at` Game end time (UTC timestamp) 
* `increment_code` Code that defines how timer is set up ([Read more](https://lichess.org/forum/lichess-feedback/adding-the-increment-for-move-one-on-lichess)) (UTC timestamp) 
* `turns` Number of turns in the game (int)
* `white_rating` White Player Rating (int) 
* `black_rating` Black Player Rating (int)
* `opening_ply` Number of plies used to complete opening move (int)
