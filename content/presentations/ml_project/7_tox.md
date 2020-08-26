---
weight: 70
outputs: ["Reveal"]
---

{{% section %}}

# Tox

---

## What is tox

Tox is designed as a testrunning framework, but also works as a generic command runner

### When we run tox

- tox reads a `tox.ini` file
- creates a virtualenv for each environment to be run
- installs defined dependencies into the virtualenv, including your package
- runs the defined command

---

{{< figure src=https://tox.readthedocs.io/en/latest/_images/tox_flow.png >}}

---

## Why use tox

For testing, tox runs the tests on the installed package, not your source code!

This mimics what your users will do and creates more reliable tests

Tox is also designed to test with multiple python versions

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

{{% /section %}}