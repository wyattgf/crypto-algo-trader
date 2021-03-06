## Initial Research

* part 1: https://medium.com/automation-generation/ultimate-list-of-automated-trading-strategies-you-should-know-part-1-c9a333f58930
* part 2: https://medium.com/automation-generation/ultimate-list-of-automated-trading-strategies-you-should-know-part-2-88184b27cd60

* time series momentum/mean reversion: http://docs.lhpedersen.com/TimeSeriesMomentum.pdf, 
* moving average: golden/death cross
* cross sectional momentum/mean reversion
* dollar cost averaging
* market making

* random walk hypothesis: implies that knowledge of previous increase/decrease provides no info on increase/decrease in the future

* relative strength index (RSI): [0,100], ~20 -> buy, ~80 -> sell, measure of how overbought/oversold an asset is, examines magnitudes of recent price changes, to calc: 
	* From one resource: overbought-> above 70, oversold-> below 30, https://www.investopedia.com/terms/r/rsi.asp
	* From another resource: overbought -> 80, oversold -> 20, https://towardsdatascience.com/machine-learning-for-day-trading-27c08274df54
	* Bullish swing rejection/Bottom swing failure: RSI falls into oversold, crosses back above 30%, another dip but does not cross oversold threshold again, RSI breaks into most recent high
	* Bearish swing rejection/Top swing failure: RSI rises into overbought territory, crosses back below 70%, forms another high without crossing into overbought again, RSI breaks into most recent low, less likely to have false alarms than bullish swing rejection
	* false positive: bullish swing -> sharp decline in asset price
	* false negative: bearish swing -> sharp increase in asset price

* moving average convergence divergence (MACD): 
 	* momentum trend-follower
 	* calculate by taking (12 day EMA - 26 day EMA)
 	* signal line is 9 day EMA of MACD
 	* MACD crosses above signal line -> buy, MACD crosses below signal line -> sell
 	* Interpretation of buy signal: the difference between 12 and 26 day moving average is greater than the 9 day moving average -> implies asset price has strong momentum up, thus buying would make sense

* RSI vs MACD: measures price change wrt highs and lows, measures EMA relationships, respectively.  both momentum indicators however

* Bollinger Bands space: 
	* price generally stays between one standard deviation on either side of SMA of price, used to understand whether price is high/low
	* https://www.fidelity.com/learning-center/trading-investing/technical-analysis/technical-indicator-guide/bollinger-bands#:~:text=Bollinger%20Bands%20are%20envelopes%20plotted,moving%20average%20of%20the%20price.&text=Bollinger%20bands%20help%20determine%20whether,conjunction%20with%20a%20moving%20average.
	* price often oscillates between bands, so when price hits bottom band and then crosses above moving average -> top band becomes price target
	* simpler strategy: buy on bottom band, sell on top band
	* also measures overbought and oversold market: if price is much higher than higher band -> asset is overbought -> likely for asset price to fall

* normalization: max/min, values between [0,1]
	v' = (v - min) / (max - min) * (new_max - new_min) + new_min
* standardization: rescale to have SD of 1 and mean of 0
	v'= (v - ??) / ??

## Simulations Over All Data:

0. Simple buy and hold: 1,338,495.6574715262%


Trade on trigger strategy: if starting price is $10 w/ a trigger of 5%, buy on $9.5 and/or sell on $10.5.  Adjust new values accordingly.

1. Trade on trigger, always update trigger threshold values, regardless of success of previous 

* 1%: 2.9491359239092204e+19% 
* 2%: 119,294,528,419.3874%
* 3%: 55,999,943.4149%
* 4%: 22,234,898.4793%
* 5%: 607,859.6312%
* ...percentages started trending down


2. Trade on trigger, only update trigger threshold values when trade is successful

* 1%: 3848.8801%
* 2%: 10904.2983%
* 3%: 847.3916%
* 4%: 366.6489%
* 5%: 410.5072%


