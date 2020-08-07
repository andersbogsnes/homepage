---
weight: 40
outputs: ["Reveal"]
---

{{% section %}}

# Datasets

---

To use ML Tooling, we need the `Model` and we need a `Dataset`

The `Dataset` represents our access to data and tells ML Tooling how to load data for training and prediction

---

A `Dataset` must implement two method

- `load_training_data`, which is expected to return a feature matrix and a target (X and y)

- `load_prediction_data`, which is expected to return a feature matrix and often accepts an ID of some kind to load data for that ID

As a general rule, we want everything in ML Tooling to be Pandas DataFrames

---

## Implementing a Dataset

```python
from  ml_tooling.data import Dataset
from sklearn.datasets import load_iris


class IrisDataset(Dataset):
    def load_training_data(self):
        """Implement how to load data when predicting"""
        # Load iris as dataframes
        iris_data = load_iris(as_frame=True) 
        return iris_data.data, iris_data.target

    def load_prediction_data(self, idx):
        """Implement how to load data when predicting"""
        iris_data = load_iris(as_frame=True)
        return iris_data.data.iloc[[idx]]
```

---

## We can now access the data

```python
>>> iris_data = IrisDataset()
>>> iris_data.x
     sepal length (cm)  sepal width (cm)  petal length (cm)  petal width (cm)
0                  5.1               3.5                1.4               0.2
1                  4.9               3.0                1.4               0.2
2                  4.7               3.2                1.3               0.2
3                  4.6               3.1                1.5               0.2
4                  5.0               3.6                1.4               0.2
..                 ...               ...                ...               ...
145                6.7               3.0                5.2               2.3
146                6.3               2.5                5.0               1.9
147                6.5               3.0                5.2               2.0
148                6.2               3.4                5.4               2.3
149                5.9               3.0                5.1               1.8

[150 rows x 4 columns]

```

---

The first time `x` or `y` is accessed, ML Tooling calls `load_training_data` and then caches the result. 

`load_training_data` is only ever called once.

---

## Convenience Dataset

The two most common usecases is loading data from a file or loading from a database

ML Tooling ships with two `Dataset` implementations to help with these usecases

- FileDataset
- SQLDataset

---

# FileDataset

---

Let's dump our data to a parquet file, because csv's are bad :smile:

The parquet file will contain our data and target

```python
import pandas as pd

# Make sure pyarrow is installed -> pip install pyarrow
pd.concat([load_iris(as_frame=True).data,
           load_iris(as_frame=True).target],
           axis=1).to_parquet("iris.parquet")
```

---

Our FileDataset will accept a filepath which we can use in our loading logic

```python
class FileIrisData(FileDataset):
    def load_training_data(self):
        data = self.read_file()
        return data.drop(columns="target"), data.target

    def load_prediction_data(self, idx):
        data = self.read_file()
        return data.drop(columns="target").iloc[[idx]]

```

---

We can now point our FileDataset at the file we want to load

```python
>>> file_data = FileIrisData("iris.parquet")
>>> file_data.x.head()
   sepal_length  sepal_width  petal_length  petal_width
0           5.1          3.5           1.4          0.2
1           4.9          3.0           1.4          0.2
2           4.7          3.2           1.3          0.2
3           4.6          3.1           1.5          0.2
4           5.0          3.6           1.4          0.2

```

---

# SQLDataset

---

SQLDataset is used when connecting to a database to load data. Let's create a local sqlite database

```python
import pandas as pd
import sqlalchemy as sa

engine = sa.create_engine("sqlite:///iris.db")

(pd.concat([load_iris(as_frame=True).data,
            load_iris(as_frame=True).target],
            axis=1)
   # Make some more friendly column names
   .rename(columns=lambda x: x.rsplit(" ", 1)[0]
                              .replace(" ", "_")
   .to_sql("iris", engine)
   )
```

---

## Creating a SQLDataset

```python
from ml_tooling.data import SQLDataset

class SQLIrisData(SQLDataset):
    table = sa.Table(
        "iris",
        sa.MetaData(),
        sa.Column("index", sa.Integer, primary_key=True),
        sa.Column("sepal_length", sa.Float),
        sa.Column("sepal_width", sa.Float),
        sa.Column("petal_length", sa.Float),
        sa.Column("petal_width", sa.Float),
        sa.Column("target", sa.Integer)
    )
    def load_training_data(self, conn):
        select_statement = sa.select([self.table])
        data = pd.read_sql(select_statement,
                           conn,
                           index_col="index")
        return data.drop(columns="target"), data.target

    def load_prediction_data(self, idx, conn):
        select_statement = (
            sa.select([self.table.c.sepal_length,
                       self.table.c.sepal_width,
                       self.table.c.petal_length,
                       self.table.c.petal_width])
              .where(self.table.c.index == idx)
              )
        return pd.read_sql(select_statement, conn)
```

---

To use it, pass a conn string and what schema to use

*(sqlite doesn't have schemas, so we set it to None)*

```python
>>> sql_data = SQLIrisData(conn="sqlite:///iris.db", schema=None)
>>> sql_data.x.head()
       sepal_length  sepal_width  petal_length  petal_width
index                                                      
0               5.1          3.5           1.4          0.2
1               4.9          3.0           1.4          0.2
2               4.7          3.2           1.3          0.2
3               4.6          3.1           1.5          0.2
4               5.0          3.6           1.4          0.2
```

---

## Copying data

One thing we can do with datasets, is to copy them between representations

*(Make sure we delete our `iris.parquet` file)*

```python
import os
>>> os.remove('iris.parquet')
```

---

We can copy data from a SQLDataset to a FileDataset

```python
new_file_data = sql_data.copy_to(file_data)
[13:24:55] - Copying data...
[13:24:55] - Dumping data from iris
[13:24:55] - Data dumped...
>>> new_file_data.x.head()
   index  sepal_length  sepal_width  petal_length  petal_width
0      0           5.1          3.5           1.4          0.2
1      1           4.9          3.0           1.4          0.2
2      2           4.7          3.2           1.3          0.2
3      3           4.6          3.1           1.5          0.2
4      4           5.0          3.6           1.4          0.2
```

---

Or from one SQLDataset to another if we need to move data between databases

```python
>>> other_sql_data = SQLIrisData("sqlite:///other_iris.db",
                                 schema=None)
>>> sql_data.copy_to(other_sql_data)
[14:55:55] - Copying data...
[14:55:55] - Dumping data from iris
[14:55:55] - Data dumped...
[14:55:55] - Inserting data into iris
```

---

## Exercise

- Go to https://www.kaggle.com/benroshan/factors-affecting-campus-placement 
- Create a Kaggle account if you don't have one
- Download the CSV
- Create a FileDataset based on this CSV

{{% /section %}}