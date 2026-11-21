# Predictive Analysis of US Flight Delays to JFK

Group project for the *Statistical Learning* course (MSc Data Science, University of Padova,
2023/2024). We study how departure delay, flight characteristics and the weather at the departure
airport affect the arrival delay of flights landing at New York JFK in 2023, with two tasks:

- **Regression**: how long is the arrival delay of a delayed flight?
- **Classification**: will a flight arrive late or not?

The analysis is written in R Markdown.

**Full report:** [report/flight_delays_jfk_report.pdf](report/flight_delays_jfk_report.pdf)

## Data

- All US civil flights of 2023 (6.7 million rows) from Kaggle, filtered to the flights arriving at
  JFK.
- Daily weather at the departure airport (temperatures, precipitation, snow, wind, pressure),
  merged on date and airport.
- After cleaning: 129,542 flights, of which 49,597 arrived late.
- Time-based split: January-September for training, October-December for testing.

See [data/README.md](data/README.md) for how to download the files.

## Methods

- Exploratory analysis: boxplots, outlier removal, covariance and correlation.
- Regression on the delayed flights:
  - simple linear regression on departure delay;
  - polynomial regression (degrees 2 and 3);
  - multiple linear regression with backward stepwise selection based on AIC and BIC;
  - residual diagnostics and collinearity checks (VIF).
- Classification of delayed vs on-time flights: logistic regression (full and reduced), LDA and
  QDA, compared with ROC curve, AUC and test accuracy.

## Results

**Regression** (test MSE on October-December flights):

| Model | R² (train) | Test MSE |
| --- | --- | --- |
| Simple linear (departure delay) | 0.968 | 284.2 |
| Quadratic | 0.969 | 262.5 |
| Multiple (after AIC/BIC selection) | 0.969 | 262.8 |
| Cubic | 0.970 | 249.0 |

Departure delay explains almost all the variance of the arrival delay: each minute of departure
delay adds about 0.96 minutes at arrival. Weather and flight variables add little on top of it, and
the cubic model gives the lowest test error.

**Classification** (test set):

| Model | Accuracy |
| --- | --- |
| Logistic regression (reduced) | **83.7%** (AUC 0.858) |
| QDA | 83.1% |
| LDA | 77.6% |

## Getting Started

1. Download the data as described in [data/README.md](data/README.md).
2. Open `us-flight-delays-jfk.Rproj` in RStudio, so that the working directory is the project
   folder.
3. Install the required packages:
   ```r
   install.packages(c("corrplot", "leaps", "car", "MLmetrics", "pROC", "MASS", "e1071", "rmarkdown"))
   ```
4. Open `flight_delays_jfk.Rmd` and click **Knit** to produce the HTML report.

## Authors

Arianna Morandi, Beatrice Spagnolo, Tomas Guarini.
