---
weight: 60
outputs: ["Reveal"]
---

{{% section %}}

# Testing

---

## Writing a test

Now that we have some preprocessing code and data loading code, we should start writing tests

---

## Pytest

We use [pytest](https://docs.pytest.org/en/stable/) - a common choice in modern python

Pytest let's us write simple asserts in our test functions and pytest will handle running the tests

pytest looks for any functions named `test_*` in files named `test_*` and run those

---

## A simple test

```python
def test_add():
    result = my_add_function(2, 2)
    assert result == 2
```

---

## Parametrization

pytest has a concept of parametrization that allows us to test many inputs at once

```python
@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),
    (2, 2, 4),
    (-2, 2, 0)
])
def test_add(a, b):
    result = my_add_function(a, b)
    assert result == expected
```

---

## Fixtures

- A fixture is some setup code that we want to run before our test
- It allows us to reuse logic across tests
- It allows us to create resources, such as a database connection, and then close it when done

---

```python
# This will run before every test that uses this fixture
@pytest.fixture()
def my_df():
    return pd.DataFrame({"a": [1, 2, 3], "b": [4, 5, 6]})

# my_df is now the return value of the fixture
def test_df_func_works(my_df):
    result = my_df_func(my_df)
    expected = pd.DataFrame({"a": 6, "b": 15})

    # Pandas has some testing functions to help assert
    pd.testing.assert_frame_equal(result, expected)
```

---

## A class testing pattern

A good test pattern is writing a test class for a given piece of functionality

That class can then contain the test code and fixtures related to that functionality

---

```python
class TestDataPipe:
    # scope defines how often the function is rerun
    @pytest.fixture(scope="class")
    def df():
        return pd.DataFrame("a": [1, 2, 3], "b": [2, 4, 6])

    # Scope = "class" means once per class
    @pytest.fixture(scope="class")
    def transformed(df):
        return pipe(df)

    # Test one thing per test
    def test_adds_col_b_correctly(df)
        assert df.b == 12

    # If it fails, we know exactly what is wrong
    def test_has_correct_name(df):
        assert df.b.name == "sum"
```

---

## Exercise

Write some tests!

- Create some mock data
- transform it and verify the output
- run pytest on your test code

---

## Coverage

Coverage 

{{% /section %}}