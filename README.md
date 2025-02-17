# Car Price Prediction Using AI and Machine Learning

This project involves predicting the selling price of used cars based on various features such as fuel type, seller type, transmission, and previous ownership. The model is developed using machine learning techniques like Linear Regression and Lasso Regression.

## Dataset

The dataset used for this project can be found on Kaggle at the following link:  
[Vehicle Dataset from CarDekho](https://www.kaggle.com/datasets/nehalbirla/vehicle-dataset-from-cardekho?resource=download)

### Features:
- **name**: Name of the car
- **selling_price**: The price at which the car is being sold (Target variable)
- **year**: Year of manufacture
- **km_driven**: Total distance driven by the car in kilometers
- **fuel**: Fuel type of the car (Petrol, Diesel, CNG, LPG, Electric)
- **seller_type**: Type of seller (Dealer, Individual, Trustmark Dealer)
- **transmission**: Type of transmission (Manual, Automatic)
- **owner**: Number of previous owners (First Owner, Second Owner, etc.)

## Table of Contents

- [Dependencies](#dependencies)
- [Data Preprocessing](#data-preprocessing)
- [Model Training and Evaluation](#model-training-and-evaluation)
  - [Linear Regression](#linear-regression)
  - [Lasso Regression](#lasso-regression)
- [Visualization](#visualization)
- [Conclusion](#conclusion)

## Dependencies

The following libraries are used in this project:

- **pandas**: For data manipulation and loading the dataset.
- **matplotlib.pyplot**: For plotting and visualizing graphs.
- **seaborn**: For statistical data visualization.
- **sklearn.model_selection**: For splitting the data into training and testing sets.
- **sklearn.linear_model**: For implementing Linear and Lasso Regression models.
- **sklearn.metrics**: For evaluating the model performance.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.linear_model import Lasso
from sklearn import metrics
```

## Data Preprocessing

### Loading the Data

The dataset is loaded from a CSV file into a pandas DataFrame.

```python
car_dataset = pd.read_csv('/content/car.csv')
```

### Inspecting the Data

We inspect the first few rows and check the shape and data types of the dataset.

```python
car_dataset.head()  # Check the first 5 rows of the dataset
car_dataset.shape  # Check the number of rows and columns
car_dataset.info()  # Get detailed information about the dataset
```

### Missing Values and Categorical Data

We check for missing values and analyze the distribution of categorical features such as `fuel`, `seller_type`, `transmission`, and `owner`.

```python
car_dataset.isnull().sum()  # Check for missing values
print(car_dataset.fuel.value_counts())  # Distribution of fuel types
print(car_dataset.seller_type.value_counts())  # Distribution of seller types
print(car_dataset.transmission.value_counts())  # Distribution of transmission types
print(car_dataset.owner.value_counts())  # Distribution of owner categories
```

### Encoding Categorical Data

Categorical features are encoded into numeric values for the model training.

```python
car_dataset.replace({'fuel':{'Petrol':0,'Diesel':1,'CNG':2, 'LPG': 3, 'Electric':4}}, inplace=True)
car_dataset.replace({'seller_type':{'Dealer':0,'Individual':1, 'Trustmark Dealer': 2}}, inplace=True)
car_dataset.replace({'transmission':{'Manual':0,'Automatic':1}}, inplace=True)
car_dataset.replace({'owner':{'First Owner':0,'Second Owner':1, 'Third Owner': 2, 'Fourth & Above Owner': 3, 'Test Drive Car': 4}}, inplace=True)
```

## Model Training and Evaluation

### Splitting the Data

We separate the target variable (`selling_price`) and features (`X`) and then split the data into training and testing sets.

```python
X = car_dataset.drop(['name', 'selling_price'], axis=1)  # Features
Y = car_dataset['selling_price']  # Target variable

X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.1, random_state=2)
```

### Linear Regression

We first train a Linear Regression model on the training data and evaluate it using R-squared error.

```python
lin_reg_model = LinearRegression()
lin_reg_model.fit(X_train, Y_train)

# Prediction on Training Data
training_data_prediction = lin_reg_model.predict(X_train)
error_score = metrics.r2_score(Y_train, training_data_prediction)
print("Linear Regression R squared Error: ", error_score)
```

### Visualizing Linear Regression Results

We visualize the predicted prices vs. actual prices for both training and test data.

```python
# Visualization for Training Data
plt.scatter(Y_train, training_data_prediction)
plt.xlabel("Actual Price")
plt.ylabel("Predicted Price")
plt.title("Linear Regression: Actual Prices vs Predicted Prices (Training Data)")
plt.show()

# Prediction on Test Data
test_data_prediction = lin_reg_model.predict(X_test)
error_score = metrics.r2_score(Y_test, test_data_prediction)
print("Linear Regression Test R squared Error: ", error_score)

# Visualization for Test Data
plt.scatter(Y_test, test_data_prediction)
plt.xlabel("Actual Price")
plt.ylabel("Predicted Price")
plt.title("Linear Regression: Actual Prices vs Predicted Prices (Test Data)")
plt.show()
```

### Lasso Regression

Lasso Regression is applied next to compare its performance against Linear Regression. Lasso performs L1 regularization, helping to reduce overfitting.

```python
lass_reg_model = Lasso()
lass_reg_model.fit(X_train, Y_train)

# Prediction on Training Data
training_data_prediction = lass_reg_model.predict(X_train)
error_score = metrics.r2_score(Y_train, training_data_prediction)
print("Lasso Regression R squared Error: ", error_score)
```

### Visualizing Lasso Regression Results

We visualize the predicted prices vs. actual prices for both training and test data.

```python
# Visualization for Training Data
plt.scatter(Y_train, training_data_prediction)
plt.xlabel("Actual Price")
plt.ylabel("Predicted Price")
plt.title("Lasso Regression: Actual Prices vs Predicted Prices (Training Data)")
plt.show()

# Prediction on Test Data
test_data_prediction = lass_reg_model.predict(X_test)
error_score = metrics.r2_score(Y_test, test_data_prediction)
print("Lasso Regression Test R squared Error: ", error_score)

# Visualization for Test Data
plt.scatter(Y_test, test_data_prediction)
plt.xlabel("Actual Price")
plt.ylabel("Predicted Price")
plt.title("Lasso Regression: Actual Prices vs Predicted Prices (Test Data)")
plt.show()
```

### Key Findings:
- **Linear Regression** provided a good baseline for the predictions.
- **Lasso Regression** helped regularize the model and reduce overfitting, resulting in improved performance.

### Future Work:
- More advanced machine learning models such as Random Forest or Gradient Boosting can be explored for better accuracy.
- Feature engineering and hyperparameter tuning can be applied to further improve model performance.

