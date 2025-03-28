# Predicting
------------

## Prediction methods

After training a model, you probably want to make predictions on new,
unseen data. Just like a sklearn estimator, you can call the prediction
methods from the model, e.g. `atom.tree.predict(X)`.

All prediction methods transform the provided data through the pipeline
in the model's branch before making the predictions. Transformers that
should only be applied on the training set are excluded from this step
(e.g. outlier pruning or class balancing).

The available prediction methods are the most common methods for estimators
in sklearn's API:

:: atom.basemodel:BaseModel
    :: methods:
        toc_only: True
        include:
            - decision_function
            - predict
            - predict_log_proba
            - predict_proba
            - score


## Prediction attributes

The prediction methods can be calculated on the train, test and
holdout set. You can access them through attributes of the form
[method]_[data_set], e.g. `atom.mnb.predict_train`, `atom.mnb.predict_test`
or `atom.mnb.predict_holdout`. The results are cached after the
first call to avoid consequent expensive calculations. This
mechanism can increase the size of the instance for large datasets.
Use the [clear][atomclassifier-clear] method if you need to free
the memory.

!!! warning
    The prediction attributes for the [score][] method return atom's
    metric score on that set, not the metric returned by sklearn's score
    method for estimators. Use the method's [`metric`][score-metric]
    parameter to calculate a different metric.

!!! note
    The `predict_proba` method of some meta-estimators for [multioutput tasks][]
    (such as [MultioutputClassifier][]) return 3 dimensions, namely, a list of
    arrays with shape=(n_samples, n_classes). One array per target column. Since
    ATOM's prediction methods return pandas objects, such 3-dimensional arrays
    are converted to a multiindex pd.DataFrame, where the first level of the row
    indices are the target columns, and the second level are the classes. Use
    `.loc[[name_of_target_column]]` to only select the predictions for one target.


## Predictions on rows in the dataset

It's also possible to get the prediction for a specific row or rows in
the dataset, providing the names or positions of the rows to the [prediction methods][],
e.g. `atom.rf.predict(10)` returns the random forest's prediction on the
10th row in the dataset, or `atom.rf.predict_proba(["index1", "index2"])`
returns the class probabilities for the rows in the dataset with indices
`index1` and `index2`.

