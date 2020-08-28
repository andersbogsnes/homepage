---
weight: 100
outputs: ["Reveal"]
---

{{% section %}}

# Adding a CLI (Command Line Interface)

---

## Running our model from the command line

Training a model in Jupyter is useful, but we often want to be able to train a model from the command line

Then we can automate the retraining pretty easily

<p class="fragment">We need a CLI!</p>

---

## Click

Click is a great package for adding a CLI to our model - see the [docs](https://palletsprojects.com/p/click/)

```python
import click

@click.command()
@click.option("--cv", default=10, type=int, help="Number of cross-validations to do")
def train(cv: int):
    click.secho(f"Training with {cv} splits...", fg="green")
    result = model.score_estimator(dataset, cv=cv)
    click.echo(result)

if __name__ == "__main__":
    train()
```

---

Now we can call it from the command line - assuming it's in a file called `main.py`

```bash
$ python main.py --cv 5
Training with 5 splits...
<Result metrics={"accuracy": .98}>
```

---

We can create a "proper" CLI by adding a line to `pyproject.toml`

```toml
[tool.poetry.scripts]
my_cli = 'my_model.main:train'
```

---

After running `poetry install` we can now do

```bash
$ my_cli --cv 5
Training with 5 splits...
<Result metrics={"accuracy": .98}>
```

---

## Multiple commands

If we want to have *subcommands* - e.g. train, publish, test etc we need a `click.group`

```python
import click

@click.group()
def cli():
    pass

@cli.command()
def train():
    # train code

@cli.command()
def publish():
    # Publish code

if __name__ == "__main__":
    cli()
```

{{% /section %}}