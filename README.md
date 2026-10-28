# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
```
import numpy as np
import pandas as pd
from scipy import stats
df=pd.read_csv("Data_set.csv")
print(df.isnull().sum())
cols=['rating','current_overall_rank','watchers']
for col in cols:
    df[col]=df[col].interpolate()
new_cols=['show_name','aired_on','original_network']
for col in new_cols:
    df[col] = df[col].fillna(df[col].mode()[0])
print(df.isnull().sum())
```

<img width="450" height="442" alt="image" src="https://github.com/user-attachments/assets/778dd4fc-6cf7-4798-8369-e27407d6c767" />

```
import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("SAMPLEIDS.csv")

print(df.head())
print(df.shape)
print(df.columns)
print(df.info())
print(df.isnull().sum())
print(df.duplicated().sum())

df = df.drop_duplicates()


for col in df.select_dtypes(include="number").columns:
    if df[col].isnull().any():
        df[col] = df[col].fillna(df[col].median())

for col in df.select_dtypes(include="object").columns:
    if df[col].isnull().any() and not df[col].mode().empty:
        df[col] = df[col].fillna(df[col].mode()[0])

print(df.isnull().sum())

for col in df.select_dtypes(include="number").columns:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    outliers = df[(df[col] < lower) | (df[col] > upper)]
    print("\nIQR outliers in", col)
    print(outliers[[col]])

for col in df.select_dtypes(include="number").columns:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]
    print("\nZ-score outliers in", col)
    print(outliers[[col]])

df.to_csv("SAMPLEIDS_cleaned.csv", index=False)

print("Cleaned dataset saved successfully!")
```

<img width="890" height="695" alt="image" src="https://github.com/user-attachments/assets/f523df0c-5780-4e47-9d1a-15070fea27be" />
<img width="623" height="702" alt="image" src="https://github.com/user-attachments/assets/41725773-571f-4649-8e18-67bee3081b02" />
<img width="792" height="692" alt="image" src="https://github.com/user-attachments/assets/4bd22af1-e6c1-4563-b599-94404b135d13" />
<img width="506" height="700" alt="image" src="https://github.com/user-attachments/assets/748c0d17-8455-41e5-8ba3-ed8937262d0f" />


```
import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("heights.csv")

print(df.head())
print(df.describe())
print(df.isnull().sum())

for col in df.select_dtypes(include="number").columns:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = df[(df[col] < lower) | (df[col] > upper)]

    print("\nUnusual measurements using IQR in", col)
    print(outliers[[col]])

for col in df.select_dtypes(include="number").columns:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]

    print("\nUnusual measurements using Z-score in", col)
    print(outliers[[col]])
```


<img width="581" height="587" alt="image" src="https://github.com/user-attachments/assets/06102a3b-63f1-4641-9ad9-0e77e1f88e0b" />


```
import pandas as pd
import numpy as np
from scipy import stats

df = pd.read_csv("iris.csv")

print(df.head())
print(df.describe())

num_cols = df.select_dtypes(include="number").columns

for col in num_cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = df[(df[col] < lower) | (df[col] > upper)]

    print("\nIQR outliers in", col)
    print(outliers[[col]])

for col in num_cols:
    z = np.abs(stats.zscore(df[col], nan_policy="omit"))
    outliers = df[z > 3]

    print("\nZ-score outliers in", col)
    print(outliers[[col]])
```

<img width="766" height="697" alt="image" src="https://github.com/user-attachments/assets/e5029f24-e543-4d93-922b-68dbb58b097a" />
<img width="708" height="713" alt="image" src="https://github.com/user-attachments/assets/50eb1243-a142-41e9-93c0-1a871908af04" />

```
import pandas as pd

df = pd.read_csv("Loan_data.csv")

print(df.head())
print(df.info())
print(df.isnull().sum())
print(df.duplicated().sum())

df = df.drop_duplicates()

num_cols = ["ApplicantIncome", "CoapplicantIncome", "LoanAmount","Credit_History","Loan_Amount_Term"]
cat_cols = ["Gender", "Married", "Education", "Self_Employed", "Property_Area","Dependents"]

for col in num_cols:
    if col in df.columns:
        df[col] = df[col].fillna(df[col].median())

for col in cat_cols:
    if col in df.columns:
        df[col] = df[col].fillna(df[col].mode()[0])
        df[col] = df[col].str.strip()

print("\nMissing values after cleaning:")
print(df.isnull().sum())

print("\nCleaned dataset:")
print(df.head())

df.to_csv("Loan_data_cleaned.csv", index=False)

print("Cleaned dataset saved successfully!")
```

<img width="777" height="697" alt="image" src="https://github.com/user-attachments/assets/9fb99a51-99f3-4cdf-8cee-32ebdcff5296" />
<img width="806" height="710" alt="image" src="https://github.com/user-attachments/assets/13b14083-6862-47ea-9f9b-cfb0e8f83184" />
<img width="848" height="712" alt="image" src="https://github.com/user-attachments/assets/99892079-066c-4670-8a47-18796495d715" />
# Summary

# Result
        
