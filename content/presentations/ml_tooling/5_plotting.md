---
weight: 50
outputs: ["Reveal"]
---

{{% section %}}

# Plotting

ML Tooling comes with a number of plotting facilities to understand our data and models better

---

## Dataset plotting

We can plot some basic information about our dataset that we implemented previously

---

### Target correlation

```python
>>> iris_data.plot.target_correlation()
```

---

![feature-target correlation](/images/iris_feature_target_correlation.png)

---

### Missing data

This will show an overview of what data is missing in the dataset

```python
>>> iris_data.plot.missing_data()
```

---

## Result plots

To understand our model better, first we need to train a model. This will give us a `Result` object, which gives us access to
the plotting functionality.

```python
from ml_tooling import Model
from sklearn.ensemble import RandomForestClassifier

>>> model = Model(RandomForestClassifier())
>>> result = model.score_estimator(iris_data)
[15:15:35] - Scoring estimator...
```

Note that all plots shown here have a `plot_*` counterpart to use if you want more finegrained control

---

When we train a model, we get back a `Result` object

```python
>>> result
<Result RandomForestClassifier: {'accuracy': 0.89}>
```

The `Result` represents a scoring of the estimator and contains information about the metrics used for scoring and their results, as well as a number of convenience plotting functions

---

We can also do cross-validated scoring by passing a `cv` parameter

```python
>>> result = model.score_estimator(iris_data, cv=10)
[15:23:17] - Scoring estimator...
[15:23:17] - Cross-validating...
>>> result
<Result RandomForestClassifier: {'accuracy': 0.96}>
```

---

We can now inspect the results by using the `Results.plot` accessor

---

## Confusion Matrix

We can plot a confusion matrix, with the option of normalizing

```python
>>> result.plot.confusion_matrix(normalized=False)
```

---

![confusion_matrix](/images/confusion_matrix.png)

---

## Feature Importance

We can plot feature importance based on either the model coefficients or the `RandomForest` feature_importance

```python
>>> result.plot.feature_importance()
```

---

![feature_importance](/images/feature_importance.png)

---

## Permutation importance

We can also use the more precise, but more costly `permutation_importance` where we permute each column and compare the resultant score to 
the baseline

```python
>>> result.plot.permutation_importance()
```
---

![permuation importance](/images/permutation_importance.png)

---

## Precision-Recall curve

The precision-recall curve shows us how we are trading off precision and recall in the estimator across different thresholds

```python
>>> result.plot.precision_recall()
```

---

![precision recall](/images/precision_recall.png)

---

## ROC AUC curve

The ROC curve is another classic performance plot for classification - we should generally always check the ROC of a classifier

```python
>>> result.plot.roc_auc()
```

---

![roc auc](/images/roc_auc.png)

---

## Lift score

The lift score will show us how much better our model is than random guessing

```python
>>> result.plot.lift_curve()
```

---

![lift curve](/images/lift_curve.png)

---

## Learning curve

Another important chart is the learning curve - we use it to diagnose whether our model is under or overfitting and if
we need to increase or decrease complexity

```python
>>> result.plot.learning_curve()
```

---

![learning curve](/images/learning_curve.png)

---

## Validation curve

The validation curve lets you plot the performance of the model against a hyperparameter.

It shows effect of the hyperparameter on the model and gives an intuition for how the model responds to that parameter

```python
>>> result.plot.validation_curve("max_depth",
                                 param_range=[1, 5, 10, 20, 30, 40, 60, 80, 100])
```

---

![validation curve](/images/validation_curve.png)

---

# Regression

If we have a regression problem, the plots available will be different, although some will be available for both types

```python
from ml_tooling.data import load_demo_dataset
from ml_tooling import Model
from sklearn.ensemble import RandomForestRegressor

>>> dataset = load_demo_dataset("boston")
>>> model = Model(RandomForestRegressor())
>>> result = model.score_estimator(dataset)
```

---

## Residual plot

To check for goodness-of-fit, we can check the residual fit to verify that the residuals seem randomly distributed

```python
>>> result.plot.residuals()
```

---

![residual plot](/images/residual.png)

---

## Prediction Error

We can also see how good the regression model is by plotting the predictions against the target

```python
>>> result.plot.prediction_error()
```

---

![prediction error](/images/prediction_error.png)


{{% /section %}}