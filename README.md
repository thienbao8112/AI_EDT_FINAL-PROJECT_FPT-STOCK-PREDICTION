# FPT Stock Price Prediction — ARIMA · Random Forest · LSTM

End-to-end time-series forecasting project predicting the **close price of FPT Corporation** (ticker: FPT, HOSE) using three approaches: ARIMA, Random Forest, and LSTM.

💻 Demo: [YouTube](https://www.youtube.com/watch?v=NutuEvZsEeE&ab_channel=lybao)

---

## Project Contents

| File | Description |
|---|---|
| `01_fpt_stock_historical_data.csv` | FPT daily stock OHLCV data, Dec 2006 – Dec 2023, sourced from Investing.com |
| `02_fpt_stock_price_prediction.ipynb` | Main analysis notebook (OSEMN pipeline: Obtain → Scrub → Explore → Model → Interpret) |
| `03_methodology_diagram.png` | Visual overview of the project methodology |
| `04_final_report.pdf` | Full written report submitted for course ML704 |

---

## Analytical Workflow (OSEMN)

### 1. Obtain
- FPT stock data (Open, High, Low, Close, Volume) from Dec 2006 – Dec 2023.
- Company overview, shareholders, governance, financial ratios fetched live via `vnstock`.
- Raw CSV loaded locally from `01_fpt_stock_historical_data.csv`.

### 2. Scrub
- Renamed Vietnamese column headers to English (`Ngày → Date`, `Lần cuối → Close`, etc.).
- Converted `Date` to `datetime` index, sorted ascending.
- Converted OHLC strings (comma-separated thousands, `M`/`K` suffixes) to `float`.
- Dropped missing values and duplicate rows (4 245 → 4 244 records).

### 3. Explore
- Candlestick chart (Bokeh) showing full price history and key market events (2008 GFC, COVID-19 2020, recession 2022).
- Distribution histograms — Close/Open/High/Low skewness ≈ 1.49, kurtosis ≈ 0.81.
- Correlation heatmap — strong correlation between OHLC due to Vietnam SBV ±7% daily limit rule.

### 4. Model
| Model | Notes |
|---|---|
| **ARIMA** | Stationarity tested (ADF, KPSS), ACF/PACF used for order selection |
| **Random Forest** | Scikit-learn `RandomForestRegressor` with `GridSearchCV` |
| **LSTM** | TensorFlow/Keras with `MinMaxScaler`, sequence-based sliding window |

### 5. Interpret
- Models evaluated and compared on RMSE and visual fit.
- Key finding: LSTM captures long-term trend best; ARIMA suitable for short-term stationary windows.

---

## Run Instructions

### Install dependencies
```bash
pip install pandas numpy matplotlib seaborn bokeh statsmodels scikit-learn tensorflow tqdm vnstock
```

### Run the notebook
```bash
jupyter notebook 02_fpt_stock_price_prediction.ipynb
```

> **Note:** The notebook loads data from `01_fpt_stock_historical_data.csv` in the same folder. Keep both files together.

---

## Key Libraries

- **Data**: `pandas`, `numpy`
- **Visualisation**: `matplotlib`, `seaborn`, `bokeh`
- **Statistics / ARIMA**: `statsmodels`
- **Machine Learning**: `scikit-learn` (RandomForest, GridSearchCV, MinMaxScaler)
- **Deep Learning**: `tensorflow` / `keras` (LSTM)
- **Stock Data API**: `vnstock`

---

## Dataset Notes

- Source: [Investing.com](https://www.investing.com) (manually downloaded; no API available).
- Period: 14 Dec 2006 – 26 Dec 2023 (~17 years, 4 244 trading days).
- Average close price: ~26 010 VND; Max: 99 000 VND; Min: 2 985.5 VND.
