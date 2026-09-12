Markdown# Machine Learning Lab: Continuous Data Preprocessing

A structured demonstration of foundational data preprocessing techniques applied to continuous variables from a `.csv` dataset.

---

## 📌 Workflow Overview

Raw CSV Dataset│▼[1. Library Import]  ──> pandas, numpy, matplotlib, seaborn, scikit-learn│▼[2. Data Ingestion]  ──> df.read_csv() & inspect structure│▼[3. Missing Values]  ──> df.isnull().sum() ──> Imputation / Drop│▼[4. Outlier Analysis] ─> IQR / Z-Score detection ──> Visual inspection (Boxplot)│▼[5. Train/Test Split]─> train_test_split(test_size=0.2) ──> 80% Train | 20% Test
---

## 🛠️ Step-by-Step Implementation

### 1. Getting the Dataset
Place your continuous target/feature data in the project root directory as a CSV file (e.g., `dataset.csv`).

├── dataset.csv├── preprocessing.py└── README.md
---

### 2. Importing Libraries

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.impute import SimpleImputer
3. Importing the DatasetLoad the CSV file into a pandas DataFrame and separate features ($X$) and target ($y$):Python# Load dataset
df = pd.read_csv('dataset.csv')

# Display basic structure
print(df.head())
print(df.info())

# Separate independent and dependent features
X = df.iloc[:, :-1]
y = df.iloc[:, -1]
4. Handling Missing DataContinuous features require statistical imputation (mean/median) or record pruning:Python# Check total missing values per continuous column
print("Missing values count:")
print(df.isnull().sum())

# Strategy: Impute missing numerical values using the mean
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
X = imputer.fit_transform(X)
5. Outlier Detection & TreatmentIdentify extreme values in continuous features using the Interquartile Range (IQR) method:$$\text{IQR} = Q_3 - Q_1$$$$\text{Lower Bound} = Q_1 - 1.5 \times \text{IQR}, \quad \text{Upper Bound} = Q_3 + 1.5 \times \text{IQR}$$Python# Visualizing outliers via Boxplot
plt.figure(figsize=(8, 4))
sns.boxplot(data=df.select_dtypes(include=[np.number]))
plt.title("Outlier Distribution in Continuous Features")
plt.show()

# IQR Capping / Trimming
Q1 = df.quantile(0.25)
Q3 = df.quantile(0.75)
IQR = Q3 - Q1

# Filter outliers
df_cleaned = df[~((df < (Q1 - 1.5 * IQR)) | (df > (Q3 + 1.5 * IQR))).any(axis=1)]
6. Splitting into Training and Test SetsPartition continuous features to prevent data leakage prior to model training:PythonX_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42
)

print(f"Training set: {X_train.shape[0]} samples")
print(f"Testing set:  {X_test.shape[0]} samples")
📊 Summary of Pipeline StagesStepTechnique / FunctionGoalIngestionpd.read_csv()Load tabular continuous records into memoryMissing DataSimpleImputer(strategy='mean')Prevent NaN errors without shrinking dataset sizeOutlier DetectionBoxplot / IQR BoundsPrevent extreme values from skewing continuous regression slopesDataset Splittingtrain_test_split(test_size=0.2)
