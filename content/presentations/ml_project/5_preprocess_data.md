---
weight: 50
outputs: ["Reveal"]
---

{{% section %}}

# Preprocessing

---

## Building the dataset

We are working with a file-based dataset, so we should use a `FileDataset` to organize our dataloading.

We can also extend the FileDataset to handle the downloading and preprocessing of the data.

---

## Identify datatypes to read

Identifying the correct datatypes and passing them to `pd.read_csv` can save a lot of time and memory.

Fileformats such as parquet will store the datatypes as metadata, but CSV's are text.

Remember, categorical and string are both datatypes in pandas! 
Check the [documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/basics.html#basics-dtypes)!

---

## Identify preprocessing steps

During the EDA process, you should have been thinking about potential preprocessing steps that need to happen.

You should also have some ideas for feature engineering that could be beneficial.

## Exercise

- Write a PriceForecasterDataset class inheriting from FileDataset
- Extend it with the download data functionality from before
- Extend it with the data preprocessing you've identified

{{% /section %}}