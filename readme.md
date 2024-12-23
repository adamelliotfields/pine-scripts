# pine-scripts

[Pine](https://www.tradingview.com/pine-script-docs/) is the scripting language for [TradingView](https://www.tradingview.com).

## Scripts

### Anchored VWAP

[`anchored_vwap.pine`](./scripts/anchored_vwap.pine)

Implementation of [Brian Shannon](https://www.youtube.com/@alphatrends)'s [Anchored VWAP](https://alphatrends.net/anchored-vwap/) indicator. Code is based on this [video](https://www.youtube.com/watch?v=nOnZjtR0vvQ). Most anchored VWAPs on TradingView use some type of datepicker; this uses `input.time(confirm = true)` so you only need to click on the chart.

### Dashboard

[`dashboard.pine`](./scripts/dashboard.pine)

Positionable table with formatting for price change, 50/200 SMA, RSI, and ATR. Uses timeframe of the chart.

### Fibonacci Retracement

[`fibonacci.pine`](./scripts/fibonacci.pine)

Fork of [LuxAlgo's](https://www.tradingview.com/script/2HmHKuo1-Fibonacci-Toolkit-LuxAlgo). Like the anchored VWAP above, you click the start and end points on the chart instead of using a datepicker.

### Kalman Filter Line

[`kalman_line.pine`](./scripts/kalman_line.pine)

A [Kalman filter](https://en.wikipedia.org/wiki/Kalman_filter) is a recursive algorithm that estimates the state of a [linear dynamical system](https://en.wikipedia.org/wiki/Linear_dynamical_system) from a series of noisy measurements.

A system is _linear_ if it follows the [superposition principle](https://en.wikipedia.org/wiki/Superposition_principle), which means it can be described by a linear equation. A system is _dynamical_ if it changes over time. Stock markets are _nonlinear_, but the Kalman filter can still be useful for smoothing noisy data.

From a signal processing perspective, a simple moving average is a low-pass filter, meaning it passes low-frequency signals (long-term trends) and attenuates high-frequency signals (short-term fluctuations). It gives equal weight to all data points in its window (e.g., 20 days) and has no "memory" outside of its window. The output depends on $n$ previous inputs, where $n$ is the window size.

Traditional indicators like moving averages are _non-adaptive_, meaning they don't change their behavior based on new data. A Kalman filter is _adaptive_, meaning it can become more or less sensitive to new data over time.

### Linear Regression Channel

[`linreg_channel.pine`](./scripts/linreg_channel.pine)

Ordinary least squares regression based on the built-in implementation. Use for automatic trendlines.

### Moving Averages

[`ma.pine`](./scripts/ma.pine)

Single script for all moving averages: simple, exponential, relative, hull, weighted, and volume weighted.

### Nadaraya-Watson Bands

[`nw_bands.pine`](./scripts/nw_bands.pine)

Fork of [LuxAlgo's](https://www.tradingview.com/script/Iko0E2kL-Nadaraya-Watson-Envelope-LuxAlgo/). This indicator creates adaptive price bands using the Nadaraya-Watson kernel regression technique. At the heart is the Gaussian kernel function:

$$
g(x,\sigma) = \mathrm{e}^{-\frac{x^2}{2\sigma^2}}
$$

Where $x$ is the distance from the current price and $\sigma$ is the bandwidth/smoothing parameter. The kernel function is used to calculate a weighted average of the prices in the window, where recent prices have more weight.

The mean absolute error (MAE) is used to measure how much prices deviate from this average line. The bands are created by adding/subtracting the MAE from the line, multiplied by a user-defined width factor.

The input doesn't have to be the close price; it can be another indicator like VWAP or even the Kalman filter line above. Signals are generated when the price crosses the bands, similar to Bollinger.

[Justin Dehorty](https://www.tradingview.com/u/jdehorty) popularized these techniques and posted a [video](https://www.youtube.com/watch?v=OfW0nwyaEd0) explaining kernel regression.

### Volume Meter

[`volume_meter.pine`](./scripts/volume_meter.pine)

Fork of [Muqwishi's](https://www.tradingview.com/script/ZMdZlGaJ-Volume-Speed-By-MUQWISHI/). Visualizes the "realtime" buying or selling pressure of a stock or coin by using `request.security_lower_tf()` to fetch the volume from the 1-second chart. Uses `U+2009` (thin space) unicode character for alignment in the settings menu. Also check out their [Tape](https://www.tradingview.com/script/IZnarK90-Time-Sales-Tape-By-MUQWISHI/) script for synthesized time and sales.

## Data

Additional data sources to use in analysis.

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
