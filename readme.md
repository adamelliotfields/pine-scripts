# pine-scripts

[Pine](https://www.tradingview.com/pine-script-reference/) is the scripting language for [TradingView](https://www.tradingview.com).

## Scripts

### Anchored Fibonacci

[`anchored_fibonacci.pine`](./scripts/anchored_fibonacci.pine)

Fork of [LuxAlgo's](https://www.tradingview.com/script/2HmHKuo1-Fibonacci-Toolkit-LuxAlgo).

Fibonacci retracement levels are horizontal lines that indicate where support and resistance are likely to occur. They appear _sometimes_ in nature - like sunflower seed spirals - but it appears to be more of a coincidence than a law of the universe. In financial markets they have become somewhat of a self-fulfilling prophecy, since many traders and algorithms look for them.

The levels are derived from the Golden Ratio, $\phi \approx 1.618$, and its reciprocal, $\frac{1}{\phi} \approx 0.618$:
* `0.618`: The most important level, directly derived from the Golden Ratio. As the sequence grows, dividing any number by the next number approaches `0.618`.
* `0.382`: $1 - \frac{1}{\phi} \approx 0.382$. The compliment of the reciprocal of the Golden Ratio. As the sequence grows, dividing any number by the number two positions higher approaches `0.382`.
* `0.236`: $\frac{2}{\phi} - 1 \approx 0.236$. As the sequence grows, dividing any number by the number three positions higher approaches `0.236`. Considered shallow retracement.
* `0.786`: $\sqrt{\frac{1}{\phi}} \approx 0.786$. The square root of the reciprocal of the Golden Ratio. Considered deep retracement.

The 50% level is not a Fibonacci ratio, but it is a significant psychological level for traders. Also, the 100% level represents a complete retracement to the start of the move, indicating a potential trend reversal.

Fibonacci numbers are also used in [Elliott wave theory](https://en.wikipedia.org/wiki/Elliott_wave_principle). The default Awesome Oscillator parameters are 5 and 34, which are Fibonacci numbers.

### Anchored Linear Regression

[`anchored_linear_regression.pine`](./scripts/anchored_linear_regression.pine)

Ordinary least squares regression based on the built-in implementation. Use for automatic trendlines. Anchored to a start date on the chart.

### Anchored VWAP

[`anchored_vwap.pine`](./scripts/anchored_vwap.pine)

Implementation of [Brian Shannon](https://www.youtube.com/@alphatrends)'s [Anchored VWAP](https://alphatrends.net/anchored-vwap/) indicator. Normally VWAP (volume-weighted average price) is calculated from the start of the bar (i.e., price resets daily). Anchored VWAP allows you to choose the start point, such as the beginning of a trend or a significant event. It results in a line that is more relevant to the current price action.

Most anchored VWAPs on TradingView use a datepicker; I use `time(confirm=true)` so you only need to click on the chart. You can drag the starting point to a new position to recalculate.

### Average Directional Index

[`adx.pine`](./scripts/adx.pine)

The [Average Directional Index](https://en.wikipedia.org/wiki/Average_directional_movement_index) (ADX) was created by [J. Welles Wilder](https://en.wikipedia.org/wiki/J._Welles_Wilder_Jr.).

My implementation uses a histogram instead of plotting separate DI lines.

### Awesome Oscillator

[`awesome.pine`](./scripts/awesome.pine)

Created by [Bill Williams](https://en.wikipedia.org/wiki/Bill_Williams_(trader)), the Awesome Oscillator is a momentum indicator that shows the difference between a 34-period and 5-period simple moving average, using `hl2` as the source. It is designed to detect market momentum and possible reversals. The histogram is colored based on the direction of the oscillator.

My implementation allows you to change the source, moving average kind, and the periods.

### Dashboard

[`dashboard.pine`](./scripts/dashboard.pine)

Positionable colored table with price change, 20/50/200 SMA, MACD, RSI, ATR, analyst price target, and upcoming earnings date. Uses timeframe of the chart. Calculated on bar close for performance.

### Kalman Filter

[`kalman_filter.pine`](./scripts/kalman_line.pine)

Based on [Zeiierman's](https://www.tradingview.com/script/PDi6enZR-Adaptive-Kalman-filter-Trend-Strength-Oscillator-Zeiierman) oscillator.

A [Kalman filter](https://en.wikipedia.org/wiki/Kalman_filter) is a recursive algorithm that estimates the state of a [linear dynamical system](https://en.wikipedia.org/wiki/Linear_dynamical_system) from a series of noisy measurements.

Traditional indicators like moving averages are _non-adaptive_, meaning they don't change their behavior based on new data. A Kalman filter is _adaptive_, meaning it can become more or less sensitive to new data over time.

From a signal processing perspective, a simple moving average is a low-pass filter, meaning it passes low-frequency signals (long-term trends) and attenuates high-frequency signals (short-term fluctuations). It gives equal weight to all data points in its window (e.g., 20 days) and has no "memory" outside of its window. The output depends on $n$ previous inputs, where $n$ is the window size.

This Kalman filter implementation tracks price (position) and momentum (velocity) in a 2D state space (matrix). On each new bar, it predicts where the price _should_ go based on its current trajectory. When its predictions are good, it gives more weight to them, and when they are bad, it gives more weight to the new data. This is what makes it adaptive.

The line can be colored based on normalized velocity.

### Moving Average

[`ma.pine`](./scripts/ma.pine)

Single script for all moving averages: simple, exponential, relative, Hull, weighted, and volume weighted.

### Moving Average Cross

[`ma_cross.pine`](./scripts/ma_cross.pine)

Plot two moving averages with crossover/crossunder symbols and alerts.

### Nadaraya-Watson Line

[`nw_line.pine`](./scripts/nw_line.pine)

Based on [LuxAlgo's](https://www.tradingview.com/pine/?id=PUB%3B7e0627ee892c4d1ea304928bb62f4c62)'s in non-repainting mode. They use a fixed 500-bar lookback and calculate all the weights on first bar (ahead-of-time). I calculate the weight for each bar (just-in-time), allowing for a variable lookback.

This indicator creates an adaptive price line using the Nadaraya-Watson kernel regression technique.

It is different from a moving average because it is non-linear and adapts to the data. It is different from a Kalman filter because it is non-parametric. It doesn't try to estimate the state of the system and makes no assumptions about the underlying process (price dynamics).

Bandwidth determines how much weight is given to each data point. A larger bandwidth creates a wider curve, giving more weight to distant points. Pick a lookback based on trading timeframe and tune bandwidth based on your data.

[Justin Dehorty](https://www.tradingview.com/u/jdehorty) popularized these techniques and posted a [video](https://www.youtube.com/watch?v=OfW0nwyaEd0) explaining kernel regression. For example, using an exponential kernel would generate a line that approximates an exponential moving average.

### Parabolic SAR

[`parabolic_sar.pine`](./scripts/parabolic_sar.pine)

The Parabolic SAR (Stop and Reverse) indicator is a trend-following indicator that provides entry and exit points. It was developed by J. Welles Wilder. The further the SAR value is from the price, the stronger the trend. The dots appear below the price during uptrends, and above the price during downtrends. As the trend weakens, the dots get closer to the price, eventually crossing it and signaling a potential reversal. This differs from the built-in indicator by using gradient coloring based on distance from price.

### Supertrend

[`supertrend.pine`](./scripts/supertrend.pine)

The Supertrend indicator was developed by Olivier Seban in 2009. It is an ATR-based trend-following indicator.

My implementation uses the built-in with some personal visual improvements.

### Symbol

[`symbol.pine`](./scripts/symbol.pine)

Displays a candlestick chart with optional moving average for the given symbol in the oscillators pane.

### Volume Meter

[`volume_meter.pine`](./scripts/volume_meter.pine)

Fork of [Muqwishi's](https://www.tradingview.com/script/ZMdZlGaJ-Volume-Speed-By-MUQWISHI/). Visualizes the "realtime" buying or selling pressure of a stock or coin by using `request.security_lower_tf()` to fetch the volume from the 1-second chart. Uses `U+2009` (thin space) unicode character for alignment in the settings menu. Also check out their [Tape](https://www.tradingview.com/script/IZnarK90-Time-Sales-Tape-By-MUQWISHI/) script for synthesized time and sales.

### Zig Zag

[`zig_zag.pine`](./scripts/zig_zag.pine)

The Zig Zag indicator finds pivot points, which are local highs and lows that exceed a minimum deviation threshold. The difference between each pivot can be displayed on the chart as a percentage or absolute value. The depth parameter determines how big the sliding window is for finding pivots. For example, the default of `20` means 10 bars before and 10 bars after the current bar. It's more of a visual indicator than a trading signal, although the pivot points can be also be used as start points for the anchored indicators.

My implementation uses TradingView's [Zig Zag](https://www.tradingview.com/script/bzIRuGXC-ZigZag/) library with both percent and ATR-based deviation.

## Notes

### Candles

#### Hollow

Hollow candles are green if the close is higher than the previous close. They are filled if they close lower than they open. In a strong uptrend you'll see mostly hollow green candles, and in a strong downtrend you'll see mostly filled red candles.

### Indices

* `CBOE:VIX`: The CBOE volatility index measures the market's expectation of future (30-day) volatility. It is calculated from the prices of S&P 500 index (`SPX`) options. A high VIX indicates uncertainty, and it is often called the "fear gauge." There are other volatility indices like `VXN` for Nasdaq or `OVX` for oil. There's also the `VVIX`, which measures the volatility of the VIX itself.

* `CBOE:SKEW`: The CBOE SKEW index measures tail or downside risk. As traders become more worried about a market collapse, they buy more out-of-the-money puts for protection. As demand for these contracts increases, they become more expensive relative to at-the-money options. This creates a skewness in the options pricing curve. This differs from the VIX, which measures volatility in both directions.

* `USI:ADD`: The NYSE advance-decline line shows the cumulative difference between rising and falling stocks. It shows how many stocks are trading up compared to the previous day's close. It is a measure of market breadth and helps to understand broader market sentiment. Note that the `ADD`, `TRIN`, `TICK`, and `VOLD` are all meant for intraday analysis, as they reset each session.

* `USI:TRIN.NY`: The `TRIN` (Arms index) is essentially a volume-weighted version of the advance-decline line.

* `USI:VOLD`: The NYSE VOLD index shows the difference between up-volume and down-volume. On days where the `VOLD` opens higher and continues to rise, it usually means the price will close higher (and vice-versa). Divergence between price and `VOLD` can signal a reversal.

* `USI:TICK`: The NYSE TICK index shows the number of stocks trading on an uptick minus the number of stocks trading on a downtick. Extreme readings are +/-1000. The normal range is +/-400. When you see large ticks, it means there was concentrated buying or selling by institutions.

* `INDEX:S5TW`: S&P 500 stocks above 20-day moving average. Updated end-of-day. See `S5FI` for 50-day and `S5TH` for 200-day.

Other interesting indices:

* `USINTR`: US interest rate (federal funds market rate)
* `USIRYY`: US inflation rate year-over-year
* `USCCPI`: US core consumer price index year-over-year
* `T10Y2Y`: US 10-year treasury minus 2-year yield spread
* `UNRATE`: US unemployment rate
* `MORTGAGE30US`: US 30-year mortgage rate from Freddie Mac

### Options

The format is: `<symbol><year><month><day><kind><strike>`

For example, the Apple January 3, 2025 $250 call is [AAPL250103C250.0](https://www.tradingview.com/symbols/OPRA-AAPL250103C250.0/).

### Hotkeys

Start typing a number (or <kbd>,</kbd>) to change the interval or letters to change the symbol. Press <kbd>/</kbd> to open the indicator list.

* <kbd>Ctrl</kbd>+<kbd>/</kbd>: Show all hotkeys.
* <kbd>Ctrl</kbd>+<kbd>K</kbd>: Quick search (similar to command palette).
* <kbd>Ctrl</kbd>+<kbd>Alt</kbd>: Show a button for adding an alert, placing an order, or drawing a line.
* <kbd>Alt</kbd>+<kbd>A</kbd>: Add an alert at the price under the cursor.
* <kbd>Alt</kbd>+<kbd>H</kbd>: Draw a horizontal line at the price under the cursor.
* <kbd>Alt</kbd>+<kbd>L</kbd>: Toggle log scale.
* <kbd>Alt</kbd>+<kbd>P</kbd>: Toggle percent scale.
* <kbd>Shift</kbd>+<kbd>F</kbd>: Toggle full screen.
* <kbd>Shift</kbd>+<kbd>T</kbd>: Toggle the trade panel. Also <kbd>Shift</kbd>+<kbd>B</kbd> to buy and <kbd>Shift</kbd>+<kbd>S</kbd> to sell.
