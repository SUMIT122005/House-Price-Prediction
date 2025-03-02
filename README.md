# House-Price-Prediction
# 1. Import Libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LinearRegression
from sklearn.tree import DecisionTreeRegressor
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.datasets import load_boston

# 2. Load Dataset (Boston Housing Dataset)
boston = load_boston()
data = pd.DataFrame(boston.data, columns=boston.feature_names)
data['PRICE'] = boston.target

# 3. Exploratory Data Analysis (EDA)
print(data.head())

# Check for null values
print(data.isnull().sum())

# Statistical Summary
print(data.describe())

# Visualize correlation between features
plt.figure(figsize=(12, 8))
sns.heatmap(data.corr(), annot=True, cmap='coolwarm')
plt.show()

# Visualize the distribution of the target variable 'PRICE'
plt.figure(figsize=(8, 6))
sns.histplot(data['PRICE'], kde=True)
plt.title('Distribution of House Prices')
plt.xlabel('Price')
plt.ylabel('Frequency')
plt.show()

# 4. Data Preprocessing
# Feature selection (use all except 'PRICE')
X = data.drop('PRICE', axis=1)
y = data['PRICE']

# Split the data into training and test sets (80% training, 20% test)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Feature scaling (standardize the features)
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# 5. Model Building

# Linear Regression Model
linear_model = LinearRegression()
linear_model.fit(X_train, y_train)

# Predictions
y_pred_linear = linear_model.predict(X_test)

# Decision Tree Regressor Model
tree_model = DecisionTreeRegressor(random_state=42)
tree_model.fit(X_train, y_train)

# Predictions
y_pred_tree = tree_model.predict(X_test)

# Random Forest Regressor Model
forest_model = RandomForestRegressor(random_state=42)
forest_model.fit(X_train, y_train)

# Predictions
y_pred_forest = forest_model.predict(X_test)

# 6. Model Evaluation

# Linear Regression Evaluation
mse_linear = mean_squared_error(y_test, y_pred_linear)
r2_linear = r2_score(y_test, y_pred_linear)

# Decision Tree Evaluation
mse_tree = mean_squared_error(y_test, y_pred_tree)
r2_tree = r2_score(y_test, y_pred_tree)

# Random Forest Evaluation
mse_forest = mean_squared_error(y_test, y_pred_forest)
r2_forest = r2_score(y_test, y_pred_forest)

# Displaying the results
print("Linear Regression MSE:", mse_linear)
print("Linear Regression R2 Score:", r2_linear)

print("Decision Tree MSE:", mse_tree)
print("Decision Tree R2 Score:", r2_tree)

print("Random Forest MSE:", mse_forest)
print("Random Forest R2 Score:", r2_forest)

# Visualizing Model Performance
models = ['Linear Regression', 'Decision Tree', 'Random Forest']
mse_values = [mse_linear, mse_tree, mse_forest]
r2_values = [r2_linear, r2_tree, r2_forest]

# Plotting MSE
plt.figure(figsize=(8, 6))
sns.barplot(x=models, y=mse_values, palette='viridis')
plt.title('MSE Comparison of Models')
plt.ylabel('Mean Squared Error')
plt.show()

# Plotting R2 Score
plt.figure(figsize=(8, 6))
sns.barplot(x=models, y=r2_values, palette='coolwarm')
plt.title('R2 Score Comparison of Models')
plt.ylabel('R2 Score')
plt.show()

# 7. Conclusion
# From the results, you can conclude which model is the best based on R2 Score and MSE.

