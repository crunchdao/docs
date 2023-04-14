---
description: Logs the master dataset changes
---

# Versioning

## Master 1.0.3 - 2023/04/14

### Train - Test split

* `X_train_full` and `y_train_full` files were added for download. The last 3 months' data and resolved targets are included. Unresolved targets are set as NaNs.



## Master 1.0.2 - 2023/04/07

### Features

* One `feature` from the `gordon` `strategy` was removed due to a minor fix.



## Master 1.0.1 - 2023/03/31

### Features

* Some `features` were removed from the `master` dataset. The `features` names changed since they are named by their order in their respective `strategies`.



## Master 1.0 - 2023/03/24

### **Features**

* The `features` processing has changed and the `features` are not orthogonalized to risk factors anymore.

### Targets

* The features changed from specific compounded returns over the target's span to raw compounded returns over the respective span.



## Master 0.1 - 2023/03/17

### Features

* Feature names range from Feature\_1 to Feature\_n. They are prefixed with their respective `strategy`.&#x20;
* Features are orthogonal to risk factors.
* The quantization of the features is done in a gaussian way by moon's cross-sections.

### Targets

* Computed as the cumulative product of the specific return of each asset on different time horizons, with a 2 days lag:
  * `target_w`: 7 days
  * `target_r`: 30 days
  * `target_g`: 60 days
  * `target_b`: 90 days
* The targets are quantized in a fat-tailed way by `moon` cross-sections.

### Train - Test split

* The dataset is split into three parts:
  * `X_train` contains the `features` up to the last `moon` - 90 days.
  * `y_train` contains the `targets` up to the last `moon` - 90 days.
  * `X_test` contains the `features` of the last 90 days.
* `Embargo`: the 90 days size of `X_test` data you must predict represents the part of the data on which the `targets` are not fully computed - `target_b` being a 90 days `target`.&#x20;
* `Moons` are continuous between the train set and the test set - First `moon` of the test set starts at max `moon` of the train set + 1.

### Example submission

The example\_submission file is built with a linear regression built with sklearn.

```
import pandas as pd
from sklearn.linear_model import LinearRegression


X_train = pd.read_parquet('X_train.parquet')
y_train = pd.read_parquet('y_train.parquet')
X_test = pd.read_parquet('X_test.parquet')

train_set = pd.merge(X_train, y_train, on=['id', 'Moons'])
train_set.dropna(inplace=True)

targets_cols = [c for c in train_set.columns if 'target' in c]
features_cols = [c for c in train_set.columns if 'Feature' in c]
    
model = LinearRegression().fit(train_set.loc[:, features_cols], train_set.loc[:, targets_cols])
predictions = pd.DataFrame(model.predict(X_test.loc[:, features_cols]), columns=targets_cols)

example_submission = pd.concat([X_test.iloc[:, :2], predictions], axis=1)

example_submission.to_csv('example_submission.csv', index=False)
example_submission.to_parquet('example_submission.parquet')
```

### Submission

Submissions file must respect the following rules to be valid:

* The column names must be the same as those in the `example_submission` file.
* Values must be in the 0-1 range.
* NaNs are not accepted.
* The number of `moons` must be the same as the ones in the `example_submission` file.
* The `ids` in each `moon` must be the same as in the `example_submission` file.
* There mustn't be any constant column in any column in any `moon`.&#x20;
