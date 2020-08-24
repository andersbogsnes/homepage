---
weight: 70
outputs: ["Reveal"]
---

{{% section %}}

# Logging

---

## Log what our models do

- We can log the output of our models, so that we can keep track of what we have done previously
- ML Tooling can automatically keep track of the models we train if we turn on logging

```python
>>> model = Model(RandomForestClassifier())
>>> with model.log("textclassifier"):
        model.score_estimator(dataset)
```

---

This will create a folder named "runs" with a yaml file inside

```yaml
created_time: 2020-08-07 12:23:21.470576
estimator:
  // The entire pipeline definition...
- classname: RandomForestClassifier
  module: sklearn.ensemble._forest
  name: estimator
  params:
    // All the parameters...
estimator_path: null
git_hash: ''
metrics:
  accuracy: 0.8333333333333334
model_name: SchoolPlacement_RandomForestClassifier
versions:
  ml_tooling: 0.11.0
  pandas: 1.1.0
  sklearn: 0.23.1
```

---

We can reload the defined model from the saved yaml if we decide we want to retry a given model

```python
>>> model = Model.from_yaml("./runs/placement/SchoolPlacement_RandomForestClassifier_130257_0.yaml")
```

{{% /section %}}