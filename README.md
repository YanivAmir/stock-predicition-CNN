# stock-predicition-CNN
##CNN to predict future stock fluctuations via CNN

work in progress

downloading historical 1hr resolution, 5000 time point each per stock, from TradingView
NASDAQ + NYSE tickers, total of ~5500 stock

Stocks over a threshold marketcap have a 90day period candlestick and volume(=marketcap) figures generated and along with the price target for 10days from the end of the 90day period.
figures are generated at a stride of 5days.
figures are saved only if the price target is +- 10% of the price at the end of the 90day period.

images are fed into a CNN with a 2-channel per image (candlestick+volume) for training.
