# 1. Imputation

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

# 2. Handling Outliers

### 1.1 Outlier Detection with Standard Deviation

#Dropping the outlier rows with standard deviation\
factor = 3\
upper_lim = data['column'].mean () + data['column'].std () * factor\
lower_lim = data['column'].mean () - data['column'].std () * factor\

data = data[(data['column'] < upper_lim) & (data['column'] > lower_lim)]\

### 1.2 Outlier Detection with Percentiles

#Dropping the outlier rows with Percentiles\
upper_lim = data['column'].quantile(.95)\
lower_lim = data['column'].quantile(.05)\

data = data[(data['column'] < upper_lim) & (data['column'] > lower_lim)]\

### 1.3 An Outlier Dilemma: Drop or Cap

#Capping the outlier rows with Percentiles\
upper_lim = data['column'].quantile(.95)\
lower_lim = data['column'].quantile(.05)\

data.loc[(df[column] > upper_lim),column] = upper_lim\
data.loc[(df[column] < lower_lim),column] = lower_lim\

# 3 Binning

#Numerical Binning Example\
Value      Bin       
0-30   ->  Low       
31-70  ->  Mid       
71-100 ->  High\

#Categorical Binning Example\
Value      Bin       
Spain  ->  Europe      
Italy  ->  Europe       
Chile  ->  South America\
Brazil ->  South America\

For numerical columns, except for some obvious overfitting cases, binning might be redundant for some kind of algorithms, due to its effect on model performance.

For categorical columns, the labels with low frequencies probably affect the robustness of statistical models negatively. Thus, assigning a general category to these less frequent values helps to keep the robustness of the model. For example, if your data size is 100,000 rows, it might be a good option to unite the labels with a count less than 100 to a new category like “Other”.

#Numerical Binning Example\
data['bin'] = pd.cut(data['value'], bins=[0,30,70,100], labels=["Low", "Mid", "High"])\
   value   bin
0      2   Low
1     45   Mid
2      7   Low
3     85  High
4     28   Low\

#Categorical Binning Example\
     Country
0      Spain
1      Chile
2  Australia
3      Italy
4     Brazil
conditions = [
    data['Country'].str.contains('Spain'),
    data['Country'].str.contains('Italy'),
    data['Country'].str.contains('Chile'),
    data['Country'].str.contains('Brazil')]

choices = ['Europe', 'Europe', 'South America', 'South America']

data['Continent'] = np.select(conditions, choices, default='Other')
     Country      Continent
0      Spain         Europe
1      Chile  South America
2  Australia          Other
3      Italy         Europe
4     Brazil  South America
