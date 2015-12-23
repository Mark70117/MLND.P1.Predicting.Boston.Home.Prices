# P1: Predicting Boston Housing Prices // Udacity Machine Learning Nanodegree
### Mark Anderson
 * [github](https://github.com/Mark70117)
 * [linkedin](https://www.linkedin.com/in/mark-anderson-4b702718)

## Synopsis

A regression model was build with the scikit-learn framework of
Boston Housing data from the 1970s[1].  Model performance was
evaluated and analyzed graphically and then automatically with the
GridSearchCV package.  Finally a prediction was made for a house
with specific features.

## Statistical Analysis and Data Exploration

Basic statistical analysis and exploration of the data was performed.
The basic shape of the data:
 * number of houses: 506
 * number of features: 13
 * minimum price: $5,000
 * maximum price: $50,000
 * mean price: $22,530
 * median price: $21,200
 * standard deviation price: $9,190


## Evaluating Model Performance

The candidate models were evaluated with Mean Square Error measure.
This measure was chosen over Mean Absolute Error because in the
likely use case of the model, pricing a house going on the market,
large errors are undesirable.  The square of the error penalizes
these undesirable large errors.  One can also take the square root
of the MSE to get a meaningful (dollar) amount of error.

The data was randomly split into 70% train, 30% test.  If one were
to train on the full dataset, then the evaluation would be done
with data used to train the model, which would have little power
to accurately reflect how the model would perform on previously
unseen data.  In other words, the model could likely be overfit to
perform well on just that one dataset.

Scikit-learn's grid_search.GridSearchCV [2] was used to generate a
final model.  GridSearchCV exhaustively searches the specified
parameter space, max_depth from 1-10 in this case.

In order for GridSearchCV to search the grid of the parameter space
it needs to apply a performance metric it can use to compare one set
of parameters against an other.  This requires further holding out
a validation set of points and diminishes the training set.  Cross
validation is a means of using data from the initial training set
as both training data and performance evaluation data.  In K-fold
validation, the data is used one time for performance evaluation and
(K-1) times for training.  Thus the use of cross validation help
maximize the use of the data.

Cross validation also diminishes the possibility of overfitting that
might occur is the training set was split into a single train and
validation pair.  If just one split of the data was done, anomalies
might heavily skew the training or performance measure.  With cross
validation, the affect of anomalies are balanced out over the K
validations.  Even in the absence of anomalies, cross validation
helps prevent overfitting that might occur if a single training set
was used. The default value of K=3 cross validations was used.

Cross validation helps identify overfitting of the data and also
helps GridSearchCV to find the sweet spot of in the tradeoff of
predictive power and model complexity.

## Analyzing Model Performance

Examining the learning curves one see a general trend as the training
size increases.  The error for the training set tends to continue
to increase wth training size increases.  The training error is
highest at the lower values of max depth, and while the absolute
amount of the training error decreases with increases in max depth
value, the upward trend of higher error as training size increases
persists.

The error of the test set, however, tends to decline for a period,
after some initial rapid fluctuations, but then fluctuate erratically
around a lower bound as the training size increases beyond a certain
point.  For all values of the max depth, the test set error seems
smoothest in the region between training set size of 125 to 175.
Especially for higher values of max depth, the fluctuations increase
in magnitude as training size increases beyond 175. This indicates
beyond a certain point, more data, just leads to overfitting the
specific data given instead of more generally modeling the feature
space.

Examining the Performance vs Training Size graph for max depth of
1, the model shows signs of under fitting.  A larger training size
produces more error, evidence that the additional data is overwhelming
the ability of the model to capture the complexity, hence under
fitting.

Examining the Performance vs Training Size graph for max depth of
10, the model shows signs of over fitting.  A larger training size
produces almost no error, evidence that the additional data is being
incorporated into the model to such a degree that the model is of
the data itself and not the feature space.

Looking at the model complexity graph, there appears to be an
inflection point, where the slope goes from < -1 to > -1, in the
training error graph close to a max depth 5.  Similarly the graph
of the test error levels out around that max depth. Based on this
relationship a model with a max depth of 5 is the best trade off
between complexity and minimizing error.

## Model Prediction

The random state of the test-train split was varied 10 times.  The
mean/median/std of the predicted value were 20.76/20.97/1.095.  The
mean/mode of the max depth chosen by GridSearchCV were 5.8/4.

There is some variation in the predicted value but it seems well
centered around $20,800.  The RMSE of the model was approximately
$5,900.  So at the prediction could be used as a starting point for
a realtor setting a listing price on a home.  Given the datasets
mean of $22,530 and standard deviation of $9,190, the prediction
looks to be somewhat better that randomly picking a value from the
data set, but given its location near the center of the data the
prediction doesn't seem all that spectacular.


## Notes [1] [dataset
archive](http://archive.ics.uci.edu/ml/datasets/Housing).  Harrison,
D. and Rubinfeld, D.L.  'Hedonic prices and the demand for clean
air', J. Environ. Economics & Management, vol.5, 81-102, 1978.  [pdf
of original
paper](http://www.colorado.edu/ibs/crs/workshops/R_1-11-2012/root/Harrison_1978.pdf)

[2]
http://scikit-learn.org/stable/modules/generated/sklearn.grid_search.GridSearchCV.html
