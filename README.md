# Robo-Stock Portfolio Advisor

A Python-based automated portfolio optimization tool that combines LSTM neural networks and Monte Carlo simulation to construct market-matching investment portfolios.

## Overview

This project implements a sophisticated portfolio management system that uses machine learning for price prediction and stochastic optimization for asset allocation. The "Market Meet" strategy aims to deliver returns closely aligned with major market indices (S&P 500 and TSX Composite) while maintaining stable risk profiles.

## How It Works

### 1. Stock Filtering Pipeline
```
Raw Stock Data
    ↓
Liquidity Filter (monthly volume, currency)
    ↓
Volatility & Quality Metrics
    ↓
Composite Scoring (momentum, growth, profitability)
    ↓
Sector Diversification Constraints
    ↓
Top N Stocks Selected
```

### 2. LSTM Price Prediction
- **Architecture**: 2 LSTM layers (50 units each) + dropout (20%) + dense output layer
- **Training data**: 60-day lookback windows from all selected stocks
- **Validation**: 80/20 train-test split with performance backtesting
- **Predictions**: Next-day price forecasts for portfolio optimization

### 3. Monte Carlo Optimization
```python
For 10,000 iterations:
    1. Generate random weight vector (respecting constraints)
    2. Calculate portfolio return using LSTM predictions
    3. Measure distance from benchmark target return
    4. Track best-matching allocation
Return optimal weights
```

### 4. Portfolio Construction
- Convert optimal weights to share quantities
- Apply realistic transaction costs
- Ensure full investment of $1,000,000 capital
- Normalize final weights to 100%

## Installation

### Prerequisites
- Python 3.9 or higher
- pip package manager
- Jupyter Notebook or compatible IDE

### Setup

1. **Clone the repository**
```bash
   git clone https://github.com/yourusername/robo-stock-advisor.git
   cd robo-stock-advisor
```

2. **Install dependencies**
```bash
   pip install -r requirements.txt
```

3. **Prepare your stock universe**
   - Create a CSV file with one column containing stock tickers
   - Use `Tickers.csv` as a template
   - Example format:
```
     Ticker
     AAPL
     MSFT
     TD.TO
```

## Usage

### Running the Portfolio Advisor

1. **Open the Jupyter Notebook**
```bash
   jupyter notebook Robo_Stock_Advisor.ipynb
```

2. **Configure parameters** (in the notebook):
   - `start_date`: Historical data start date (default: "2022-05-01")
   - `end_date`: Historical data end date (default: "2025-11-22")
   - `num_stocks`: Number of stocks to select (default: 24)
   - `min_monthly_volume`: Minimum trading volume threshold (default: 100,000)

3. **Execute all cells** to:
   - Download and filter stocks
   - Train LSTM prediction model
   - Run Monte Carlo optimization
   - Generate final portfolio

### Output Files

The program generates:
- **`Final_Portfolio.csv`**: Contains selected stocks and their share quantities
  - Columns: `Ticker`, `Shares`
  - Ready for trade execution

## Performance Metrics

The strategy is designed to:
- **Match market returns**: Target ≈1.8% expected return (based on S&P 500 + TSX average)
- **Maintain diversification**: 20-24 stocks across multiple sectors
- **Control risk**: No single position exceeds 15% of portfolio
- **Minimize tracking error**: Close correlation with benchmark indices

## Example Results

From backtesting over 18 months (May 2024 - November 2025):
- **Portfolio Return**: 37.10%
- **Benchmark Return**: 37.78%
- **Tracking Difference**: -0.68%
- **Risk Profile**: Smoother trajectory than benchmark

---

**Built with**: Python, TensorFlow, NumPy, Pandas, yfinance, Matplotlib, scikit-learn
