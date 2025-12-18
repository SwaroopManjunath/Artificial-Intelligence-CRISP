# NASDAQ Market Pattern Prediction

Predicting 1-week NASDAQ trends and market patterns using machine learning and the CRISP-DM methodology.

## What This Project Does

Rather than trying to predict exact stock prices (which is nearly impossible), we predict **market patterns** and **trend direction** for the NASDAQ Composite Index.

**Two Predictions:**
1. **Weekly Trend (5 classes):** Strong Down, Weak Down, Neutral, Weak Up, Strong Up
2. **Market Pattern (5 types):** Uptrend, Downtrend, Consolidation, Breakout, Reversal

**Why this matters:** Knowing "we're in a downtrend" is more actionable than a wrong price prediction. Traders can adjust position sizing and strategy based on market regime.

## Key Findings

### Model Performance

| Metric | Value | Interpretation |
|--------|-------|----------------|
| **Best Model** | XGBoost | Gradient boosting classifier |
| **Accuracy** | ~46% | **2.3x better than random** (20% baseline) |
| **Dataset Size** | 3,700+ samples | 15 years of NASDAQ data (2010-2025) |
| **Features** | 37 | Technical indicators + trend patterns |
| **Forecast Horizon** | 1 week | 5 trading days ahead |

### Why 46% Accuracy Is Good

- **Random baseline:** 20% (5 equal classes)
- **Financial markets are noisy:** Even 50-60% accuracy is considered excellent
- **Focus on extremes:** Predicting "Strong Up" and "Strong Down" correctly is most valuable
- **Actionable insights:** Combined with pattern recognition, enables context-aware trading

## Data & Features

**Source:** Yahoo Finance NASDAQ (^IXIC) daily OHLCV data

**Feature Engineering:**
- **Trend indicators:** SMA, EMA, ADX, directional indicators
- **Momentum:** RSI, MACD 
- **Volatility:** Bollinger Bands, ATR
- **Volume:** OBV, volume surge detection
- **Patterns:** Swing analysis, trend slope, BB squeeze
- **Lag features:** Previous 5 days' closing prices

**Target Creation:** Dynamic percentile-based thresholds (adapts to market volatility)

## Project Structure

```
notebooks/
├── 01_EDA_Data_Preparation.ipynb    # Data analysis & feature engineering
└── 02_Modeling_Evaluation.ipynb     # Model training & evaluation
data/
├── raw/                              # Raw NASDAQ data
└── processed/                        # Engineered features + targets
```

## Quick Start

```bash
pip install -r requirements.txt
jupyter notebook notebooks/01_EDA_Data_Preparation.ipynb
```

## Technical Highlights

### What Makes This Project Rigorous

✅ **No data leakage:** Future data never used for training (proper `.shift()` for targets)  
✅ **Balanced classes:** Dynamic percentile thresholds ensure 20% per class  
✅ **Time-series aware:** Walk-forward validation, no random train/test shuffle  
✅ **Comprehensive features:** 37 indicators covering trend, momentum, volatility, volume  
✅ **Dual prediction task:** Both "what will happen" (weekly trend) and "what's the context" (pattern)

### Key Innovations

- **Trend Pattern Classification:** Priority-based logic (Reversal → Breakout → Consolidation → Trends)
- **ADX + Directional Indicators:** Separates strong trends from ranging markets
- **Bollinger Band Squeeze + Volume:** Detects breakout opportunities
- **Dynamic Thresholds:** Adapts to changing market volatility (vs. fixed ±σ)

## Repository Structure

```
├── notebooks/
│   ├── 01_EDA_Data_Preparation.ipynb    # Data analysis, feature engineering
│   └── 02_Modeling_Evaluation.ipynb     # Model training, evaluation
├── data/
│   ├── raw/                              # Raw NASDAQ OHLCV data
│   └── processed/                        # Engineered features + targets
├── PROJECT_DOCUMENTATION.md              # Complete technical documentation
├── DOCUMENTATION.md                      # Additional notes
├── requirements.txt                      # Python dependencies
└── README.md                             # This file
```

## Quick Start

```bash
# Install dependencies
pip install -r requirements.txt

# Run notebooks in order
jupyter notebook notebooks/01_EDA_Data_Preparation.ipynb
jupyter notebook notebooks/02_Modeling_Evaluation.ipynb
```

## Results Interpretation

**What 46% accuracy means:**
- Out of 100 predictions, 46 are correct (vs. 20 expected by chance)
- Extreme classes (Strong Up/Down) are most valuable → focus on precision for these
- Combined with pattern labels, provides context for risk management

**Trading Application:**
- Strong Up + Breakout pattern → High-confidence long entry
- Strong Down + Reversal pattern → Exit longs or short entry
- Neutral + Consolidation → Reduce position size, wait for clarity

## Methodology: CRISP-DM

1. **Business Understanding:** Define trend prediction as classification problem
2. **Data Understanding:** Analyze 15 years of NASDAQ data, identify regimes
3. **Data Preparation:** Engineer 37 features, create balanced targets
4. **Modeling:** Train/evaluate Random Forest, XGBoost, LSTM (next phase)
5. **Evaluation:** Compare against baseline, analyze confusion matrix
6. **Deployment:** Document findings, create reproducible pipeline

## Authors

- Varun H Shamaraju
- Swaroop K Manjunathaswamy
