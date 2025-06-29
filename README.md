# Kameng Hostel Trading Strategy

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Data Acquisition](#data-acquisition)
- [Preprocessing & Feature Engineering](#preprocessing--feature-engineering)
- [Machine Learning Models](#machine-learning-models)
- [Strategy Development](#strategy-development)
- [Backtesting & Results](#backtesting--results)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Repository Structure](#repository-structure)
- [Contributing](#contributing)
- [License](#license)

---

## Project Overview

This repository contains the implementation of an options data-driven trading strategy for the NIFTY BANK index using machine learning models. Inspired by a research paper from Stanford University, the project integrates advanced feature engineering, classification and regression techniques, and risk management rules to generate buy/sell signals.

## Features

- Data acquisition for NIFTY BANK index and options chain via `yfinance`
- Handling missing values and outlier removal based on normalized close
- Feature engineering including EMA returns, EMA put-to-call ratio, implied volatility metrics, and option-index volume ratio
- Classification with CatBoost to predict market direction (up/down)
- Regression with decision trees to estimate return magnitude
- Buy/Sell rules with configurable thresholds and stop-loss
- Backtesting engine and performance metrics (Profit, Drawdown, Sharpe Ratio)

## Data Acquisition

- Historical index data for NIFTY BANK from July 2023 to December 2023
- Options chain data leveraged to compute:
  - Put-to-call ratios
  - Open interest, contract volumes, and normalized prices

## Preprocessing & Feature Engineering

1. **Missing Values:**
   - Impute missing `No. of contracts`, `Open Interest`, and `Change in OI` with zero
   - Fill missing OHLC prices with the day’s close

2. **Outlier Removal:**
   - Compute `Normalized Close = Close Price / Strike Price`
   - Use IQR method to drop extreme values (~1,416 rows removed)

3. **Key Engineered Features:**
   - EMA Returns & EMA Put-to-Call Ratio
   - Implied Volatility (Call, Put, Spread)
   - Option-Index Volume Ratio (OS Volume)

## Machine Learning Models

- **CatBoost Classifier:**
  - Predicts direction (`1` for positive return, `0` for negative)
  - Hyperparameters tuned via cross-validation

- **Decision Tree Regressor:**
  - Estimates next-day return magnitude
  - Provides an additional filter with a buy threshold (e.g., 0.001)

## Strategy Development

- **Buy Condition:**
  1. Classifier output == 1
  2. Regressor predicted return > buy threshold

- **Sell Condition:**
  - Classifier output == 0 or
  - Stop-loss hit at 2% drawdown

## Backtesting & Results

| Metric                | Value    |
|-----------------------|----------|
| Profit                | 7.83%    |
| Maximum Drawdown      | 8.75%    |
| Sharpe Ratio          | 1.49     |
| Total Trades          | 27       |

## Requirements

- Python 3.8+
- pandas
- numpy
- scikit-learn
- catboost
- yfinance
- matplotlib

## Installation

```bash
# Clone the repository
git clone https://github.com/your-username/kameng-hostel-trading.git
cd kameng-hostel-trading

# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
``` 

## Usage

1. **Data Download & Preprocessing**
   ```bash
   python scripts/data_preprocessing.py
   ```

2. **Training Models**
   ```bash
   python scripts/train_models.py
   ```

3. **Backtesting Strategy**
   ```bash
   python scripts/backtest_strategy.py
   ```

4. **Visualize Results**
   ```bash
   python scripts/plot_results.py
   ```

## Repository Structure

```
├── data/                  # Raw and processed data files
├── notebooks/             # Jupyter notebooks for EDA and prototyping
├── scripts/               # Data preprocessing, training, backtesting scripts
├── models/                # Saved trained models
├── results/               # Backtest reports and plots
├── requirements.txt       # Python dependencies
└── README.md              # Project overview
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request with your enhancements.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
