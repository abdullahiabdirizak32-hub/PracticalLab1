# Streaming Data for Predictive Maintenance (Linear Regression Alerts)

## Project Summary
A per-axis linear regression anomaly-detection system for multichannel current data (8 axes) is implemented in this project. Time-stamped sensor streams are ingested by the notebook (lab1.ipynb), which then trains regression models, computes residuals, determines alert thresholds from residual distributions, and sends out time-based alerts (warning or critical) when residual behavior surpasses certain thresholds. 

## Table of contents
 1. project Summary
 2. Requirements & setupFile structure
 3. Quickstart
 4. Step-by-step implementation overview
 5. Regression & alert rules
 6. Threshold justification (evidence)
 7. Live Dashboard that displays alerts and errors in real time and records them
 8.  Expected outputs

## Requirements & setup
Python 3.10+

Virtualenv or Conda

Install (example using venv + pip):

Create virtual environment

Activate it

Install dependencies from requirements.txt

Suggested dependencies:

pandas, numpy, scikit-learn, matplotlib, seaborn, jupyterlab

## Quickstart
Place training_data.csv inside the data/ folder.

Launch Jupyter and open lab1.ipynb or view the exported lab1.html.

Run through the notebook in order to reproduce results and generate outputs.

## Step-by-step implementation overview
1.Load and clean the dataset
2.Parse timestamps into timezone-aware format
3.Train per-axis regression models
4.Compute residuals (differences between predicted and actual)
5.Derive thresholds from the residual distribution
6.Detect events when residuals exceed thresholds
7.Persist alerts and visualize results

## Regression & alert rules
Model: per-axis linear regression (simple, interpretable).

Residuals: actual − predicted; focus on positive residuals when high current indicates anomalies.

Thresholds: percentile-based on positive residuals (95th = warning, 99th = critical).

Event detection: requires multiple consecutive violations (windowed detection) to reduce noise.

Labels: alerts categorized into INFO / ALERT / Error depending on severity.
## Threshold justification (evidence)
The 95th percentile, or MinC, is where 95% of values fall, according to residual histograms. This cut-off minimizes excessive notifications while maintaining sensitivity.

Only about 1% of residuals surpass the 99th percentile, or MaxC, which denotes substantial deviations. This guarantees that crucial alerts are infrequent yet important.

T (window = 3 samples): Numerous single spikes were found in time series plots. While collecting persistent anomalies, requiring three consecutive exceedances significantly lowers false positives.

## Live Dashboard that displays alerts and errors in real time and records them
The live dashboard displays Alert and Erorrs per axis(axis1). Using the above thresholds, we were able to notice two alert and 1 error, meaning that there were two cases in which we were able to notice that the current had spiked and exceeded the 95th percentile for more than 3 consecutive times. There was 1 case in which we got an error, as the current had exceeded the 99th percentile.