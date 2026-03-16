# S&P 500 Range Reversion Trading Strategy

This is a Python project I built to test a trading idea I had about the S&P 500. Instead of just writing a script that buys and sells, I used data analysis to find out exactly where the strategy was failing and wrote rules to fix it.

### The Original Idea
I wanted to build a mean reversion algorithm on a 1-minute timeframe. I calculated a moving average of the Highs and Lows over a 30-minute window to create a "pipe" around the price. Then, I added standard deviation lines above and below it. 
* The algorithm scales in (buys) when the price drops below the bottom lines.
* It scales in (shorts) when the price breaks above the top lines.
* It takes profit when the price snaps back to the edge of the pipe.

### The Problem
The first test worked okay, but it took way too many trades. It was losing money when the market was barely moving because the price would just drift without ever snapping back.

### How I Fixed It Using Data
I logged the details of every single trade and analyzed the losers to find the root cause:
1. **Time of Day Analysis:** I grouped the losses by hour. The data showed the algorithm was getting crushed during the middle of the night (like 2:00 AM and 4:00 AM) when trading volume is dead.
2. **Volatility Analysis:** I measured the width of the blue pipe. The data showed that when the pipe was less than 2 points wide, the market was totally flat, causing the code to take fake signals from random 1-tick price movements.

### The Final Results
I added a "Sleep Schedule" to block trades during those quiet hours, and a "Volatility Filter" so it only takes a trade when the pipe is wider than 2.0 points.

By just telling the computer when to stay out of the market, the results improved massively over a 7-day test:
* **Trades Taken:** Dropped from almost 400 down to 274.
* **Win Rate:** Jumped from 64% up to 75.9%.
* **Profit:** Increased to over 8% on the base index price.

### Tools I Used
* Python (Pandas, NumPy)
* Yahoo Finance API for 1-minute market data
* Matplotlib for visualizing the standard deviation bands
