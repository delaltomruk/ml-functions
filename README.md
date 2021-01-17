# helper-ml-functions

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

## K-fold cross-validation
```python
# Import KFold
from sklearn.model_selection import KFold

# Create a KFold object
kf = KFold(n_splits=3, shuffle=True, random_state=123)

# Loop through each split
fold = 0
for train_index, test_index in kf.split(train):
    # Obtain training and testing folds
    cv_train, cv_test = train.iloc[train_index], train.iloc[test_index]
    print('Fold: {}'.format(fold))
    print('CV train shape: {}'.format(cv_train.shape))
    print('Medium interest listings in CV train: {}\n'.format(sum(cv_train.interest_level == 'medium')))
    fold += 1
```
## Splitting Order

* Encode/transform all categorical variables of your data (on the entire dataset, this ensures categorical variables are encoded the same across training/test sets, if you can't do this, make sure the training and test sets have the same column names).

* Split your data (into train/test).

* Fill the training set and test set numerical values separately.

    * Don’t use numerical data from the future (test set) to fill data from the past (training set).

## Fitting Different Models (regression example)
```python
models = { "Lasso": Lasso(),
         "Ridge": Ridge(),
         "RandomForestRegressor": RandomForestRegressor()}
         
         
def fit_score(models, X_train, X_test, y_train, y_test):
    """
    Fit and evaluate each ML model.
    """
    model_score = {}
    
    for model_name, model_initialized in models.items():
        model_initialized.fit(X_train, y_train)
        model_score[model_name] = model_initialized.score(X_test, y_test)
    return model_score
         
model_score = fit_score(models, X_train, X_valid, y_train, y_valid)
model_score
```