Analysis of #1 & #2:

Strategy 1 is safer than strategy 2.  Essentially, if one is willing to hold assets for an indefinite time period, it is impossible to incur loss, as the strategy will only buy an asset when it knows it has previously sold that asset for a higher value (and it is only willing to sell an asset knowing that it previously bought the same asset at a lower value.  This strategy is safe, yet severely underperforms a simple buy-and-hold strategy by many magnitudes of 10.  

Strategy 2 seems questionably trustworthy.  In essence, if BTC followed a steadily declining market price over a long period of time, and threshold values are constantly being updated, it is possibly for an asset to be sold originally at a price much lower than what it originally was purchased for.  

For instance, if an asset was originally purchased for $10 on a 5% trigger, the originally expected value to sell the asset at would be $10.50.  However, if the asset price falls at least ~4.8%, the new upper threshold to sell on, $9.996, is less than the original purchase price of $10.  If executed, this trade would be a loss.  As the asset price continues to fall, this effect would only be accentuated.  

However, as seen by the above results, strategy 2 triumphs over strategy 1.  This can largely be attributed to strategy 1's inclination to get stuck in minimums/maximums as its trading thresholds are not often met.  In strategy 2, by updating the threshold values constantly, this strategy is able to escape these "holes" and make small, but many, gains.  This phenomenon is reflected by the significant increase in gain as the trigger value decreases from 5% to 1%.  A lower trigger allows for more volatility in the strategy, which for BTC, was largely successful in the long run.  

Note: There is an advantage in not always updating threshold values: resistance to severely, long-term dropping asset prices.  If an asset price drops dramatically, and does not recover for quite some time, the volatility of strategy 1 will not triumph, as it is provided no opportunities to recover its losses.  Strategy 2 will resist this as getting "stuck" in a maximum/minimum prevents it from trading below any desired values.
  
3. Simple Moving Average (SMA): buy when asset price crosses above the SMA, sell when the asset price crosses below the SMA


* 50 day: 3181.702408804006%
* 200 day: 7411.21370305519%

Analysis of #3:

The reasoning behind this strategy is simple: if the asset price crosses above the SMA, the SMA can be imagined to be a barrier: a price that the asset price is unlikely to dip below as it trends up.  Therefore, the investor should buy.  Additionally, as the asset price crosses below the SMA, the SMA can act as a barrier that the asset price is unlikely to cross above, in the short term.  Consequently, the investor should sell his current assets, as the price is expected to drop further.  

Poor performance in relation to the simple buy-and-hold strategy.  

Performance in the beginning of time range is likely slightly skewed due to high prevalence of missing data, but effects are most likely diminished as time passes.  The results of this strategy are most likely minimal due to the nature of the strategy itself.  As stated in the name, the calculation was a *simple* moving average, and it is a *simple* strategy.  The SMA merely serves as a *prediction* of the future behavior of the asset price; it is very likely for the exact conditions for trading to be met, yet the asset price then changes counterintuitively to the predicted behavior. 

The performance of the strategy could most likely be improved by utilizing an exponential moving average, that heavily-weighted more recent values in comparison to older ones . 


4. Bollinger Bands: these bands are upper and lower thresholds calculated from some multiple of the standard deviation of the previous X amount of data points off the SMA.  It is typically observed that asset price oscillates between these two bands as time progresses.  

A simple strategy is to sell on the upper band, as the price is likely to bounce back down, and buy on the lower band, as the price is likely to bounce back up.

* Over all time for selling on high band, buying on low band:
	* 10-day: -4.2568898672914885
	* 14-day: 187.99999787751258
	* 20-day: -34.206997286910465
	* 50-day: 251.84728636489785

* Over randomly-chosen 2 month period:
	* Strategy 1: Sell on high band, buy on low band, 14 MA: 13.640246957069415%
	* Strategy 2: Sell on BB value >= 1, buy on BB value <= 0: -46.387803151112585%

After experimenting over many days, it seems that the curves of total portfolio value of strategy 1 and strategy 2 generally follow the same patterns.  However, typically, the curve of strategy 1 is usually higher in magnitude than the curve of strategy 2.  Though, at times, the ending portfolio worth of strategy 2 would creep up above the ending portfolio worth of strategy 1. 


5. Relative Strength Index: buy when asset price <= 30% and sell when asset price >= 70%

Portfolio Gain: 809,261,883,688,650.2% #This is so very wrong

Analysis of #5 + GENERAL LIMITATIONS of Real-Life for all Algorithms:

Some aspects of this algorithm, similar to some aspects of the other previously implemented algorithms, are not entirely realistic/actually-implementable in real life.  Essentially, the results of this algorithm can be explained by the sheer amount of trades performed across a long period of time where the price of BTC is rapidly increasing.  Not only does it assume that trades can be perfectly executed every minute at exactly the market price that is listed, but it also assumes that enough liquidity exists such that it can instantly convert millions/billions of USD directly to BTC, and similarly have enough willing buyers to instantly liquidate this amount of BTC in a trade that occurs only a few moments later. 
 
## Improvements:

To improve the above algorithm, as well as the others, I would add a threshold value of USD/BTC-equivalent that, once passed, would apply some sort of attenuation constant to the trading functions that does not allow for a portfolio's entire volume of BTC to be liquidated or for its entire volume of USD to be converted to BTC.  Additionally, the performance could be made more realistic by adding a variable slippage percentage (quasi-randomly determined within a certain range of market price) that would account for variable price differences between orders and actual executions.  

## Running Journal:

1. Try buying when price is above moving average and selling when price is below moving average
2. BTC price is available at increments of 1 min, can toggle "time_delta" in code to make 50 "day" moving average, for instance, to correspond to 50 minutes, 50 hours, 50 days, etc...
3. Add parameters to allow for different proportions of cash willing to spend and/or coin willing to sell
4. Played around adjusting time_delta and rank of moving average (rank would be the "50" in "50 day moving avg")
5. day that mostly saw increase in btc price: rank 50, units of hours, did really well, ~90% increase
6. day that saw bimodal increases in btc price: same parameters as above did good, ~24% increase; rank 200 units of hours, ~57% increase; rank 50 units of days saw similar increase, ~57% increase
7. day that saw initial decrease in btc price: model saw poor performance, rank 50 units hours, -23%; rank 200 units of hours, -6%
8. day that saw overall decrease: model saw neutral performance, rank 50 unit hours, 2.4%; rank 200 unit hours, 14.69%
9. ON POOR PERFORMANCE DAYS: distance between btc price curve and overall wealth curve are well matched is small in beginning but gap increases as day goes on, however, shape of curve is generally matched  
10. OBSERVATION: days that do well, very minimal distance between two curves, this distance can be attempted to be minimized in a cost function for an ML model!  

Sometimes it's funny because I'll scroll up out of the output box because I'm nervous about there results.

Might've just also stumbled upon something pretty crazy.

Ideas to explore:

1. Sell when above threshold, buy when below threshold, always update values regardless of buy/sell ability
2. Sell when above threshold, buy when below threshold, only update values when buy/sell possible
3. Buy when above threshold, sell when below threshold, always update values regardless of buy/sell possibility, this was found on accident
4. Buy when above threshold, sell when below threshold, only update values when buy/sell possible, this definitely won't work


1. Over all data: 607859.6312%, Over individual random days: didn't always do great, sometimes negative returns but small in magnitude
2. Over all data: 873.5607%, Over individual random days: performance felt pretty variable
3. Over all data: ~120%, Over individual random days: variable return but pretty much always positive when using trad_prop = 1 
4. Over all data: -5.0915%, 


Gain for individual days would vary on trade_proportion values.
 
Thoughts on above: Currently I update threshold values to percentages off the current price, should I instead try out updating the values to percentages based off of the previous threshold price?

 




 
