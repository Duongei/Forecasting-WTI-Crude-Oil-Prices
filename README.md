# Modeling and Forecasting Monthly WTI Crude Oil Prices Using ARIMA and ARDL

This repository contains the dataset, notebooks, figures, and final report for a time-series analysis project on monthly West Texas Intermediate (WTI) crude oil prices from 2010 to 2025. The project applies ARIMA as a univariate forecasting benchmark and ARDL as a macro-financial relationship model to study the role of the U.S. Dollar Index and the Federal Funds Effective Rate.

## Project Title

**Modeling and Forecasting Monthly WTI Crude Oil Prices Using ARIMA and ARDL: The Role of the U.S. Dollar Index and the Federal Funds Rate, 2010–2025**

## Research Objectives

1. Evaluate whether past WTI crude oil price dynamics can provide useful short-run forecasts using ARIMA models.
2. Examine whether the U.S. Dollar Index and the Federal Funds Effective Rate help explain the short-run and long-run movements of WTI crude oil prices using an ARDL framework.

## Research Questions

1. Can ARIMA effectively model and forecast monthly WTI crude oil prices during 2010–2025?
2. What are the short-run and long-run relationships between WTI crude oil prices, the U.S. Dollar Index, and the Federal Funds Effective Rate?
3. Do macro-financial variables such as the U.S. Dollar Index and interest rates improve the understanding of WTI crude oil price dynamics beyond a purely univariate ARIMA model?

## Main Hypotheses

- Monthly WTI crude oil prices, the U.S. Dollar Index, and the Federal Funds Rate are expected to be non-stationary in levels but stationary after first differencing.
- The U.S. Dollar Index is expected to have a negative relationship with WTI crude oil prices.
- The effect of the Federal Funds Rate on WTI prices is expected to be dynamic and ambiguous in sign.
- If a long-run relationship exists, the error correction term should be negative and statistically significant.

## Dataset

The final dataset contains monthly observations from **January 2010 to December 2025**, with **192 observations** and no missing values.

### Variables

- `WTI_Oil`: Monthly WTI crude oil price, measured in U.S. dollars per barrel.
- `USD_Index`: U.S. Dollar Index.
- `Interest_Rate`: Federal Funds Effective Rate.

### Data Construction

Daily WTI crude oil price data and daily U.S. Dollar Index data were aggregated to monthly frequency. The Federal Funds Effective Rate was aligned at the monthly level. The final cleaned dataset is balanced, continuous, and suitable for time-series analysis.

For modeling:

- `Log_WTI = log(WTI_Oil)`
- `Log_USD = log(USD_Index)`
- `Interest_Rate` is kept in level form.
- First differences are used for stationarity testing:
  - `D_Log_WTI`
  - `D_Log_USD`
  - `D_Interest_Rate`

## Repository Structure

```text
.
├── data/
│   └── cleaned monthly dataset and related data files
│
├── figures/
│   └── figures used in the exploratory data analysis and report
│
├── notebook/
│   └── Jupyter notebooks for data cleaning, EDA, ARIMA, and ARDL modeling
│
├── report/
│   └── LaTeX report files, references, and final report outputs
│
└── README.md
```

## Methodology

### 1. Exploratory Data Analysis

The EDA examines:

- Time-series trends of WTI, USD Index, and Federal Funds Rate.
- Year-over-year changes.
- Normalized comparison between WTI and the U.S. Dollar Index.
- STL decomposition of log WTI prices.
- Histograms and boxplots of level and transformed variables.
- Rolling volatility of WTI log changes.
- Correlation analysis among level and transformed variables.

The EDA shows that WTI prices experienced several major regimes, including the 2014–2016 oil price collapse, the COVID-19 shock in 2020, the strong recovery in 2021–2022, and the gradual decline after 2022.

### 2. Stationarity Testing

Phillips–Perron unit root tests are applied to the training sample.

Main finding:

- `Log_WTI`, `Log_USD`, and `Interest_Rate` are non-stationary in levels.
- Their first differences are stationary.

Therefore, the variables are treated as integrated of order one, I(1). This supports the use of ARIMA with first differencing and validates the ARDL framework because none of the variables appears to be I(2).

### 3. ARIMA Modeling

ARIMA is used as the univariate forecasting benchmark.

Main steps:

- Use `Log_WTI` as the dependent variable.
- Set differencing order `d = 1`.
- Inspect ACF and PACF of first-differenced log WTI.
- Estimate candidate ARIMA models.
- Compare models using AIC, BIC, residual diagnostics, and forecast performance.
- Evaluate both fixed multi-step forecasts and rolling one-step-ahead forecasts.

### 4. ARDL Modeling

ARDL is used to examine the relationship between WTI prices, the U.S. Dollar Index, and the Federal Funds Effective Rate.

