# Time-Series Forecasting On UK Monthly Average Temperature
## Introduction 
This project conducts a Univariate Time-Series analysis to forecast UK Monthly Average Temperature and compares the SARIMAX implementation versus auto_arima way. The SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors) model is an extension of the ARIMA ( Autoregressive Integrated Moving Average) model that allows for seasonality and exogenous variables. auto_arima is a function from the pmdarima library that automatically finds the best ARIMA/SARIMA model for a given time series.

## Data 
The dataset used in this project is downloaded from from [Kaggle]  https://www.kaggle.com/datasets/aryakrishnanar/average-rainfall-and-temperature-in-uk20102019/data. It contains average rainfall (mm) and average temperature (Celsius degree) for the North East England and East England for period 2010-2019.

## Data Pre-processing and Exploratory Data Analysis
Pre-processing and exploratory data analysis were first conducted to explore the dataset. Stationarity of the data series was evaluated. Analysis of the Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) plots were conducted, showing a yearly seasonality of the data and a promising seasonal model structure (SARIMAX(2, 1, 1)x(2, 1, 1, 12)) was proposed. 
<p align="center">
  <img width="460" height="300" src="https://github.com/mkosaka1/AirPassengers_TimeSeries/blob/master/Images/FirstPredictions.jpg">
</p>
## Model Construction
### Prepare the dataset. 
* Split the data into training/validation/test. The validation dataset will be used to evaluate different models. The final selected model is further trained on combined training and validaiton dataset. 
* Scale the datasets
* Transform the datasets using sliding windows
### Build and train RNN models. 
For a deep RNN architecture, we need to choose the depth (i.e, the number of layers the RNN has — e.g. stacking multiple LSTM or GRU layers on top of each other) and the neurons per layer (i.e., the number of units each RNN layer has — controls the layer’s capacity to learn patterns in sequences). 
Usually, a depth of 1–3 layers is sufficient for many problems. 
Typical number of units per layer are 32, 50, 64, 128, depending on data and compute. Too deep or too wide can cause overfitting or slow training — so we tune these by experimenting + validation.

Four different architectures (SimpleRNN, 1-layer LSTM and 2-layer SLTM with different window sizes) were experimented in this project. The 1-layer LSTM model with a window size of 60 performed best.
### Retrain the selected model.
 Combine the training dataset and validation dataset as the new full training dataset and go through the dataset preparation for LSTM models again before retraining the model.  
### Evaluate the model performance on test dataset
### Make predictions for the future
Future stock price prediction were made using prior 60 days stock price. Here each predicted value is fed back into the input for the next prediction. If a prediction is slightly off, that error propagates and often amplifies over subsequent steps. This causes the forecast to “drift” away from reality as we predict farther into the future. No mechanism here to adapt or retrain on new data as predictions move forward. 


## Summary results 

* A simple 1-layer LSTM architecture forcasts the *close price*  accurately with its prior 60 days *close price* as input, achieving a RMSE of 6.85 on the test data set. It has a RMSE of 6.85 on the test data set. This may or may not be acceptable depending on specific problems. Using log returns ($ \log \frac{P_t}{P_{t-1}} $) or price difference ($P_t-P_{t-1}$) are alternative methods for the stock price forcast. 

* We can extend this project for real-time streaming data by automating new data ingestion and retraining the model daily for the forcast of the next day's stock price.
