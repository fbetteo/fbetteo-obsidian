# Scikit Learn
#datascience
### Transformers

Classes that take a matrix/dataframe input and transform it. Transformation could mean adding features, replacing values, keeping or removing columns, filter rows, etc.

An example is StandardScaler() that enables to standardize the data. We will use this to illustrate.

A transformer must have two method:
* fit
* transform

Fit() will do any calculation required **before** transforming the data. 
In our example with StandardScaler(), the method fit() will calculate the mean and the standard deviation of all the columns.

Transform() will do the actual transformation of substracting the mean and dividing by the standard deviation.

The value of this is that you can fit() with a dataset and apply the same transformation to other dataset. For instance when using training and validation data.

There are a few built-in transformers but you can create your own defining those functions.

Example of a custom transformer that keeps  some columns based on parameter "keep".
```python 
class MonthSelector(BaseEstimator, TransformerMixin):
    """Keeps or drop column "month_ci" in DataFrame"""

    def __init__(self, keep=False):
        """
        Args:
        - keep: one of ["cyclical", "raw_month", False]
        "cyclical" keeps columns ("month_ci_sine","month_ci_cosine")
        """
        self.keep = keep

    def fit(self, X, y=None):
        """"""
        self.all_feature_names_ = list(X.columns)
        return self

    def transform(self, X):
        """"""

        if not self.keep:
            columns_to_drop = [
                "month_ci",
                "month_ci_sine",
                "month_ci_cosine",
            ]
        elif self.keep == "cyclical":
            columns_to_drop = ["month_ci"]
        elif self.keep == "raw_month":
            columns_to_drop = ["month_ci_sine", "month_ci_cosine"]
        X = X.drop(columns_to_drop, axis=1)
        return X
```


### Pipelines

A pipeline allows to chain many transformers together, one after the other. The output of one will be the input for the next one. 

If pipeline.fit() is called, then it will fit and transform all the steps except for the last one, that will be only fitted. This is typically used when the last step is as estimator (model).

If pipeline.fit_transform() is called, then it will fit and transform all the steps. This can be used to preprocess a bunch of stuff but not running a model straight away.

A pipeline fitted can then be used to transform another dataframe using the previous fit() values, or even predict on a new dataset using the model already fitted. 

```python
pipe = Pipeline([('guard', InputGuard()),
                 ('scaler', StandardScaler()),
                 ('reg', LoggingEstimator(est_class=ElasticNet))])

# perform hyperparameter tuning
grid = GridSearchCV(
    pipe, param_grid={'reg__alpha': [0.5, 1.0, 2.0]}, n_jobs=-1)
best_pipe = grid.fit(X_train, y_train).best_estimator_

# make predictions using the best model
y_pred = best_pipe.predict(X_test)
print(f'MAE: {np.abs(y_test - y_pred).mean():.2f}')
```