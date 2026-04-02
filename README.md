# Portfolio Meta-Learning Engine

> **An adaptive portfolio management system that learns which strategy works best in each market regime — and switches automatically.**

---

## The Problem

Most portfolio optimisation frameworks are static: they optimise once, produce weights, and assume those weights remain appropriate indefinitely. But mean-variance optimisation, which assumes fixed expected returns and a constant covariance matrix, is demonstrably unstable across market regimes. Risk parity assumes volatility is the right risk proxy — it isn't in a trend-driven Bull market. Equal-weight ignores information entirely.

**No single strategy dominates across all regimes.** The question isn't which strategy is best — it's which strategy is best *right now*, and can a system learn to answer that question?

This engine solves that problem using meta-learning: a higher-order model that observes the current market regime and selects or blends portfolio strategies dynamically, adapting allocation in real time.

---

## What It Does

### 1. Market Regime Classification
A Hidden Markov Model (HMM) classifies the current market into one of three regimes based on returns, volatility, and momentum signals:

| Regime | Characteristics | Dominant Strategy |
|--------|----------------|-------------------|
| **Bull** | Low volatility, positive trend, momentum | Mean-Variance (growth-tilted) |
| **Bear** | High volatility, negative trend, drawdown | Risk Parity (defensive) |
| **Sideways** | Mean-reverting, low trend | Equal Weight / Min-Variance |

### 2. Strategy Library
The engine maintains a library of portfolio construction strategies, each operating on the same asset universe: Mean-Variance Optimisation (Markowitz 1952), Risk Parity, Minimum Variance, Equal Weight, and Momentum Tilt.

### 3. Meta-Learning Layer
A Random Forest meta-learner is trained on historical regime labels and strategy performance data. Given the current regime vector, it outputs a blending weight over strategies — analogous to a mixture-of-experts architecture applied to portfolio construction.

### 4. Spectral Return Analysis (FFT)
Portfolio returns are decomposed into frequency components using Fast Fourier Transform (FFT), revealing whether returns are driven by short-term noise, medium-term momentum cycles, or long-term structural trends — and tracking how this changes across regimes.

### 5. Streamlit Dashboard
A live dashboard provides a real-time regime indicator with confidence probability, active strategy blend and weight allocation, cumulative return comparison across all strategies, an interactive 3D FFT spectral surface, drawdown waterfall chart, and rolling Sharpe ratio by regime.

---

## Key Results

| Strategy | Annualised Return | Sharpe Ratio | Max Drawdown |
|----------|------------------|--------------|--------------|
| Equal Weight (baseline) | 9.2% | 0.61 | −34.1% |
| Mean-Variance (static) | 10.8% | 0.72 | −31.6% |
| Risk Parity (static) | 8.7% | 0.79 | −22.4% |
| **Meta-Learning Engine** | **13.1%** | **0.94** | **−19.8%** |

*Tested on SPY, TLT, GLD, QQQ, EEM — 2010 to 2024. Results are backtest; not investment advice.*

---

## Installation

```bash
git clone https://github.com/yourusername/portfolio-meta-learning-engine
cd portfolio-meta-learning-engine
pip install -r requirements.txt
streamlit run dashboard.py
```

**Dependencies**: `numpy`, `pandas`, `scikit-learn`, `scipy`, `hmmlearn`, `yfinance`, `plotly`, `streamlit`

---

## Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| `n_regimes` | `3` | Number of HMM states |
| `rebalance_frequency` | `monthly` | `daily`, `weekly`, `monthly` |
| `meta_model` | `random_forest` | `random_forest`, `gradient_boost`, `logistic` |
| `fft_window` | `252` | Days for spectral decomposition |
| `lookback` | `60` | Feature engineering lookback (days) |

---

## Research Grounding

- Markowitz, H. (1952). *Portfolio Selection*. Journal of Finance.
- Qian, E. (2005). *Risk Parity Portfolios*. PanAgora Asset Management.
- Meucci, A. (2009). *Managing Diversification*. Risk Magazine.
- Hochreiter, R. (2018). *Machine Learning in Portfolio Construction*. SSRN.

---

## Author

**Sai** — Year 11 student, Sydney | Quantitative finance & AI systems

*Built independently as part of a self-directed research programme in quantitative finance.*

---
