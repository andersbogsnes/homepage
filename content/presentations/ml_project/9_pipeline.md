---
weight: 90
outputs: ["Reveal"]
---

{{% section %}}

# Pipelines & Transformers

---

## Writing a custom Transformer

Writing a custom transformer is not hard - we just need to follow the scikit-learn recipe!

---

## The template

```python
class MyCustomTransformer(BaseEstimator, TransformerMixin):
    def __init__(self, parameter_1=1, parameter_2=2):
        self.parameter_1 = parameter_1
        self.parameter_2 = parameter_2
```

- Must inherit from `BaseEstimator` and `TransformerMixin`
- The `__init__ ` should only set parameters
- It should not have any logic inside
- All parameters must have default values

---

```python
    def fit(self, x, y = None):
        validate_data(x)
        self.learned_parameter_ = my_func(x)
        return self
```

- The "learning" of parameters from training data happens here
- Any validation of data/parameters goes in here
- Scikit-learn convention is that "learned" attributes have "_" after
- Most transformers work only with `X`, but some need `y` - default to None
- Returns self for chaining

---

```python
    def transform(self, x, y = None):
        # We often want to make sure we don't modify the original data
        x_ = x.copy()
        # Transform the data using learned parameters
        new_data = my_transform_func(x, self.learned_parameter_)
        return new_data
```

- Transform the data using the learned parameter
- Make sure not to modify `x` directly

{{% /section %}}