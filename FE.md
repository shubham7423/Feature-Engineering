<h1> 1.	Imputation:<h1>

<p>threshold = 0.7

#Dropping columns with missing value rate higher than threshold<br>
data = data[data.columns[data.isnull().mean() < threshold]]

#Dropping rows with missing value rate higher than threshold<br>
data = data.loc[data.isnull().mean(axis=1) < threshold]<p>

