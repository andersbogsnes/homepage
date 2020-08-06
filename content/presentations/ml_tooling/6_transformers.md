---
weight: 60
outputs: ["Reveal"]
---

{{% section %}}

# Transformers

---

Everything in ML Tooling is based around pandas `DataFrames` - this design decision lets us pass metadata such as column names
to our functions and methods.

As a consequence, we need to implement DataFrame-friendly transformers in ML-Tooling

---

## The importance of Pipelines

The `Pipeline`  is the foundation of building robust preprocessing in scikit-learn.

It lets us specify our entire data pipeline as a single object

Additionally, scikit-learn makes sure to only **learn** attributes about your data when training, so that no accidental data leakage occurs.

---

## An overview of transformers

All ML Tooling transformers live in `ml_tooling.transformers`

It's simple to implement your own, and is considered part of the scikit-learn toolkit

---

## Select

`Select` allows you to select columns from the DataFrame, and is usually used as a first step in a Pipeline

```python
>>> Select(["col1", "col2"])
```

--- 

## FillNA

Fills NaNs with multiple different strategies or simply a fill value. 

Also includes a `indicate_nan` param, which will add a column indicating whether the value was `NaN`

```python
>>> FillNA(strategy="mean", indicate_nan=True)
>>> FillNA("missing")
```

{{% /section %}}