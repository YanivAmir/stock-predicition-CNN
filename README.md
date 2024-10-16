
## Prediction of future stock fluctuations with CNN

work in progress

Goal: To construct a classification model to identify stocks' future price range based on technical data (later, fundemental data will be added into the model), using historical 1hr price data to train the CNN.

5000 1hr time points per stock, downloaded from TradingView

NASDAQ + NYSE tickers: Total of ~5500 stocks.

Stocks over a threshold marketcap have a 12week candlestick plot and volume plot generated, in 1hr resolution ,along with the price target for 2weeks from the end of the 12week period.
Plots are generated at a stride of 1week.
These parameters will be screened for optimal performance.

Images are fed into a CNN with a 2-channel per timepoint ( candlestick + volume ).
Additional indicators can be fed into the model in multiple channels per timepoint, as well as fundemental data.


