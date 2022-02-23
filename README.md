## Research Questions
Based on a set of chess match records:
1. In a new match between two players, who is most likely to win?
2. Can a player's rating be determined from a single match record with another player?

## Model Design
The questions entail two supervised learning problems, where our dataset is labeled with match winners and player ratings respectively. 

Predicting the `winner` of a match is a multiclass classification problem, since the column has 3 unique values {`black`, `white`, `draw`}. Apache Spark implements the following multiclass classifiers: 
- logistic regression, 
- decision trees and ensembles, 
- MLP, 
- One-vs-Rest, 
- and Naive Bayes. 

The decision tree methods generally perform extremely well and are highly valued for their interpretability. They allow ranking features which provide insights about their relative importance for the prediction. Finally, their training time are among the lowest. The generative model Naive Bayes assumes the features are conditionally independent and will likely perform well on smaller datasets. By contrast, logistic regression is a discriminative model that optimizes parameters using gradient descent and generalizes better with larger datasets. 

Predicting the `black_rating` is a regression problem; the target value is an integer ranging from 789 to 2723. Spark only supports the following regressors for our case: linear regression, decision trees and ensembles. 

Our final choices are logistic regression and decision tree for multiclass classification, and linear regression and decision tree for regression. 