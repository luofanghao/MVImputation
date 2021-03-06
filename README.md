# Introduction
This component is for missing value imputation. It will give the evaluation result of different imputation method. Now the functionality is limited to:

* one label problem and label target is the second column in label file

### Dependencies
[check here](environment.yml)

if you have conda, simply do the following:

```sh
conda-env create .
source activate mvi
python test_example.py
```

### Usage:
see [test_example.py](test_example.py): 

```python
from sklearn import svm
from sklearn import metrics
from sklearn.metrics import f1_score, make_scorer

from dsbox.datapreprocessing.cleaner import Imputation
from dsbox.datapreprocessing.cleaner import helper_func

# STEP 1: get data
data_path = "../dsbox-data/o_38/data/"
data_name = data_path + "trainData.csv"
label_name = data_path + "trainTargets.csv" # make sure your label target is in the second column of this file

drop_col_name = ["d3mIndex"] #input the column name that is useless in the dataset, eg. id-like column, name column, etc.
data, label = helper_func.dataPrep(data_name, label_name, drop_col_name)

# STEP 2: define your machine learning model and scorer
clf = svm.SVC(kernel='linear')
scorer = make_scorer(f1_score, average="macro") # score will be * -1, if greater_is_better is set to False

# STEP 3: go to use the Imputer !
imputer = Imputation(model=clf, scorer=scorer, strategies=["mean", "max", "min", "zero"])
best_imputation = imputer.fit(data, label)
print best_imputation

```


### TODO:
1. finish verbose func
