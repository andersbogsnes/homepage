---
weight: 90
outputs: ["Reveal"]
---

{{% section %}}

# Searching

---

ML Tooling implements three hyperparameter search options

- Gridsearch
- Randomsearch
- Bayesiansearh

---

## Gridsearch

Classic gridsearch

```python
>>> param_grid = {"estimator__max_depth": [1, 2, 4, 8, 16, 32, 64]}
>>> best_model, results = model.gridsearch(dataset, 
                                           param_grid=param_grid)
```

---

We use gridsearch to explore the hyperparameter space systematically

- Focus on one hyperparameter at a time
- Start with a wide range and narrow it down

---

## Randomsearch

- Randomsearch is used to cover a larger hyperparameter space with the same "budget"
- This is often a good first pass if we have no prior knowledge of where to search

---

To use, we specify distributions to sample from using Space objects from skopt

```python
from ml_tooling.search import Integer

>>> param_distributions={"estimator__max_depth": Integer(1, 200)}
>>> best_estimator, results = model.randomsearch(
                          dataset,
                          param_distributions=param_distributions)
```

By default, we run 10 trials, but can change that with the `n_iter` parameter

---

## Bayesiansearch

- Often more effective than Randomsearch, but can be more expensive since it cannot be parallelized
- Uses the results of the previous result to guide the choice of the next hyperparameter from the distributions

---

```python
>>> param_distributions = {"estimator_max_depth": Integer(1, 200)}
>>> best_estimator, results = model.bayesiansearch(
                              dataset,
                              param_distributions=param_distributions)
```

Like randomsearch, bayesiansearch runs 10 trials by default

---

## ResultGroups

For all the searches we get back a `ResultGroup`- a container for `Results`.

```python
best_estimator, results = model.bayesiansearch(dataset,
                          param_distributions={"estimator__max_depth": Integer(1, 200)},
                          metrics=["accuracy", "roc_auc"],
                          n_iter=2)
>>> results
ResultGroup(results=[
    <Result RandomForestClassifier: {'accuracy': 0.85, 'roc_auc': 0.93}>,
    <Result RandomForestClassifier: {'accuracy': 0.84, 'roc_auc': 0.94}>])
```

---

The ResultGroup sorts by the first metric passed, but we can change the sorting, either by changing the order passed
or by calling sort

```python
>>> result.sort(by="roc_auc")
ResultGroup(results=[
    <Result RandomForestClassifier: {'accuracy': 0.84, 'roc_auc': 0.94}>,
    <Result RandomForestClassifier: {'accuracy': 0.85, 'roc_auc': 0.93}>])
```

---

Attribute access is delegated to the first result - otherwise we have to index into the ResultGroup

```python
>>> results.metrics # We get the first result's metrics
Metrics(metrics=[
        Metric(name='accuracy', score=0.8444852941176471),
        Metric(name='roc_auc', score=0.9356060606060606)])
>>> results[1].metrics
Metrics(metrics=[
        Metric(name='accuracy', score=0.8511029411764707),
        Metric(name='roc_auc', score=0.931060606060606)])
```

---

Remember, we can log searches too - we get one log per model trained

```python
param_distributions = {"estimator__max_depth": Integer(1, 200)}

with model.log("search"):
    best_estimator, results = model.bayesiansearch(
                          dataset,
                          param_distributions=param_distributions,
                          metrics=["accuracy", "roc_auc"],
                          n_iter=2)
```

---

## Final Assignment

Build the best model you can on the data

- Try some different hyperparameter searches
- Log some results
- Save the best estimators

{{% /section %}}