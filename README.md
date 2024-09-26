# stock-predicition-CNN
## CNN to predict future stock fluctuations via CNN

work in progress

downloading historical 1hr resolution, 5000 time point each per stock, from TradingView
NASDAQ + NYSE tickers, total of ~5500 stocks.

Stocks over a threshold average marketcap have a 90day period candlestick and volume (as marketcap) figures generated, along with the price target for 10days from the end of the 90day period.
Figures are generated at a stride of 5days an are saved only if the price target is represents a 10% flactuation (over and under) that of the price at the end of the 90day period.

Images are fed into a CNN with a 2-channel per image (candlestick+volume) for training.
