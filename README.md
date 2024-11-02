# Marketing Mix Modeling

## Project Overview
This project focuses on building a time series forecasting model using the Prophet library in R to predict future sales based on historical data. The model and project materials include exploratory data analysis, data preprocessing, and forecasts for future values. Results are presented in an R Markdown file that shows the entire pipeline from data preparation to visualization.

## Files and Structure
`data.zip`: Compressed folder containing the datasets used in the project, including:
    `train.csv` - historical sales data
    `test.csv` - future dates to forecast
    `stores.csv` - store-specific information
    `oil.csv` - oil price data
    `holidays_events.csv` - holiday and event data

The data was taken from [this Kaggle competition](https://www.kaggle.com/c/store-sales-time-series-forecasting/data)

`eda.ipynb`: Jupyter notebook for exploratory data analysis (EDA). Contains initial data exploration, general trends, and insights into the data's structure and quality.

`forecasting.Rmd`: The main R Markdown file that contains the entire workflow of the project, including:
Data loading and preprocessing
Exploratory data analysis and visualizations
Time series decomposition and ACF/PACF plots
Model training using Prophet with multiple configurations (baseline, features, parameters)
Forecast generation and evaluation

`forecasting.html`: The knitted HTML output of forecasting.Rmd, showing results, visualizations, and code for the project.

`model*`: files with fitted models

`./robyn`: a folder with an attempt to do the task using Robyn library (before knowing that the Prophet library was actually meant), don't look there

## Requirements
To reproduce this project, ensure that the following libraries are installed in R:

```
install.packages(c("dplyr", "ggplot2", "readr", "lubridate", "tidyr", "gridExtra", "tidyverse", "zoo", "forecast", "prophet", "Metrics"))
```

## Project Workflow
#### 1. Data Loading and Preprocessing
The project begins by loading and cleaning the data:

* Parsing dates
* Merging datasets (stores.csv, oil.csv, and holidays_events.csv) with train.csv and test.csv
* Handling missing values with imputation or substitutions as needed

#### 2. Exploratory Data Analysis (EDA)
EDA in eda.ipynb and forecasting.Rmd provides insights into:

* Sales trends over time
* Sales by product family, holiday type, and promotion effects
* Oil price trends and their relationship to sales
* Seasonal patterns in the sales data (day of the week, holidays)

#### 3. Time Series Decomposition and Analysis
The project analyzes the main components of the time series, using STL decomposition to extract seasonality, trend, and residuals, as well as examining ACF and PACF plots to understand autocorrelation in the data.

#### 4. Model Training with Prophet
The following models were trained on the data:

* Baseline model: Basic Prophet model trained on historical sales data
* All features model: Prophet model with all dataset features as regressors
* Some features model: Prophet model using a subset of impactful regressors
* Parameterized model: Prophet model with holidays and custom changepoints, and selected regressors for refined forecast accuracy

All models were saved as .RDS files for reproducibility.

#### 5. Forecasting and Evaluation
Forecasts are generated on the validation set, and predictions from each model configuration are evaluated and visualized. Plots show the comparison between actual and forecasted sales values.

#### 6. Visualization and Reporting
The report in forecasting.html documents the entire process, including code, comments, and visualizations.

## Results:

I used MAE, MSE and RMSE metrics to evaluate the fitted models. The results are the following:

| Model                 |   MAE     |   MSE      |   RMSE    |
|-----------------------|-----------|------------|-----------|
| Baseline Model        | 662.2  | 1 848 150    | 1 359.5  |
| All Features Model    | 429.5  | 1 253 813    | 1 119.7  |
| Some Features Model   | 437.5  | **1 223 101**    | **1 105.9**  |
| Model with Parameters  | **428.0**  | 1 250 384    | 1 118.2  |

We can see that all the models beat the baseline and the metrics are pretty similar. It is also worth mentioning that the target variable (total sales) is of order ~1e6 so the computed metrics are not that scary. Based on this, the best model turns out to be "some features model" which uses the following featurs as regressors: 
* family
* onpromotion
* city
* type.store
* dcoilwtico (oil prices)
* type.holiday

For graphs and visualizations please see the `forecasting.html` or `forecasting.Rmd`.