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

```python
>>> iris_data.plot.target_correlation()
```

![feature-target correlation](/images/iris_feature_target_correlation.png)

---

To understand our model better, first we need to train a model

```python
from ml_tooling import Model
from sklearn.ensemble import RandomForestClassifier

>>> model = Model(RandomForestClassifier())
>>> result = model.score_estimator(iris_data)
[15:15:35] - Scoring estimator...
```

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

![permuation importance](/images/permutation_importance.png)

---


{{% /section %}}