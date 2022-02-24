# Project Summary
## Team #6
|Nafisa Shamsuzzaman| Cheikh Idrissa Diagne| Yu Xiang Zhang| Karim Rhoualem|
|-|-|-|-|
|40095391|40094098|40009567|26603157|
|nafisa.shamsuzzaman@gmail.com|cheikh.diagne76@gmail.com|yuxiang.zhang@mail.concordia.ca|karim.rhoualem@gmail.com|

## Research Questions
Based on a set of chess match records:
1. In a new match between two players, who is most likely to win?
2. Can a player's rating be determined from a single match record with another player?

## Model Design
The questions entail two supervised learning problems, where our dataset is labeled with match winners and player ratings respectively.

Predicting the `winner` of a match is a multiclass classification problem, since the column has 3 unique values {`black`, `white`, `draw`}. Apache Spark implements a limited set of multiclass classifiers.

The decision tree methods generally perform extremely well and are highly valued for their interpretability. They allow ranking features which provide insights about their relative importance for the prediction. Finally, their training time are among the lowest. Logistic regression is a discriminative model that optimizes parameters using gradient descent and generalizes better with larger datasets. 

Predicting the `black_rating` is a regression problem; the target value is an integer ranging from 789 to 2723. Spark only supports the following regressors for our case: linear regression, decision trees and ensembles. If time permits, we may consider ScikitLearn/Dask because it supports more models like SVM.

Our final choices are logistic regression and decision tree for multiclass classification, and linear regression and decision tree for regression. 

## Dataset Summary 
The selected dataset from Kaggle contains 20,058 samples of chess matches, with no missing data. There are 7 numerical and 9 categorical features. For both questions, there exists class imbalance for the labels. For classification, 95% of `winner` fall into `black/white`, with 5% `draw`. Because predicting the correct winner is as important as reducing number of incorrect predictions, we chose the F1 score given our class imbalance. For regression, the predicted value will be the 'black_rating'. It follows a normal distribution, with the majority being "mid-ranking". The Root Mean Squared Error (RMSE) is chosen to evaluate the regressor because it serves as a single measure of predictive power for the dataset. The games are timestamped with ~70% occurring from January to September 2017. We will try to sample evenly across time. We observed that some players appear more than once in our dataset. To generalize the model prediction for new players, we consider grouping the samples by player_id when splitting the data to avoid introducing bias from some players. We aim for a dataset split of 60-20-20. Given that the dataset is not large, we need at least 60% of the datapoints for sufficient training. Given that our models include hyperparameters, we need a further 20% for validation. And lastly, we need 20% for testing.

For preprocessing, the numerical features do not require normalization under the selected algorithms. Categorical features will require encoding. For `opening_move_eco`, we will extract the letters as a new feature and encode them sequentially. The `list_of_moves` feature requires further research to determine how it must be preprocessed. For the enum and boolean categorical features, one hot encoding will be used.

## Appendix: Features of Dataset
### Categorical
* game_id (string)
* white_player_id (string)
* black_player_id (string)
* victory_status (enum) 
* winner (enum)
* is_game_rated (boolean)
* opening_move_eco (string)
* opening_move_name (string)
* list_of_moves (list of strings) 

### Numerical
* start_time (timestamp)
* end_time (timestamp) 
* time_increment (timestamp) 
* number_of_turns (int)
* white_player_rating (int) 
* black_player_rating (int)
* opening_ply (int)
