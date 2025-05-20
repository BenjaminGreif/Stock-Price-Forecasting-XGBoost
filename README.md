# 🚀 Lightning‑Fast Multihorizon Stock Forecasting & Automated Trading (XGBoost‑Powered)

> Fully automated, model-driven trading on a balanced set of 10 large-cap stocks.  
> Powered by weekly XGBoost forecasts — no human overrides, no heuristics.  
> Outperformed Buy-&-Hold by 11.38 % and the Dow Jones by 3.88 % in a 52-week backtest.  
> Total ROI: +31.24 % ($100k → $131k), delivered in under 1,000 lines of clean Python.

---

## 📌 At‑a‑Glance

| 📊 Metric      | 🔥 My Bot | 🟩 Buy‑&‑Hold | 🏩 Dow Jones |
| -------------- | --------- | ------------- | ------------ |
| Final Capital  | \$131,238 | \$119,833     | \$127,357    |
| 52‑Week Return | +31.24 %  | +19.83 %      | +27.36 %     |

### 🎯 Directional Accuracy Breakdown

| Horizon  | Model Accuracy | Industry Baseline\* |
| -------- | -------------- | ------------------- |
| 6 Weeks  | 62 %           | 50–55 %             |
| 12 Weeks | 55 %           | 50–55 %             |
| 52 Weeks | 52 %           | 50–55 %             |

\*Baseline drawn from [Kamalov et al. (2021), “Forecasting with Deep Learning: S&P 500 Index”](https://arxiv.org/abs/2103.14080)


---

## 🌟 Project Objective

Showcase how a fast, interpretable XGBoost model, trained per ticker on short-term momentum & trend signals, can drive profitable weekly trading without deep-learning latency or leakage.

---

## 🔑 Why This Project Stands Out

* Real‑Money KPI > RMSE — judged in dollars, not abstract loss
* Per-Ticker Models — better specialization than a global regressor
* Leakage‑Proof Forecasting — iterative predictions using only known/predicted data
* Pipeline Speed — backtest + retraining in under a minute
* Balanced Sample — avoids cherry-picking FAANG outperformance

---

## 👨‍💼 Tech Stack

| Layer          | Tools                                           |
| -------------- | ----------------------------------------------- |
| Language       | Python 3.11                                     |
| Core ML        | XGBoost 1.7 (tree\_method=hist, CPU‑only)       |
| Data Wrangling | Pandas / NumPy                                  |
| TA Indicators  | Momentum\_1/2/3, EMA‑SMA delta, Momentum\_Combo |
| Backtest       | Vectorized NumPy simulator                      |
| Visuals        | Matplotlib                                      |
| Notebooks      | Jupyter Lab                                     |

---

## 🗺️ Repository Map

```
├─ notebooks
│  ├─ XGBOOST_Forecasting.ipynb       # feature eng. + per-stock training
│  └─ Trading_Simulation.ipynb        # backtest & P&L analytics
├─ data
│  ├─ stock_sample_predictions_6_weeks.json
│  ├─ stock_sample_predictions_12_weeks.json
│  └─ stock_sample_predictions_52_weeks.json
└─ README.md  (you are here)
```

---

## 🛠️ Methodology

1. Feature Engineering
 For each stock individually:

* Compute daily close-to-close % changes
* Momentum features: Momentum\_1, Momentum\_2, Momentum\_3
* Trend features: EMA – SMA deltas (Trend\_2–5)
* Composite signal: Momentum\_Combo = 0.5 × M1 + 0.3 × M2 + 0.2 × M3
* All features standardized using a stock-specific StandardScaler

2. Model Training (Per Stock)

* Train separate XGBoost regressors for each ticker
* Target: next-day % return (Change)
* Exclude the final (weeks + 1) × 5 rows to avoid lookahead bias
* Conservative hyperparams (low learning rate, regularization)

3. Forecasting Logic

* Iteratively simulate 5-day forecasts using only known/predicted data
* After each prediction: update Close, recalc features, rescale, repeat
* All predictions are serialized into JSON files (one per horizon)
* These files are then loaded by the trading simulator to ensure strict data separation

4. Trading Simulation

* Each Friday: rank all tickers by cumulative 5-day forecast
* Enter top 3 tickers equally weighted
* Exit if gain > +7 %, loss < −4 %, or after 10 days
* Force liquidation every Friday before new cycle

5. Backtest & Evaluation

* Rolling weekly backtest over 52 weeks
* Metrics: ROI, final capital, directional accuracy
* Benchmarks: Buy-&-Hold, Dow Jones Index
* 0.2 % fee per side, no data leakage

---

## 📈 Key Insights

* Edge compounds — 62 % model hit-rate → +11 % alpha vs Buy-&-Hold
* Multi-day forecasts work — day 2–3 accuracy remains viable
* Even 52 % directional accuracy beats market over time
* Pipeline is fast — < 60 s per full backtest run on laptop CPU

---

## ⚠️ Limitations

* 🧠 Overfitting to a Narrow Market Period
  Models are trained solely on Jan 2023 – Dec 2024 data — capturing only one regime (post-hike recovery, AI euphoria). This raises overfitting risks if conditions shift.

* ⏳ Forecast Horizon Constraints
  Forecasts degrade after day 3. The longer the horizon, the more uncertainty compounds — especially in volatile or gap-prone stocks.

* 🔁 No Online Adaptation
  Models are fixed during backtest. They don’t retrain or respond to new data — missing regime shifts or news events.

* 📉 No Probabilistic Confidence
  XGBoost outputs point forecasts only. Without prediction intervals, it’s impossible to modulate exposure by uncertainty.

* 🔄 Backtest ≠ Live Trading
  Assumes ideal fills at close price, no slippage or partial executions. Live performance will deviate.

---

## ⚡ Future Roadmap

* Regime‑Aware Retraining (bull vs bear vs chop)
* Sector‑Focused Ensembles
* Bayesian Hyper‑Tuning via Optuna or Ray
* Live Forward Walk via Alpaca
* Streamlit Dashboard for P\&L, explainability (SHAP)

---

## 📊 Quick Start

```bash
# 1. Clone
$ git clone https://github.com/your-handle/stock-xgb-trader.git
$ cd stock-xgb-trader

# 2. Create env
$ conda env create -f environment.yml
$ conda activate xgb-trader

# 3. Run notebooks
$ jupyter lab
```

---

## 🙋‍♂️ Contact & License

Made with ❤️ in Munich — connect on [LinkedIn](https://www.linkedin.com/in/benjamin-greif) or email [b.greif@mailbox.org](mailto:b.greif@mailbox.org).
MIT License. Educational use only — not financial advice.





