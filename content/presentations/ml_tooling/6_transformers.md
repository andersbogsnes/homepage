---
weight: 60
outputs: ["Reveal"]
---

{{% section %}}

# Transformers

---

- Everything in ML Tooling is based around pandas `DataFrames`
- This lets us pass metadata such as column names to our functions and methods.
- Thus, we need to implement DataFrame-friendly transformers in ML-Tooling

---

## The importance of Pipelines

- The `Pipeline`  is the foundation of building robust preprocessing in scikit-learn.
- It lets us specify our entire data pipeline as a single object
- Additionally, scikit-learn makes sure to only **learn** attributes about your data when training, so that no accidental data leakage occurs.

---

## An overview of transformers

- All ML Tooling transformers live in `ml_tooling.transformers`
- It's simple to implement your own, and is considered part of the scikit-learn toolkit
- The [documentation](https://ml-tooling.readthedocs.io/en/stable/transformers.html) has plenty on the available transformers

---

## A typical pipeline

We want to set up a Pipeline describing what features we want to use, how to preprocess them and join them together

---

### Define our features

```python
from ml_tooling.transformers import (
    Pipeline,
    DFFeatureUnion,
    Select,
    FillNA,
    ToCategorical,
    DFStandardScaler)

age = Pipeline([
    ("select", Select("age")),
    ("fillna", FillNA(strategy="mean", indicate_nan=True))
])

house_type = Pipeline([
    ("select", Select("house_type")),
    ("fillna", FillNA("missing", indicate_nan=True)),
    ("categorical", ToCategorical())
])

numerical = Pipeline([
    ("select", Select(["customer_since_days",
                       "car_probability",
                       "profitability"])),
    ("scale", DFStandardScaler())
])
```

---

### Combine our features

```python
feature_pipeline = DFFeatureUnion([
    ("age", age),
    ("housetype", house_type),
    ("numerical", numerical)
])

>>> feature_pipeline.fit_transform(train_x)
```

---

### Define our Model

```python
>>> model = Model(RandomForestClassifier(),
                  feature_pipeline=feature_pipeline)
```

---

## Exercise

- Setup a pipeline for the FileDataset we made
- Train a model on the data
- Explore the plots

{{% /section %}}