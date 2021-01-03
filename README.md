# ml-functions

## Change the Object Type to Categorical Values

```
def to_cat(df):
    for label, content in df.items():
        if pd.api.types.is_string_dtype(content):
            df[label] = content.astype("category").cat.as_ordered()
            
```

## Fill the Missing Numerical Values


```
def fill_missing_numeric(df):
    for label, content in df.items():
        if pd.api.types.is_numeric_dtype(content):
            if pd.isnull(content).sum():
                # keep track of which rows we've filled with median
                df[label+"_is_missing"] = pd.isnull(content)
                # fill the missing values with the median
                df[label] = content.fillna(content.median())
 ```
## Fill and Tune Categorical Values

```
def fill_and_tune_cat(df):
    for label, content in df.items():
        if not pd.api.types.is_numeric_dtype(content):
                # see which columns were filled
                df[label+"_is_missing"] = pd.isnull(content)
                # change to numerical, + 1 since pandas assigns -1
                df[label] = pd.Categorical(content).codes + 1
                
```
