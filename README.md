# S&P 500 Intraday Mean Reversion Strategy

This repository contains a quantitative trading algorithm built in Python. It uses an intraday mean-reversion strategy on the S&P 500, specifically tested on 13 years of 1-minute SPY data (over 2 million rows) from 2008 to 2021.

## The Strategy Logic
The algorithm treats the market like a rubber band. When the price moves too fast in one direction and stretches too far from its recent average, it tends to snap back. 

To measure this, the code builds a dynamic "pipe" around the price:
* It calculates a 30-period Exponential Moving Average (EMA) of the Highs and Lows.
* It wraps those averages in 4 levels of Standard Deviation bands.
* When the price hits the extreme outer bands, the algorithm enters a trade assuming the price will revert to the mean.

![Backtest Result](https://github.com/cliteka-cell/Absolute-Range-Moving-Average-Strategy/blob/main/yahoo%20graph%20with%20ranges.png?raw=true)

## Dynamic Volatility Filter
The core edge of this algorithm comes from its context awareness. Market volatility in 2012 was entirely different from the 2020 pandemic. 

Instead of using a fixed point gap, the algorithm uses a **Dynamic Percentage Rule**. It only trades when the distance between the upper and lower bands is greater than **0.1% of the current asset price**. This ensures the algorithm stays quiet during flat, low-volatility markets and actively harvests profits during high-volatility events.

It also includes a strict time filter to prevent trading during historically erratic or low-volume overnight hours.

## 13-Year Backtest Results (2008 - 2021)
The strategy was tested across 13 years, meaning it had to survive the 2008 financial crisis, the quiet bull run of the 2010s, and the 2020 pandemic. 

* **Total Minutes Processed:** 2,070,834
* **Total Trades Taken:** 10,237
* **Win Rate:** 68.0%

### Performance vs. The Market
* **S&P 500 Buy & Hold Return:** 220.77%
* **Algorithm Compounded Return:** 3,561.53%
* **Maximum Drawdown:** -16.55%

While outperforming the market's total return is great, the most important metric here is the Maximum Drawdown. The algorithm generated 15 times the return of the baseline market while only exposing the account to a worst-case 16.55% drop.

## How to Run
1. Clone this repository.
2. Ensure you have `pandas`, `numpy`, and `matplotlib` installed.
3. Download the historical 1-minute SPY data (the 2008-2021 Kaggle dataset was used for this backtest).
4. Run the main Python script to calculate the bands, execute the trading logic, and generate the log-scale equity curve.
