# Air-Pollution-Analysis-using-Pyhton-EDA
This is an analysis project on Air Pllotion over 2015 to 2020 , mainly focused on indian aor pollution. 

# import pandas as pd
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

import pandas as pd
data = pd.read_csv(r"C:\Users\sowba\OneDrive\Documents\pyhton project\airdataset.csv", encoding='latin-1')
data.head()

print(data.isnull().sum())

import pandas as pd
from scipy import stats
pollutants = ['PM2.5','PM10', 'NO', 'NO2', 'NOx', 'NH3', 'CO', 'SO2', 'O3', 'Benzene', 'Toluene', 'Xylene']

data = data.dropna()

for pollutant in pollutants:  # Assuming `pollutants` is a list of pollutant column names
    z_scores = stats.zscore(data[pollutant])
    data = data[(z_scores < 3)]
print(data)


data.info()

# Check for missing values in each column 
print(data.isnull().sum())

import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
df = pd.DataFrame(data)

numeric_df = df.select_dtypes(include=['float64', 'int64'])

corr_matrix = numeric_df.corr()

plt.figure(figsize=(8, 6))
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', fmt='.2f')
plt.title("Correlation Matrix Heatmap")
plt.show()

print(data.describe())

#OUTLIERS
pollutants = ['PM2.5','PM10', 'NO', 'NO2', 'NOx', 'NH3', 'CO', 'SO2', 'O3', 'Benzene', 'Toluene', 'Xylene']
for pollutant in pollutants:
    Q1 = data[pollutant].quantile(0.25)
    Q3 = data[pollutant].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    outliers = data[(data[pollutant] < lower_bound) | (data[pollutant] > upper_bound)]
    print(f'Outliers for {pollutant}: {outliers}')
print("Data shape after removing outliers:", data.shape)


import matplotlib.pyplot as plt
import pandas as pd
data = pd.read_csv(r"C:\Users\sowba\OneDrive\Documents\pyhton project\airdataset.csv", encoding='latin-1')

pollutants = ['PM2.5','PM10', 'NO', 'NO2', 'NOx', 'NH3', 'CO', 'SO2', 'O3', 'Benzene', 'Toluene', 'Xylene']
 
for pollutant in pollutants:
    data[[pollutant,'City']].groupby(["City"]).median().sort_values(by=pollutant,ascending=False).head(10).plot.bar(color='r')
    plt.show()

import matplotlib.pyplot as plt
import pandas as pd
data = pd.read_csv(r"C:\Users\sowba\OneDrive\Documents\pyhton project\airdataset.csv", encoding='latin-1')

pollutants = ['PM2.5','PM10', 'NO', 'NO2', 'NOx', 'NH3', 'CO', 'SO2', 'O3']
 
for pollutant in pollutants:
    data[[pollutant,'City']].groupby(["City"]).median().sort_values(by=pollutant,ascending=False).tail(10).plot.bar(color='b')
    plt.show()


import pandas as pd
data['Date'] = pd.to_datetime(data['Date'], format='%d-%m-%Y')  
data['year'] = data['Date'].dt.year  
data['year'] = data['year'].fillna(0.0).astype(int)


import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
# Correct the date format
data['Date'] = pd.to_datetime(data['Date'], format='%d-%m-%Y')

# Extract the year
data['year'] = data['Date'].dt.year

# Continue with your analysis
pollutants = ['PM2.5', 'PM10', 'NO', 'NO2', 'NOx', 'NH3', 'CO', 'SO2', 'O3', 'Benzene', 'Toluene', 'Xylene']

for pollutant in pollutants:
    plt.figure(figsize=(10, 6))
    sns.lineplot(x='Date', y=pollutant, data=data)
    plt.title(f'{pollutant} Levels Over Time')
    plt.xlabel('Date')
    plt.ylabel(f'{pollutant}')
    plt.xticks(rotation=45)
    plt.grid(True)
    plt.show()


import pandas as pd
import matplotlib.pyplot as plt
data = pd.read_csv(r"C:\Users\sowba\OneDrive\Documents\pyhton project\airdataset.csv", encoding='latin-1')
years = range(2015, 2021)
data['Date'] = data['Date'].astype(str)
pollutants = ['PM2.5', 'PM10', 'NO', 'NO2', 'NOx', 'NH3', 'CO', 'SO2', 'O3', 'Benzene', 'Toluene', 'Xylene']
yearly_ranges = {year: {pollutant: None for pollutant in pollutants} for year in years}
for year in years:
    data_year = data[data['Date'].str[-4:] == str(year)]
    for pollutant in pollutants:
        min_value = data_year[pollutant].min()
        max_value = data_year[pollutant].max()
        yearly_ranges[year][pollutant] = (min_value, max_value)
fig, axs = plt.subplots(3, 2, figsize=(15, 15))
fig.subplots_adjust(hspace=0.5, wspace=0.5)
for i, year in enumerate(years):
    ax = axs[i // 2, i % 2]
    ax.bar(yearly_ranges[year].keys(), [yearly_ranges[year][pollutant][1] - yearly_ranges[year][pollutant][0] for pollutant in pollutants])
    ax.set_xlabel('Pollutant')
    ax.set_ylabel('Range')
    ax.set_title(f'Range of Pollutants in {year}')
    ax.tick_params(axis='x', rotation=45)
plt.show()

pollutants = ['PM2.5', 'PM10', 'NO', 'NO2', 'NOx', 'NH3', 'CO', 'SO2', 'O3', 'Benzene', 'Toluene', 'Xylene']
for pollutant in pollutants:
    plt.figure(figsize=(12, 6))
    sns.lineplot(x='City', y=pollutant, data=data)
    plt.xticks(rotation=45)
    plt.xlabel('City')
    plt.ylabel(f'{pollutant} Level')
    plt.title(f'{pollutant} Levels by Area')
    plt.show()

import seaborn as sns
import matplotlib.pyplot as plt

sns.pairplot(data.select_dtypes(include=['float64', 'int64']))
plt.suptitle('Pairplot of Numeric Variables', y=1.02)
plt.show()

import pandas as pd
from sklearn.impute import SimpleImputer
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
import matplotlib.pyplot as plt

data = pd.read_csv(r"C:\Users\sowba\OneDrive\Documents\pyhton project\airdataset.csv", encoding='latin-1')

numeric_features = data.select_dtypes(include=['float64', 'int64']).columns.tolist()
numeric_features.remove('AQI')
X = data[numeric_features]
y = data['AQI']

data_clean = data.dropna(subset=numeric_features + ['AQI'])

X_clean = data_clean[numeric_features]
y_clean = data_clean['AQI']

X_train, X_test, y_train, y_test = train_test_split(X_clean, y_clean, test_size=0.2, random_state=42)

model = LinearRegression()
model.fit(X_train, y_train)

print("\nLinear Regression Model Coefficients:")
for feature, coef in zip(numeric_features, model.coef_):
    print(f"{feature}: {coef}")
print(f"Intercept: {model.intercept_}")


import matplotlib.pyplot as plt

y_pred = model.predict(X_test)

mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

print("\nEvaluation Metrics:")
print(f"Mean Squared Error: {mse}")
print(f"R-squared: {r2}")
plt.figure(figsize=(8, 6))
plt.scatter(y_test, y_pred, color='blue')
plt.title('Actual vs Predicted AQI')
plt.xlabel('Actual AQI')
plt.ylabel('Predicted AQI')
plt.grid(True)
plt.show()


