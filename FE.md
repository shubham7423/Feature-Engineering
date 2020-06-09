# 1.	Imputation

threshold = 0.7

#Dropping columns with missing value rate higher than threshold\
data = data[data.columns[data.isnull().mean() < threshold]]

#Dropping rows with missing value rate higher than threshold\
data = data.loc[data.isnull().mean(axis=1) < threshold]

### 1.1 Numerical Imputation

#Filling all missing values with 0\
data = data.fillna(0)\
#Filling missing values with medians of the columns\
data = data.fillna(data.median())\

### 1.2 Categorical Imputation

#Max fill function for categorical columns\
data['column_name'].fillna(data['column_name'].value_counts()
.idxmax(), inplace=True)\
