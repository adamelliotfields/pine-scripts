# pine-scripts

[Pine](https://www.tradingview.com/pine-script-reference/) is the scripting language for [TradingView](https://www.tradingview.com).

## Scripts

### Average Directional Index

[`adx.pine`](./scripts/adx.pine)

The [Average Directional Index](https://en.wikipedia.org/wiki/Average_directional_movement_index) (ADX) was created by [J. Welles Wilder](https://en.wikipedia.org/wiki/J._Welles_Wilder_Jr.). My implementation uses a histogram instead of plotting separate DI lines.

### Anchored VWAP

[`anchored_vwap.pine`](./scripts/anchored_vwap.pine)

Implementation of [Brian Shannon](https://www.youtube.com/@alphatrends)'s [Anchored VWAP](https://alphatrends.net/anchored-vwap/) indicator. Normally VWAP (volume-weighted average price) is calculated from the start of the bar (i.e., price resets daily). Anchored VWAP allows you to choose the start point, such as the beginning of a trend or a significant event. It results in a line that is more relevant to the current price action.

Most anchored VWAPs on TradingView use a datepicker; this uses `time(confirm=true)` so you only need to click on the chart. You can drag the starting point to a new position to recalculate.

### Dashboard

[`dashboard.pine`](./scripts/dashboard.pine)

Positionable colored table with price change, 50/200 SMA, MACD, RSI, and ATR. Uses timeframe of the chart. Calculated on bar close for performance.

### Fibonacci Retracement

[`fibonacci.pine`](./scripts/fibonacci.pine)

Fork of [LuxAlgo's](https://www.tradingview.com/script/2HmHKuo1-Fibonacci-Toolkit-LuxAlgo).

Fibonacci retracement levels are horizontal lines that indicate where support and resistance are likely to occur. They appear _sometimes_ in nature - like sunflower seed spirals - but it appears to be more of a coincidence than a law of the universe. In financial markets they have become somewhat of a self-fulfilling prophecy, since many traders and algorithms look for them.

The levels are derived from the Golden Ratio, $\phi \approx 1.618$, and its reciprocal, $\frac{1}{\phi} \approx 0.618$:
* `0.618`: The most important level, directly derived from the Golden Ratio. As the sequence grows, dividing any number by the next number approaches `0.618`.
* `0.382`: $1 - \frac{1}{\phi} \approx 0.382$. The compliment of the reciprocal of the Golden Ratio. As the sequence grows, dividing any number by the number two positions higher approaches `0.382`.
* `0.236`: $\frac{2}{\phi} - 1 \approx 0.236$. As the sequence grows, dividing any number by the number three positions higher approaches `0.236`. Considered shallow retracement.
* `0.786`: $\sqrt{\frac{1}{\phi}} \approx 0.786$. The square root of the reciprocal of the Golden Ratio. Considered deep retracement.

The 50% level is not a Fibonacci ratio, but it is a significant psychological level for traders. Also, the 100% level represents a complete retracement to the start of the move, indicating a potential trend reversal.

Fibonacci numbers are also used in [Elliott wave theory](https://en.wikipedia.org/wiki/Elliott_wave_principle).

Like the anchored VWAP above, you click the start and end points on the chart, going from left-to-right during a significant trend.

### Kalman Filter

[`kalman_filter.pine`](./scripts/kalman_line.pine)

Based on [Zeiierman's](https://www.tradingview.com/script/PDi6enZR-Adaptive-Kalman-filter-Trend-Strength-Oscillator-Zeiierman) oscillator.

A [Kalman filter](https://en.wikipedia.org/wiki/Kalman_filter) is a recursive algorithm that estimates the state of a [linear dynamical system](https://en.wikipedia.org/wiki/Linear_dynamical_system) from a series of noisy measurements.

Traditional indicators like moving averages are _non-adaptive_, meaning they don't change their behavior based on new data. A Kalman filter is _adaptive_, meaning it can become more or less sensitive to new data over time.

From a signal processing perspective, a simple moving average is a low-pass filter, meaning it passes low-frequency signals (long-term trends) and attenuates high-frequency signals (short-term fluctuations). It gives equal weight to all data points in its window (e.g., 20 days) and has no "memory" outside of its window. The output depends on $n$ previous inputs, where $n$ is the window size.

This Kalman filter implementation tracks price (position) and momentum (velocity) in a 2D state space (matrix). On each new bar, it predicts where the price _should_ go based on its current trajectory. When its predictions are good, it gives more weight to them, and when they are bad, it gives more weight to the new data. This is what makes it adaptive.

The line can be colored based on normalized velocity.

### Linear Regression Channel

[`linreg_channel.pine`](./scripts/linreg_channel.pine)

Ordinary least squares regression based on the built-in implementation. Use for automatic trendlines.

### Moving Average Cross

[`ma_cross.pine`](./scripts/ma_cross.pine)

Plot two moving averages with crossover/crossunder symbols and alerts.

### Moving Averages

[`ma.pine`](./scripts/ma.pine)

Single script for all moving averages: simple, exponential, relative, Hull, weighted, and volume weighted.

### Nadaraya-Watson Line

[`nw_line.pine`](./scripts/nw_line.pine)

Based on [LuxAlgo's](https://www.tradingview.com/pine/?id=PUB%3B7e0627ee892c4d1ea304928bb62f4c62)'s in non-repainting mode. They use a fixed 500-bar lookback and calculate all the weights on first bar (ahead-of-time). I calculate the weight for each bar (just-in-time), allowing for a variable lookback.

This indicator creates an adaptive price line using the Nadaraya-Watson kernel regression technique.

It is different from a moving average because it is non-linear and adapts to the data. It is different from a Kalman filter because it is non-parametric. It doesn't try to estimate the state of the system and makes no assumptions about the underlying process (price dynamics).

Bandwidth determines how much weight is given to each data point. A larger bandwidth creates a wider curve, giving more weight to distant points. Pick a lookback based on trading timeframe and tune bandwidth based on your data.

[Justin Dehorty](https://www.tradingview.com/u/jdehorty) popularized these techniques and posted a [video](https://www.youtube.com/watch?v=OfW0nwyaEd0) explaining kernel regression. For example, using an exponential kernel would generate a line that approximates an exponential moving average.

### Volume Meter

[`volume_meter.pine`](./scripts/volume_meter.pine)

Fork of [Muqwishi's](https://www.tradingview.com/script/ZMdZlGaJ-Volume-Speed-By-MUQWISHI/). Visualizes the "realtime" buying or selling pressure of a stock or coin by using `request.security_lower_tf()` to fetch the volume from the 1-second chart. Uses `U+2009` (thin space) unicode character for alignment in the settings menu. Also check out their [Tape](https://www.tradingview.com/script/IZnarK90-Time-Sales-Tape-By-MUQWISHI/) script for synthesized time and sales.

## Notes

### Indices

* `PC`: Put/call ratio
* `VIX`: CBOE volatility index
* `DXY`: US dollar index
* `ADD`: NYSE advance-decline line
* `ADDQ`: NASDAQ advance-decline line
* `USINTR`: US interest rate (federal funds market rate)
* `USIRYY`: US inflation rate year-over-year
* `USCCPI`: US core consumer price index year-over-year
* `T10Y2Y`: US 10-year treasury minus 2-year yield spread
* `UNRATE`: US unemployment rate
* `MORTGAGE30US`: US 30-year mortgage rate from Freddie Mac

### Options

The format is: `<symbol><year><month><day><kind><strike>`

For example, the Apple January 3, 2025 $250 call is [AAPL250103C250.0](https://www.tradingview.com/symbols/OPRA-AAPL250103C250.0/).
