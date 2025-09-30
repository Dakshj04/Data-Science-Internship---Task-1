# Task 1: Cyclone Machine Data Analysis

## Overview
Complete time-series analysis of 3 years of cyclone sensor data including:
- Data preparation & EDA
- Shutdown detection
- Machine state clustering
- Contextual anomaly detection
- Short-term forecasting
- Actionable insights

## Requirements

### Python Version
- Python 3.8 or higher

### Required Libraries
```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy statsmodels
```

Or use this complete requirements file:
```txt
pandas>=1.5.0
numpy>=1.21.0
matplotlib>=3.5.0
seaborn>=0.12.0
scikit-learn>=1.0.0
scipy>=1.7.0
statsmodels>=0.13.0
```

## Setup Instructions

### 1. Prepare Your Data
1. Export your Google Sheet as CSV:
   - File → Download → Comma Separated Values (.csv)
2. Save as `cyclone_data.csv` in the same directory as the script
3. Ensure the CSV has these columns:
   - `time`
   - `Cyclone_Inlet_Gas_Temp`
   - `Cyclone_Gas_Outlet_Temp`
   - `Cyclone_Outlet_Gas_draft`
   - `Cyclone_cone_draft`
   - `Cyclone_Inlet_Draft`
   - `Cyclone_Material_Temp`

### 2. Run the Analysis

**Option A: Jupyter Notebook**
```bash
# Convert .py to .ipynb (or create notebook with cells)
jupyter notebook task1_analysis.ipynb
```

**Option B: Python Script**
```bash
python task1_analysis.py
```

### 3. Execution Time
- Expected runtime: **10-20 minutes** depending on data size
- The script will create all outputs automatically

## Output Structure

After running, you'll have:

```
Task1/
├── shutdown_periods.csv          # All detected shutdown events
├── anomalous_periods.csv         # All detected anomalies with metadata
├── clusters_summary.csv          # Statistics for each operational state
├── forecasts.csv                 # Forecasting model performance metrics
├── plots/                        # All visualizations
│   ├── 01_correlation_matrix.png
│   ├── 02_one_week_data.png
│   ├── 03_one_year_data.png
│   ├── 04_shutdowns_one_year.png
│   ├── 05_clustering_metrics.png
│   ├── 06_cluster_analysis.png
│   ├── 07_anomaly_1.png (through 5)
│   └── 08_forecasting_results.png
└── README.md                     # This file
```

## Key Outputs Explained

### shutdown_periods.csv
- **start_time**: When shutdown began
- **end_time**: When shutdown ended
- **duration_hours**: Length of shutdown

### anomalous_periods.csv
- **start_time**: Anomaly start
- **end_time**: Anomaly end
- **duration_minutes**: How long it lasted
- **cluster**: Which operational state it occurred in
- **implicated_variables**: Which sensors showed deviation
- **severity**: Magnitude of deviation

### clusters_summary.csv
- **cluster**: Cluster ID (0-3)
- **state_name**: Interpreted state name
- **count**: Number of records
- **percentage**: % of active operation time
- **avg_inlet_temp**, etc.: Average sensor values for this state

### forecasts.csv
- Model comparison metrics (RMSE, MAE) for:
  - Persistence baseline
  - ARIMA
  - Random Forest

## Customization Options

### Adjust Shutdown Detection Sensitivity
In the code, find:
```python
temp_threshold = df['temp_mean'].quantile(0.10)  # Bottom 10%
std_threshold = df['temp_std'].quantile(0.15)    # Low variance
```
- Increase percentiles for more sensitive detection
- Decrease for stricter detection

### Change Number of Clusters
```python
optimal_k = 4  # Try 3, 5, or 6
```

### Adjust Anomaly Detection Sensitivity
```python
contamination=0.02  # 2% of data flagged as anomalies
```
- Increase for more anomalies
- Decrease for stricter detection

### Modify Forecast Horizon
```python
horizon = 12  # 12 steps = 1 hour at 5-min intervals
```

## Troubleshooting

### Issue: "File not found"
- Ensure `cyclone_data.csv` is in the same directory
- Or update the path in the script: `df = pd.read_csv('your/path/to/cyclone_data.csv')`

### Issue: "Memory Error"
- Reduce the dataset size for testing
- Use sampling: `df = df.iloc[::2]` (every 2nd row)

### Issue: ARIMA takes too long
- Reduce the sample size:
  ```python
  train_sample = train.iloc[-5000:]  # Use fewer points
  ```

### Issue: Missing imports
```bash
pip install --upgrade pandas numpy matplotlib seaborn scikit-learn scipy statsmodels
```

## Expected Results

### Typical Findings:
- **Uptime**: ~85-95% (depends on your data)
- **Shutdowns**: 50-200 events over 3 years
- **Operational States**: 4 distinct states
  - Normal Operation (~60-70%)
  - High Load (~15-20%)
  - Low Load/Startup (~10-15%)
  - Degraded/Unstable (~5-10%)
- **Anomalies**: 50-500 events (2-5% of data)
- **Forecasting RMSE**: 5-15°C for 1-hour ahead

## Performance Tips

1. **For faster execution**:
   - Reduce ARIMA fitting frequency
   - Use fewer Random Forest estimators
   - Sample the data for clustering

2. **For better accuracy**:
   - Increase Random Forest estimators to 100
   - Use more lag features
   - Fine-tune ARIMA parameters with auto_arima

3. **For large datasets (>500k rows)**:
   - Process in chunks
   - Use Dask or parallel processing
   - Reduce plot resolution


**Author**: Daksh Jain  
**Date**: 2025-09-30  
**Project**: ExactSpace Data Science Internship Assessment