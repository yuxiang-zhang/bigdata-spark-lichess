
## Research Questions
Given a set of chess match records:
1. which player is most likely to win in a new match between two players?
2. can a player rating be assessed from a given match between two players?

## Model Design
The research questions pose two supervised learning problems. Our dataset contains the labels corresponding to match winners and player ratings. 

Predicting the `winner` of a match is a multiclass classification problem, since the column has 3 unique values {`black`, `white`, `draw`}. Apache Spark implements the following classifiers for multiclass: 
- logistic regression, 
- decision trees and ensembles, 
- MLP, 
- One-vs-Rest, 
- and Naive Bayes. 

Not only does the family of decision tree methods generally perform extremely well, they are highly valued for their interpretability. They allow ranking features according to their importance in the decision. Finally, their training time are among the lowest. The generative model Naive Bayes assumes the features are conditionally independent and will likely perform well on smaller datasets. By contrast, logistic regression is a discriminative model that optimizes parameters using gradient descent and generalizes better with larger datasets. 

Predicting the `black_rating` is a regression problem; the target value is an integer ranging from 789 to 2723. Spark only supports the following regressors for our case: linear regression, decision trees and ensembles. 
