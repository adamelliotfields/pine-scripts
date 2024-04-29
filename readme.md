# pine-scripts

A handful of scripts I forked to learn [Pine](https://www.tradingview.com/pine-script-docs/en/v5/Introduction.html), the scripting language for [TradingView](https://www.tradingview.com). If you're unfamiliar, it's a domain-specific language purpose-built for time series.

## Scripts

### Anchored VWAP

[`anchored_vwap.pine`](./scripts/anchored_vwap.pine)

Implementation of [Brian Shannon](https://www.youtube.com/@alphatrends)'s [Anchored VWAP](https://alphatrends.net/anchored-vwap/) indicator. Code is based on this [video](https://www.youtube.com/watch?v=nOnZjtR0vvQ). Most anchored VWAPs on TradingView use some type of datepicker; this uses `input.time(confirm = true)` so you only need to click on the chart.

To test for accuracy, use the built-in auto-anchored VWAP and anchor to the same point. Note that the built-in has standard deviation bands, while mine does not.

### SMA and EMA

[`sma.pine`](./scripts/sma.pine) | [`ema.pine`](./scripts/ema.pine)

Like the built-ins, except the smoothing line is not calculated unless it is explicitly checked (the built-in just hides it).

In older versions of Pine, you used the `iff()` function, which executed both branches of the condition; in v5 you can just use a ternary which only evaluates one.

**TODO:** Merge into a single `ma.pine` script that allows you to select the type of moving average.

### Fibonacci Retracement

[`fibonacci.pine`](./scripts/fibonacci.pine)

Fork of [LuxAlgo's](https://www.tradingview.com/script/2HmHKuo1-Fibonacci-Toolkit-LuxAlgo). Like the anchored VWAP above, you simply click the start and end points on the chart.

### Nadaraya-Watson Bands

[`nadaraya-watson_bands.pine`](./scripts/nadaraya-watson_bands.pine)

Fork of [LuxAlgo's](https://www.tradingview.com/script/Iko0E2kL-Nadaraya-Watson-Envelope-LuxAlgo/). Also check out their [smoothing line](https://www.tradingview.com/script/amRCTgFw-Nadaraya-Watson-Smoothers-LuxAlgo/).

[Justin Dehorty](https://www.tradingview.com/u/jdehorty) popularized these techniques and posted a [video](https://www.youtube.com/watch?v=OfW0nwyaEd0) explaining kernel regression. Check out Justin's [scripts](https://www.tradingview.com/u/jdehorty/#published-scripts) for more ideas.

### Volume Meter

[`volume_meter.pine`](./scripts/volume_meter.pine)

Fork of [Muqwishi's](https://www.tradingview.com/script/ZMdZlGaJ-Volume-Speed-By-MUQWISHI/). Visualizes the "realtime" volume flow of a stock or coin by using `request.security_lower_tf()` to fetch the volume from the 1-second chart. Uses `U+2009` (thin space) unicode character for alignment in the settings menu.

Also check out their [Tape](https://www.tradingview.com/script/IZnarK90-Time-Sales-Tape-By-MUQWISHI/) script for synthesized time and sales. Note that for real data like this in TradingView, you need to connect to [Interactive Brokers](https://www.interactivebrokers.com) or [TradeStation](https://www.tradestation.com).
