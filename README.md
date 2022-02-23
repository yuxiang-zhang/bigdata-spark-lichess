
## Research Questions
Based on a set of chess match records:
1. In a new match between two players, who is most likely to win?
2. Can a player's rating be determined from a match between two players?

## Model Design
Both questions entail supervised learning problems. Match winners and player ratings are labeled in our dataset.

Predicting the `winner` of a match is a multiclass classification problem, since the column has 3 unique values {`black`, `white`, `draw`}. Apache Spark implements the following multiclass classifiers:
- logistic regression
- decision trees and ensembles 
- MLP 
- One-vs-Rest
- Naive Bayes 

Decision trees are highly valued for their interpretability, and they perform extremely well. Based on their importance, they rank the features. Training time is among the shortest. Naive Bayes assumes that features are conditionally independent, so it performs well on smaller datasets. Contrarily, logistic regression is a discriminative model that optimizes parameters using gradient descent and generalizes better with larger datasets. A decision tree is a tree diagram which is used for making decisions in computer programming.

Predicting the `black_rating` is a regression problem; the target value is an integer from 789 to 2723. For our case, Spark only supports linear regression, decision trees, and ensembles. Spark was chosen over Dask and scikit-learn due to its familiarity and ease of use to the group members.
