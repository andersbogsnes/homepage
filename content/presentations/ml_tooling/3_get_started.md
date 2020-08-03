---
weight: 30
outputs: ["Reveal"]
---

{{% section %}}

# Ml-tooling :heart: sklearn

---

ML Tooling is built on top of scikit-learn

{{< figure src=/images/sklearn_logo.png alt="Scikit-learn Logo" >}}

---

This means that ML-Tooling is compatible with most Scikit-Learn workflows

We can use any estimator from Scikit-learn when creating our models

```python
from ml_tooling import Model
from sklearn.ensemble import RandomForestClassifier

model = Model(RandomForestClassifier())
```

---

We can directly implement any Scikit-learn pipelines/transformers we want

```python
from ml_tooling import Model
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier

>>> pipe = Pipeline([
...    ("scaler", StandardScaler),
...    ("classifier", RandomForestClassifier())
...])

>>> model = Model(pipe)
<Model: RandomForestClassifier>
```

*(though to gain the full benefits, we should use ML-Tooling's transformers)*


{{% /section %}}