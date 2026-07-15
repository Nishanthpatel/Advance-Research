# Advance-Research
Project Overview

This project analyses and forecasts German electricity demand using statistical, machine-learning and deep-learning approaches. The study focuses on both point-forecast accuracy and prediction-interval reliability over a fixed two-year test horizon.

The main objective is to determine which forecasting method provides the best balance between:

Forecast accuracy
Uncertainty calibration
Interpretability
Computational complexity
Operational usefulness
Data

The analysis uses publicly available German hourly electricity-load data from the Open Power System Data platform.

Berlin temperature data is used as a representative weather variable for Germany. German public-holiday and calendar indicators are also included as external predictors.

The hourly load data is prepared by:

Removing duplicate timestamps
Checking missing observations
Creating a regular hourly index
Converting load from MW to GW
Aggregating the data into daily and weekly averages
Retaining complete weeks only
Splitting the final 104 weeks as the test period
Forecasting Models

The project compares the following models:

Benchmark Models
Mean forecast
Naive forecast
Seasonal-naive forecast
Drift forecast
Statistical Models
SARIMA
SARIMAX with temperature
SARIMAX with calendar variables
SARIMAX with temperature and calendar variables
Machine-Learning Model
Quantile Gradient Boosting

Separate quantile models are used to generate:

Median forecasts
80% prediction intervals
95% prediction intervals
Deep-Learning Model
Long Short-Term Memory network

The LSTM uses hourly electricity-load sequences and generates recursive hourly forecasts. These forecasts are aggregated to weekly values for comparison with the other models.

Analysis Workflow

The project follows these stages:

Load and clean the electricity-demand data.
Aggregate hourly observations into daily and weekly values.
Download and prepare temperature and holiday variables.
Conduct exploratory time-series analysis.
Examine trend, seasonality and changing variability.
Perform ADF and KPSS stationarity tests.
Inspect ACF and PACF plots.
Generate benchmark forecasts.
Search SARIMA parameter combinations using AIC.
Fit SARIMA and SARIMAX models.
Evaluate statistical-model residuals.
Tune Quantile Gradient Boosting using chronological validation.
Tune and train the hourly LSTM.
Compare all forecasts using a common two-year test period.
Evaluate prediction-interval calibration and sharpness.
Evaluation Metrics

Point forecasts are compared using:

Mean Absolute Error
Root Mean Squared Error
Symmetric Mean Absolute Percentage Error
Mean Absolute Scaled Error
Forecast bias

Prediction intervals are compared using:

Prediction Interval Coverage Probability
Mean Prediction Interval Width
Interval score
Number of observations below the lower boundary
Number of observations above the upper boundary
Coverage during high-demand periods
Coverage by season
Data-Leakage Prevention

The project uses a chronological training and testing split.

All scaling parameters are estimated from training data only. Actual test-period demand is not used when constructing later lagged or rolling features.

Recursive machine-learning and LSTM forecasts use previously predicted values when generating future inputs.

Observed test-period temperature is used for some models. These results are therefore interpreted as conditional forecasts rather than fully operational forecasts based on unknown future weather.

Main Findings

The seasonal-naive model provides the lowest point-forecast error over the two-year test period.

Quantile Gradient Boosting is the strongest advanced point-forecast model, although it does not outperform seasonal naive.

SARIMA provides the strongest prediction-interval performance, but its actual interval coverage remains below the nominal levels.

SARIMAX produces narrower intervals, but these intervals are severely under-calibrated and frequently exclude actual demand.

The LSTM captures short-term hourly patterns but performs poorly when recursively forecasting over a long two-year horizon.

Requirements

The main Python packages required are:

numpy
pandas
matplotlib
scipy
statsmodels
scikit-learn
requests
holidays
tensorflow

Install the required packages using:

pip install numpy pandas matplotlib scipy statsmodels scikit-learn requests holidays tensorflow
Running the Analysis

Run the notebook cells in order.

The workflow downloads the required public datasets, prepares the variables, trains the models, generates forecasts and saves the resulting figures and evaluation tables.

Some stages, particularly the SARIMA parameter search and LSTM training, may require additional execution time.

Outputs

The analysis produces:

Data-quality summaries
Exploratory time-series plots
Stationarity-test results
ACF and PACF plots
Benchmark forecasts
SARIMA and SARIMAX forecasts
Prediction-interval plots
Residual-diagnostic plots
Quantile Gradient Boosting forecasts
LSTM forecasts and learning curves
Point-forecast comparison tables
Prediction-interval evaluation tables
Operational-usefulness comparisons
Reproducibility

A fixed random seed is used for machine-learning and deep-learning models.

The same training and test periods are applied across the weekly forecasting models. Evaluation functions are reused to ensure that all models are compared consistently.

Limitations

The forecasting period includes unusual demand behaviour during 2020, which was not represented in the training data.

Berlin temperature is used as a representative national weather measure and may not capture regional variation across Germany.

Observed future temperature provides an informational advantage and would not normally be available for a genuine two-year operational forecast.

The effective weekly training sample is relatively small after lag and rolling features are created.
