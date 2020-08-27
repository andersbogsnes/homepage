---
weight: 80
outputs: ["Reveal"]
---

{{% section %}}

# CI/CD

---

## Integration and Deployment

- Integration means adding new code to our codebase
- Deployment means deploying new code to production

---

## CI/CD providers

Many providers in the market

- Travis
- CircleCI
- Jenkins
- Github Actions
- Azure Pipelines
- Gitlab CI/CD
- Argos

---

## Integration

Gitlab has CI/CD built-in and we are going to add some integration steps

When new code is pushed to gitlab, we want to

- Lint the code
- Run unit tests

---

## The config file

Gitlab looks for a file named .gitlab-ci.yml which describes what jobs to run

```yaml
image: python:3.8.5-slim # What docker image to use

lint: # a job name - can be anything
  script: # A list of commands to run
    - pip install flake8
    - flake8

test:
  script:
    - pip install pytest
    - pytest
```

---

## Exercise

- Add the .gitlab-ci.yml to a new branch
- Push the new branch
- What happens?

---

## A failing pipeline

{{< figure src=/images/added_ci.png >}}

---

## Pipeline overview

{{< figure src=/images/pipeline_view.png >}}

---

## Pipeline logs

{{< figure src=/images/pipeline_logs.png height=500 >}}

---

## Aside - exit codes

Exit codes is a number returned by a command-line program

- 0 is good
- Not 0 is bad

---

## Fix failing CI

We need to write some tests!

---

## Exercise

- Create a `test_calculator.py`
- Write some tests for the add function
- Write some tests for the subtract function
- Git add, commit, push
- Get to a passing pipeline!

---

## Happy pipeline

{{< figure src=/images/happy_ci.png >}}

---

## Happy team

With CI turned on, your teammates can be confident that the code in `develop` and `master` is tested and linted

- Reduces errors
- Reduces codereview (we don't codereview failing code!)
- We know that when we deploy, our code is passing all tests

---

## Exercise

- Add [pytest-cov](https://pytest-cov.readthedocs.io/en/latest/) coverage to CI
- Add [black](https://black.readthedocs.io/en/stable/) formatting
- Add [mypy](http://mypy-lang.org/) (and type hints)


{{% /section %}}