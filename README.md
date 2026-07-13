# Options on a Cointegrated Spread

**Pricing and regime-aware hedging of a pairs trade, expressed with spread options.**

Erdős Institute — Introduction to Quantitative Methods in Finance, Summer 2026
Author: Sara Mezuri

---

## Overview

The classic pairs trade holds a long/short share position in two cointegrated stocks and bets that their spread mean-reverts. This project instead expresses and manages the trade with **European options on the spread**, which raises three quantitative challenges:

1. **No closed form.** A spread option with strike K ≠ 0 has no closed-form price, so pricing is done by Monte Carlo with a **Margrabe (K = 0) control variate** for variance reduction.
2. **Two-dimensional hedging.** Deltas in both legs are estimated by central finite differences on the Monte Carlo price with common random numbers, and hedge performance is compared across rebalancing frequencies.
3. **Regime risk.** Volatilities and correlation are not constant; a 2-state Markov regime-switching model (calm/volatile) quantifies the pricing model risk and tests whether a regime-aware hedger beats a constant-parameter one.

Everything is assembled into an event-driven, out-of-sample historical backtest comparing three implementations of the same Ornstein–Uhlenbeck signal: a classic share pairs trade, and one option trade marked under constant-parameter vs regime-switching pricing.

## Notebooks

The notebooks are designed to run **independently**: notebook 01 prints a frozen parameter block (estimated on training data only, 2021-01 → 2024-07) that is copied into the headers of notebooks 02–05. Nothing is re-estimated out of sample.

| Notebook | Contents |
|---|---|
| [`01_pair_selection`](notebooks/01_pair_selection.ipynb) | Cointegration screening, OU fit to the spread, parameter-stability checks, entry/exit signals |
| [`02_pricing_control_variates`](notebooks/02_pricing_control_variates.ipynb) | Correlated-GBM simulator verified against the Margrabe closed form; K = 0 price as a control variate for the K ≠ 0 spread option |
| [`03_delta_hedging`](notebooks/03_delta_hedging.ipynb) | Two-asset delta hedging of the traded ATM spread option at {none, weekly, daily} rebalancing; finite-difference deltas with common random numbers |
| [`04_regime_switching`](notebooks/04_regime_switching.ipynb) | 2-state Markov regime-switching vol & correlation; pricing model risk and regime-aware vs constant-parameter hedging |
| [`05_backtest`](notebooks/05_backtest.ipynb) | Event-driven out-of-sample backtest: shares vs option (two markings), exit-reason tracking, conclusions |

## Setup

```bash
git clone https://github.com/<your-username>/spread-option-pairs-trading.git
cd spread-option-pairs-trading
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Price data is downloaded at runtime with `yfinance`; no data files are stored in the repo. Note that Yahoo occasionally revises adjusted prices, so numerical results may drift slightly from those shown in the committed notebook outputs.

## Key methods

- Engle–Granger cointegration screening and Ornstein–Uhlenbeck spread modeling
- Monte Carlo pricing under correlated GBM with a Margrabe control variate
- Finite-difference Greeks with common random numbers
- Two-step estimation of a Markov regime-switching volatility/correlation model
- Event-driven backtesting with frozen training parameters (no look-ahead)

## License

MIT — see [LICENSE](LICENSE).
