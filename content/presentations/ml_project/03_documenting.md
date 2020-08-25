---
weight: 40
outputs: ["Reveal"]
---

{{% section %}}

# Start documenting

---

## Sphinx

Sphinx is a documentation compiler - takes your files and documents and compiles it into a target - usually HTML

---

## Exploring Sphinx

The cookiecutter comes with a basic sphinx setup in the `docs` folder.

- Install sphinx as a dev-dependency
- navigate to docs folder
- run `make html`
- Open `./_build/html/index.html`
- Compare the `.rst` files in the `docs` folder to the html

---

## Change the theme

We can configure sphinx in the `conf.py` file in docs.

We can change the theme by finding the `html_theme` variable and setting it to the theme we want.


---

## Exercise

- Find a theme you like here -> https://sphinx-themes.org/
- Some themes need to be installed
- Set the theme and rebuild the docs

---

## .rst syntax

Sphinx uses `reStructuredText` as it's markup language.

Read the basics here: 

https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html

---

## Directives

Sphinx and .rst has many `directives` that let sphinx know you want to do something.

A directive looks like this:

```rst
.. my_directive::
    :arg1: 1
    :arg2: 2

    Content given to the directive
```

---

### toctree

- The first directive we will be looking at is `toctree`

- `toctree` defines what .rst files are included in the build and should be present somewhere on the `index.rst`

- It has a number of [options](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html)

- Generally, we can list the names of the files we want to include as content

---

## Exercise

- Add a new .rst file `features.rst`
- Describe a feature
- Add features to the toc
- Rebuild the documentation
- Read about the other directives

---

## Linking

We can refer to other sections or definitions using the specific [**role**](https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html)

If we have a **label** somewhere

```rst
.. _my_label:
```

We can create a link to it

```rst
This refers to :ref:`my_label`
```

*(note the backticks)*

---

If we have defined a class documentation, for example, we could link to it

```rst
:class:`Foo`
```

---

## Extensions

Sphinx has a number of extensions we can use - some built-in and some we can install

These are configured in the `conf.py` file and there are a number of useful ones we should set up

---

## [sphinx.ext.autodoc](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html)

Extracts documentation from your code docstrings

### Usage

This will import the documentation for the `my_package.my_module` module

```rst
.. automodule:: my_package.my_module
    :members:
```

---

## [sphinx.ext.napoleon](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html)

We recommend writing docstrings in the "Numpy" docstyle

https://numpydoc.readthedocs.io/en/latest/format.html


This extensions lets sphinx parse this style

---

## [sphinx.ext.mathjax](https://www.sphinx-doc.org/en/master/usage/extensions/math.html)

Allows for writing LaTex math equations and have them render properly

---

## [spinx.ext.intersphinx](https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html)

Lets us refer to other projects that use sphinx in our documentation

Need to add a section to `conf.py` mapping a name to the external project

```python
intersphinx_mapping = {
    "pd": ("https://pandas.pydata.org/pandas-docs/stable/", None),
    "sklearn": ("https://scikit-learn.org/stable/", None),
}
```

Now I can refer to the pandas documentation with `:class:pd.DataFrame` and sphinx will auto-link to the pandas documentation

---

## [sphinx.ext.doctest](https://www.sphinx-doc.org/en/master/usage/extensions/doctest.html)

Test the codesnippets in your documentation. This adds some new directives, chief among them `doctest`

```rst
.. doctest::

    >>> print(2 + 2)
    4
```

{{% /section %}}