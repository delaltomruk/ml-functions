# ml-functions

## Change the Object Type to Categorical Values

```python
def to_cat(df):
    for label, content in df.items():
        if pd.api.types.is_string_dtype(content):
            df[label] = content.astype("category").cat.as_ordered()
            
```

## Fill the Missing Numerical Values


```python
def fill_missing_numeric(df):
    for label, content in df.items():
        if pd.api.types.is_numeric_dtype(content):
                # keep track of which rows we've filled with median
                df[label+"_is_missing"] = pd.isnull(content)
                # fill the missing values with the median
                df[label] = content.fillna(content.median())
 ```
## Fill and Tune Categorical Values

```python
def fill_and_tune_cat(df):
    for label, content in df.items():
        if not pd.api.types.is_numeric_dtype(content):
                # see which columns were filled
                df[label+"_is_missing"] = pd.isnull(content)
                # change to numerical, + 1 since pandas assigns -1
                df[label] = pd.Categorical(content).codes + 1
                
```

## Splitting Order

* Encode/transform all categorical variables of your data (on the entire dataset, this ensures categorical variables are encoded the same across training/test sets, if you can't do this, make sure the training and test sets have the same column names).

* Split your data (into train/test).

* Fill the training set and test set numerical values separately.

    * Don’t use numerical data from the future (test set) to fill data from the past (training set).


