# Robo-Stock Portfolio Advisor

A Python-based automated portfolio optimization tool that combines LSTM neural networks and Monte Carlo simulation to construct market-matching investment portfolios.

## Overview

This project implements a sophisticated portfolio management system that uses machine learning for price prediction and stochastic optimization for asset allocation. The "Market Meet" strategy aims to deliver returns closely aligned with major market indices (S&P 500 and TSX Composite) while maintaining stable risk profiles.

## Key Features

### 🤖 Machine Learning Price Prediction
- **LSTM Neural Networks**: Deep learning model trained on 3+ years of historical price data
- **Multi-stock training**: Global model learns patterns across 20+ securities simultaneously
- **Backtesting**: Validates predictions against actual market performance with visualization

### 📊 Intelligent Stock Filtering
- **Multi-factor analysis**: Evaluates momentum (3M, 6M, 12M), revenue growth, profit margins, ROE, and relative strength
- **Composite scoring**: Weighted ranking system to identify high-potential securities
- **Market cap diversity**: Ensures mix of large-cap, mid-cap, and small-cap stocks
- **Sector constraints**: Prevents over-concentration in any single sector (max 40%)
- **Liquidity requirements**: Filters for minimum monthly trading volume and currency (USD/CAD only)

### 💼 Portfolio Optimization
- **Monte Carlo simulation**: 10,000+ iterations to find optimal weight allocation
- **Target matching**: Aims to replicate market benchmark returns (S&P 500 + TSX average)
- **Investment constraints**: 
  - Minimum position: ~2.08% (1/48 of portfolio)
  - Maximum position: 15% (prevents over-concentration)
  - Full investment: Weights sum to 100%
- **Transaction cost modeling**: Incorporates broker fees ($2.15 minimum + $0.001/share)

### ⚡ Performance Optimization
- **Parallel processing**: Multi-threaded Monte Carlo simulation for faster computation
- **Currency conversion**: Automatic USD/CAD exchange rate handling
- **Efficient data structures**: Optimized for handling large datasets

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

## Technical Details

### Model Architecture
- **Input**: 60-day price sequences, normalized to [0,1] range
- **Hidden layers**: 2 × LSTM(50 units) with 20% dropout
- **Optimizer**: Adam with MSE loss function
- **Training**: 5 epochs with 10% validation split

### Data Requirements
- Minimum 100 trading days of historical data per stock
- At least 18 trading days per month for volume calculations
- Valid monthly volume data for liquidity assessment

### Constraints Applied
| Parameter | Minimum | Maximum |
|-----------|---------|---------|
| Position weight | 2.08% (1/48) | 15.0% |
| Sector allocation | - | 40.0% |
| Market cap | $2B CAD | - |
| Monthly volume | 100,000 shares | - |

## Limitations & Considerations

- **Historical performance**: Past results don't guarantee future returns
- **Market conditions**: Model trained on recent bull market data
- **Transaction costs**: Actual fees may vary by broker
- **Currency risk**: CAD/USD fluctuations impact returns
- **Model retraining**: Should be updated periodically with new data
- **Computational resources**: Full run takes ~15-30 minutes depending on hardware

## Future Enhancements

Potential improvements:
- [ ] Real-time data streaming and continuous rebalancing
- [ ] Additional ML models (XGBoost, Transformer architectures)
- [ ] Risk parity and minimum variance portfolio strategies
- [ ] Sentiment analysis integration from news/social media
- [ ] Tax-loss harvesting optimization
- [ ] ESG factor incorporation

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is provided for educational purposes. Use at your own risk. Not financial advice.

## Disclaimer

⚠️ **Important**: This tool is for educational and research purposes only. It does not constitute financial advice. Always:
- Consult with licensed financial advisors before making investment decisions
- Conduct your own due diligence
- Understand that all investments carry risk
- Never invest money you cannot afford to lose

Past performance does not guarantee future results. The authors assume no liability for financial losses incurred through use of this software.

## Contact

For questions or feedback, please open an issue on GitHub.

---

**Built with**: Python, TensorFlow, NumPy, Pandas, yfinance, Matplotlib, scikit-learn
