# Time-Series Forecasting On UK Monthly Average Temperature
## Introduction 
This project conducts a Univariate Time-Series analysis to forecast UK Monthly Average Temperature and compares the SARIMAX implementation versus auto_arima way. The SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors) model is an extension of the ARIMA ( Autoregressive Integrated Moving Average) model that allows for seasonality and exogenous variables. auto_arima is a function from the pmdarima library that automatically finds the best ARIMA/SARIMA model for a given time series.

## Data 
The dataset used in this project is downloaded from from [Kaggle]  https://www.kaggle.com/datasets/aryakrishnanar/average-rainfall-and-temperature-in-uk20102019/data. It contains average rainfall (mm) and average temperature (Celsius degree) for the North East England and East England for period 2010-2019.

## Data Pre-processing and Exploratory Data Analysis
Pre-processing and exploratory data analysis were first conducted to explore the dataset. Stationarity of the data series was evaluated. Analysis of the Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) plots were conducted, showing a yearly seasonality of the data and a promising seasonal model structure (SARIMAX(2, 1, 1)x(2, 1, 1, 12)) was proposed. 
<p align="center">
  <img width="460" height="300" src="https://raw.githubusercontent.com/JShi12/Time-Series-Forecasting-On-UK-Monthly-Average-Temperature/main/Images/Temperature_ACF_PACF.png">
</p>

<p align="center">
  <img width="460" height="300" src="https://raw.githubusercontent.com/JShi12/Time-Series-Forecasting-On-UK-Monthly-Average-Temperature/main/Images/temp_difference_acf_pacf.png">
</p>

## Model Construction
### Prepare the dataset. 
* Split the data into train/test. ARIMA model family do not require feature scaling and model input data needs to be 1d. 

### Build and train models. 
Two models/approaches were tested, including:
* SARIMAX(2, 1, 1)x(2, 1, 1, 12). The hyperparameters were set from prior analysis.
* Using pmdarima for uto_arima model selection


The best model from auto_arima is `ARIMA(0,1,1)(3,1,0)[12]`, which has higher AIC and BIC compared to the SARIMAX(2, 1, 1)x(2, 1, 1, 12), therefore the SARIMAX model is preferred. `ARIMA(2,1,1)(2,1,1)[12]` was skipped during the training with auto_arima likely due to early convergence failures. It demonstrates SARIMAX is the better choice when we already know a promising seasonal model structure from prior analysis.  
### Model Validation and Forcast
Both models demonstrate strong performance, each achieving an R² score above 0.9. However, the SARIMAX model outperforms the best SARIMA model selected by auto_arima, delivering a superior fit with a lower RMSE of 1.12.

<p align="center">
  <img width="460" height="300" src="https://raw.githubusercontent.com/JShi12/Time-Series-Forecasting-On-UK-Monthly-Average-Temperature/main/Images/Temperature_Fitting_and_Predictions.jpg">
</p>

## Summary results 

Univariate Time-Series Analysis and Forcasting on UK Monthly Average Temperature with SARIMAX model and auto_arima is conducted in this project. With analysis of the Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) plots, a promising seasonal model structure was proposed and trained with SARIMAX. Another model was trained using the auto_arima from the pmdarima library to the best SARIMA model hyperparameters.

Both models demonstrate strong performance, each achieving an R² score above 0.9. However, the SARIMAX model outperforms the best SARIMA model selected by auto_arima, delivering a superior fit with a lower RMSE of 1.12. When the optimal parameters (p, d, q)(P, D, Q, s) are known from domain knowledge or prior analysis, SARIMAX is the preferred choice. Conversely, for quickly establishing a baseline or prototype model without manual parameter tuning, auto_arima is an effective and convenient option.
