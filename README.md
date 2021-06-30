# Various BTC Algorithmic Trading Simulations

# Overview
Explores a few different trading strategies involving moving averages, Bollinger Bands, percentage gain/loss,and RSI for BTC historical data from 01/01/13 to 03/31/2021.

# Data
Data available at https://www.kaggle.com/mczielinski/bitcoin-historical-data, market price listed in 1 minute increments.

# Display/Implementations
Results of algorithms ran over good representative time periods as well as actual algorithm implementations can be found in TradingAlgo.ipynb.

# Discussion/Results
Description of research/methods-used/results can be found in journal.txt.

# Limitations of Simulations
Some aspects of these algorithms are not entirely realistic/implementable in real life.

Not only does my implementation assume that trades can be perfectly executed every minute at exactly the market price that is listed, but is also assumes that enough liquidity exists such that it can instantly convert millions/billions of USD directly to BTC (if needed), and similarly have enough willing buyers to instantly liquidate this amount of BTC in trade(s) that occur only moments later. 
