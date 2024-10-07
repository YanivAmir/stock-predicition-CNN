# stock-predicition-CNN
## CNN to predict future stock fluctuations via CNN

work in progress

The goal is to construct a classification model that will find stocks that are about to rise or fall by more than 10% in a 10 day period.

Using historical 1hr stock data to train a CNN.
5000 1hr time points per stock, downloaded from TradingView
NASDAQ + NYSE tickers: Total of ~5500 stocks.

Stocks over a threshold marketcap have a 90day period candlestick plot and volume plot generated, along with the price target for 10days from the end of the 90day period.
Plots are generated at a stride of 5days.
These parameters will be screened for optimal performance using precision as criteria.

Images are fed into a CNN with a 2-channel per image ( candlestick + volume ) as a single instance.
Ultimately more indicators can be fed into the model in multiple channels per instance.


