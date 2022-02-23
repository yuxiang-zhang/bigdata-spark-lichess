# Appendix: Features of Dataset
## Categorical
* game_id (string)
* white_player_id (string)
* black_player_id (string)
* victory_status (enum) 
* winner (enum)
* is_game_rated (boolean)
* opening_move_eco (string)
* opening_move_name (string)
* list_of_moves (list of strings) 

## Numerical
* start_time (timestamp)
* end_time (timestamp) 
* time_increment (timestamp) 
* number_of_turns (int)
* white_player_rating (int) 
* black_player_rating (int)
* opening_ply (int)


# Dataset Summary 

The selected dataset from Kaggle contains 20,058 samples of chess matches, with no missing data. There are 7 numerical and 9 categorical features. For both questions, there exists class imbalance for the labels. For classification, 95% of winners fall into `black/white`, with 5% `draw`. Because predicting the correct winner is as important as reducing number of incorrect predictions, we chose the F1 score given our class imbalance. For regression, the predicted value will be the 'black_rating'. It follows a normal distribution, with the majority being "mid-ranking". The Root Mean Squared Error (RMSE) is chosen to evaluate the regressor because it serves as a single measure of predictive power for the dataset. The games are timestamped with ~70% occurring from January to September 2017. We will try to sample evenly across time. We observed that some players appear more than once in our dataset. To generalize the model prediction for new players, we consider grouping the samples by player_id when splitting the data to avoid introducing bias from some players. We aim for a dataset split of 60-20-20. Given that the dataset is not large, we need at least 60% of the datapoints for sufficient training. Given that our models include hyperparameters, we need a further 20% for validation. And lastly, we need 20% for testing.

