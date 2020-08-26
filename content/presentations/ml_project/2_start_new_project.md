---
weight: 20
outputs: ["Reveal"]
---

{{% section %}}

# Starting a new project

---

## Install pipx

https://github.com/pipxproject/pipx

---

use pipx to install cookiecutter

```bash
$ pipx install cookiecutter
```

---

## Cookiecutter Data Science

We are going to base our project on the Data Science cookiecutter

https://drivendata.github.io/cookiecutter-data-science/

---

```bash
$ cookiecutter https://github.com/drivendata/cookiecutter-data-science

project_name [project_name]: price_forecaster
repo_name [price_forecaster]:
author_name [Your name (or your organization/company/team)]: Anders Bogsnes
description [A short description of the project.]: Predicting Airbnb Prices
Select open_source_license:
1 - MIT
2 - BSD-3-Clause
3 - No license file
Choose from 1, 2, 3 [1]: 1
s3_bucket [[OPTIONAL] your-bucket-for-syncing-data (do not include 's3://')]:
aws_profile [default]:
Select python_interpreter:
1 - python3
2 - python
Choose from 1, 2 [1]: 1
```

---

## The structure

```nohighlight
├── LICENSE
├── Makefile           <- Makefile with commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default Sphinx project; see sphinx-doc.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creators initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── src                <- Source code for use in this project.
│   ├── __init__.py    <- Makes src a Python module
│   │
│   ├── data           <- Scripts to download or generate data
│   │   └── make_dataset.py
│   │
│   ├── features       <- Scripts to turn raw data into features for modeling
│   │   └── build_features.py
│   │
│   ├── models         <- Scripts to train models and then use trained models to make
│   │   │                 predictions
│   │   ├── predict_model.py
│   │   └── train_model.py
│   │
│   └── visualization  <- Scripts to create exploratory and results oriented visualizations
│       └── visualize.py
│
└── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io
```

---

## A few changes

- We want a `src/<package_name>` layout (https://hynek.me/articles/testing-packaging/#src)
- We want to use poetry instead of pip
- We want a centralized configuration file in our package
- We want a `tests` directory to put our tests in

The rest is up to you to decide as we work through the project

---

## Setup our src

```bash
$ cd price_forecaster/src
$ mkdir price_forecaster
# Ignore the error - everything should have worked
$ mv * price_forecaster
# Get back to our project root
$ cd ..
```

---

## Poetry + pyenv for package management

---

### Install poetry

https://python-poetry.org/docs

---

### Install pyenv

https://github.com/pyenv/pyenv-installer

---

### Setup your environment

```bash
$ pyenv install 3.8.5

$ pyenv virtualenv 3.8.5 price_forecaster

$ pyenv local price_forecaster
```

---

```bash
$ poetry init
Package name [price_forecaster]:  
Version [0.1.0]:  
Description []:  Airbnb price forecaster
Author [Anders Bogsnes <andersbogsnes@gmail.com>, n to skip]:  
License []:  MIT
Compatible Python versions [^3.8]:  

Would you like to define your main dependencies interactively? (yes/no) [yes] no
Would you like to define your development dependencies interactively? (yes/no) [yes] no
Generated file

[tool.poetry]
name = "price_forecaster"
version = "0.1.0"
description = "Airbnb price forecaster"
authors = ["Anders Bogsnes <andersbogsnes@gmail.com>"]
license = "MIT"

[tool.poetry.dependencies]
python = "^3.8"

[tool.poetry.dev-dependencies]

[build-system]
requires = ["poetry>=0.12"]
build-backend = "poetry.masonry.api"


Do you confirm generation? (yes/no) [yes] 

```

---

## Poetry

Poetry is a relatively new dependency manager which takes advantage of the new `pyproject.toml` file ([#PEP-518](https://www.python.org/dev/peps/pep-0518/) & [#PEP-517](https://www.python.org/dev/peps/pep-0517/))

---

### Project deps vs Dev-deps

Poetry differentiates between dev-dependencies and runtime-dependencies

```bash
$ poetry add pandas

$ poetry add --dev pytest
```


---

### Lockfile

- Dependencies are split into `pyproject.toml` and `poetry.lock`
- Your direct dependencies are saved in `pyproject.toml`
- The exact state of your environment is saved in `poetry.lock`

---

### Install

- `poetry install` will install your `pyproject.toml`
- It also installs your package in `edit` mode (`-e .`) automatically

### Envs

- Poetry will always install in a virtualenv
- If you are already in one, it will install there
- If you are not, it will automatically generate one and install there

---

# Assignment

- Setup your project with cookiecutter, poetry and git
- Create a gitlab repo for your project

---

## Create a configuration file

We want to be able to reference our data folders - so let's have a config file to import those paths from

```python
import pathlib

# This depends on where the config file is
ROOT_DIR = pathlib.Path(__file__).parents[2]
DATA_DIR = ROOT_DIR / "data"
RAW_DIR = DATA_DIR / "raw"
INTERIM_DIR = DATA_DIR / "interim"
PROCESSED_DIR = DATA_DIR / "processed"
```

{{% /section %}}