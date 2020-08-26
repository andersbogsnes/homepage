---
weight: 80
outputs: ["Reveal"]
---

{{% section %}}

# Tox

---

## What is tox

Tox is designed as a testrunning framework, but also works as a generic command runner

---

### When we run tox

- tox reads a `tox.ini` file
- creates a virtualenv for each environment to be run
- installs defined dependencies into the virtualenv, including your package
- runs the defined command

---

{{< figure src=https://tox.readthedocs.io/en/latest/_images/tox_flow.png >}}

---

## Why use tox

- For testing, tox runs the tests on the installed package, not your source code!
- This mimics what your users will do and creates more reliable tests
- Tox is also designed to test with multiple python versions

---

Tox can run arbitrary commands with separated dependencies.

- Want to run sphinx? define an environment
- Want to run black? define an environment
- Want to run mypy? define an environment

---

## The tox.ini file

This is where we define our environments

```ini

[tox]
envlist = what envs will run if I just write `tox` without args

[testenv]
# A special env - will be run if we specify a python version
# Example: `tox -e py38` will run `testenv` with python 3.8
# `py` will run this env with just `python`

[testenv:name_of_env]
# The rest of the envs have a given name
deps = List of things to pip install
commands = command to run
skip_install = whether or not to skip installing your package
```

---

## Implementing a black environment

```ini
[tox]
envlist = black

[testenv:black]
skip_install = True
deps = black
commands = black .
```

---

## Points of note

- tox will remove all environment variables when running - use `passenv` or `setenv` if you need to keep some
- tox is installing in a separate environment - you need to specify all dependencies
- Run a specific environment with `-e` e.g `tox -e black`

---

## Exercise

Let's set up our tox file to run our tests and some utilities

- Add the testing to tox
- Add pre-commit
- Add a linter, like flake8
- BONUS POINTS: check the flake8 docs for adding some flake8 plugins


---

## Configuration

Since tox is a very common framework, many tools will look for configuration in the tox.ini

For details, check the toolings documentation (ex. [pytest](https://docs.pytest.org/en/stable/customize.html))

This lets us have one file with all our configuration in it, instead of one per tool

Let's look at some common recommendations for configuration

---

## Pytest

- Automatically look in the `tests` folder for tests with `testpaths`
- `addopts` adds commandline flags automatically to the `pytest` command (list [here](https://docs.pytest.org/en/stable/usage.html))
  - `-ra` means "generate short report of all tests that didn't pass"
  - `-l` means "show local variables in the failed tests"

```ini
[pytest]
addopts = -ral
testpaths = tests
filterwarnings =
    once::Warning
```

---

## Tox + Coverage

If you have implemented pytest + coverage in your tox file, you might have noticed that the
test paths look a bit strange.

Tox runs your code in a virtualenvironment, so the path to the code is inside that virtualenv!

We configure coverage to treat tox paths the same as our `src`

```ini
[coverage:paths]
source =
    src
    .tox/*/site-packages
```

---

## Flake8

- Control flake8 ignores with `ignore = <error code>` - Only if you really need to!
- Set linelength higher than 79 - `black` uses 88
- Exclude directories like `.tox` or `venv` - if you run flake8 it will go through *everything*

```ini
[flake8]
max-line-length = 100
exclude =
    .git
    build
    dist
    notebooks
    __pycache__
    .tox
    .pytest_cache
    *.egg-info
```

{{% /section %}}