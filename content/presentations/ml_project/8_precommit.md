---
weight: 80
outputs: ["Reveal"]
---

{{% section %}}

# [Pre-commit](https://pre-commit.com/)

{{< figure src=https://avatars2.githubusercontent.com/u/6943086?s=400&v=4 >}}

---

## What is it?

- It lets us run quick tests everytime we make a commit by using git `pre-commit hooks`

- It makes it easy to define and install these little scripts by writing a short config file

- It ensures that linting and formatting is run continously on your codebase - no more CI failures!

---

## Example file

Must be named `.pre-commit-config.yaml`

```yaml
repos:
    # Where to find the hooks
-   repo: https://github.com/pre-commit/pre-commit-hooks
    # What version
    rev: v2.3.0
    # Which hooks to use in that repo
    hooks:
    -   id: check-yaml
    -   id: end-of-file-fixer
    -   id: trailing-whitespace
-   repo: https://github.com/psf/black
    rev: 19.3b0
    hooks:
    -   id: black
```

---

## Usage

Install the hooks

```bash
$ pre-commit install
```

All the checks will now run on every git commit on the changed files

We can also run on all our files - useful for CI

```bash
$ pre-commit run --all-files
```

---

## Exercise

- Check [here](https://github.com/pre-commit/pre-commit-hooks) for some out-of-the-box hooks
- Check [here](https://pre-commit.com/hooks.html) for hooks for other tools
- Add some hooks you think could be useful

{{% /section %}}