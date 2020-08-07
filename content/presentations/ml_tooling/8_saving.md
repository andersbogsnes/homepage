---
weight: 80
outputs: ["Reveal"]
---

{{% section %}}

# Saving models

---

## Storing your estimators

We can save our models as pickle files to reuse later or to put in production

---

## Storage

We provide two Storage classes that let you store the model

- FileStorage
- ArtifactoryStorage


---
Create an instance of `Storage` and pass it to `model.save_estimator`

```python
>>> storage = FileStorage("./my_models")
>>> model.save_estimator(storage)
[13:13:55] - Saved estimator to estimators/RandomForestClassifier_2020_08_07_13_13_55_137029.pkl
```

---

We can also log the saved estimator - this saves the estimator filepath to the log as well

---

```python
>>> with model.log("classifier"):
...     model.save_estimator(fs)
```

---

### ArtifactoryStorage

`ArtifactoryStorage` works similarly to `FileStorage` expect we need to instantiate it with the url, repo and authorization

```python
storage = ArtifactoryStorage('http://artifactory.com',
                            repo='classifier_project',
                            api_key="MYAPIKEYHERE")
```

---

It can then be used just like FileStorage, but it will save and load estimators in Artifactory instead.

*(Must have installed with the artifactory optional dependency - ml_tooling[artifactory])*

{{% /section %}}