Main specification:

```text
Log_WTI = f(Log_USD, Interest_Rate)
```

ARDL is used because it can model short-run dynamics and possible long-run relationships when variables are I(0), I(1), or mixed I(0)/I(1), as long as no variable is I(2).

## Main Results

### ARIMA Results

- AIC selects ARIMA(2,1,0).
- BIC selects ARIMA(0,1,1).
- Rolling one-step forecast performance selects ARIMA(2,1,1) as the best short-run forecasting model.
- ARIMA(0,1,0), the random walk benchmark, performs very close to the best ARIMA model.

Rolling one-step-ahead forecast performance:

```text
ARIMA(2,1,1): MAPE ≈ 4.82%
Random walk ARIMA(0,1,0): MAPE ≈ 4.91%
```

Interpretation:

ARIMA performs well for short-run rolling forecasts, but the improvement over the random walk benchmark is modest. This suggests that monthly WTI prices are highly persistent and difficult to forecast beyond recent price information.

### ARDL Results

The BIC-selected ARDL model is:

```text
ARDL(2,1,3)
```

This model is preferred because it is more parsimonious than the AIC-selected model, passes residual diagnostics, and performs slightly better in forecast checks.

Residual diagnostics indicate:

- No strong residual autocorrelation.
- No evidence of non-normality from the Jarque–Bera test.
- No clear remaining ARCH effects.

Bounds test result:

```text
Bounds statistic ≈ 3.9024
Upper-bound p-value ≈ 0.0834
```

Interpretation:

The bounds test provides weak evidence of cointegration at the 10% significance level, but not at the 5% level. Therefore, long-run coefficients should be interpreted cautiously.

Error correction coefficient:

```text
Log_WTI.L1 ≈ -0.1197
```

Interpretation:

Approximately 12% of deviations from the possible long-run equilibrium are corrected each month.

Long-run coefficient estimates:

```text
Log_USD: approximately -3.30
Interest_Rate: approximately 0.049
```

The negative coefficient on `Log_USD` is consistent with the expected inverse relationship between dollar strength and crude oil prices.

### Forecast Comparison

Fixed multi-step forecasts and rolling one-step-ahead forecasts provide different information.

Fixed multi-step forecast:

- ARIMA performs better than ARDL.
- ARDL fixed dynamic forecasts perform poorly because errors accumulate over a long forecast horizon.

Rolling one-step-ahead forecast:

- ARIMA(2,1,1) performs best.
- ARDL(2,1,3) performs reasonably but is weaker than ARIMA in forecast accuracy.
- ARDL remains useful for economic interpretation rather than pure forecasting performance.

## Key Interpretation

The project finds that ARIMA is more suitable for short-run forecasting, especially in a rolling one-step-ahead framework. ARDL is more useful for interpreting the role of macro-financial variables, especially the U.S. Dollar Index and the Federal Funds Effective Rate.

In summary:

```text
ARIMA = better short-run forecasting model
ARDL  = better economic relationship model
```

## Limitations

The project has several limitations:

- ARIMA is univariate and cannot directly incorporate macroeconomic shocks, geopolitical events, OPEC decisions, supply disruptions, or monetary policy variables.
- ARDL includes only the U.S. Dollar Index and the Federal Funds Rate, while WTI prices are also influenced by global demand, supply, inventories, geopolitical risks, and futures market expectations.
- ARDL forecasts are conditional because actual out-of-sample values of the U.S. Dollar Index and the Federal Funds Rate are used.
- The bounds test provides only weak evidence of cointegration at the 10% level.
- WTI price volatility is time-varying, suggesting that GARCH-type volatility models could be considered in future research.

## How to Run the Project

1. Clone this repository.

```bash
git clone <repository-url>
cd <repository-folder>
```

2. Install the required Python packages.

```bash
pip install -r requirements.txt
```

3. Open the notebooks in the `notebook/` folder.

```bash
jupyter notebook
```

4. Run the notebooks in order:

```text
1. Data cleaning and preparation
2. Exploratory data analysis
3. Stationarity testing, ARIMA modeling, and ARDL modeling
```

5. Compile the LaTeX report in the `report/` folder.

## Suggested Python Requirements

The project uses the following main Python libraries:

```text
pandas
numpy
matplotlib
statsmodels
arch
scipy
scikit-learn
jupyter
```

## Report

The final report is written in LaTeX and is located in the `report/` folder. It includes:

- Introduction
- Literature review
- Data and variable construction
- Exploratory data analysis
- Methodology
- Empirical results
- Limitations
- Conclusion
- References

## Author

**Pham Khanh Duong**  
Time Series Analysis  
National Economic University

## License

This repository is created for academic and educational purposes.
