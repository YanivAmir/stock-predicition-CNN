
## Prediction of future stock fluctuations with CNN

work in progress

Goal: To construct a classification model to identify stocks' future price range based on technical data (later, fundemental data will be added into the model), using historical 1hr price data to train the CNN.

5000 1hr time points per stock, downloaded from TradingView

NASDAQ + NYSE tickers: Total of ~5500 stocks.


###Data construction:
Stocks over a threshold marketcap have a 12week candlestick plot and volume plot generated, in 1hr resolution.
Plots are generated at a stride of 1week.
#### Price target is the stock price after two weeks from the end of the 12week period image range.

Calssification CNN is trained with a 2-channel image containing the candlestick and volume plots per single timepoint, see example below. 
Additional indicators will be introduced into the model in the future via more channels, as well as fundemental data.

Data is divided into 6 datasets, each containing all stocks and all date ranges, but with different offsets of dates.


#### Network architecture:

An ensemble is generates such that 6 networks are constructed and trained on a different dataset. The test set is identical across trainings, and the final results is the sum of individual networks / majority vote. The networks are with classification outputs (either 6 or 8 labels) and cross entropy loss.

Each network in the ensemble contains:
  4 convolution layers, and 2 linear layers, with batch normalization and max pooling
  wieght decay and drop out rate for regularization 
  optional weighted loss matrix for more extreme prediction errors

Example of data images:

<img width="810" alt="Screenshot 2024-10-18 at 16 04 25" src="https://github.com/user-attachments/assets/be22dd4b-3221-4fe7-8a61-08c41d0e01a6">

Example of target and label distribution:

<img width="587" alt="Screenshot 2024-10-18 at 16 02 55" src="https://github.com/user-attachments/assets/4e825a41-df24-4173-bd8e-e22d8d241595">

Example of training error:

<img width="603" alt="image" src="https://github.com/user-attachments/assets/26a99733-e743-4141-97d7-5558f8453472">

Example of confustion matrix: Values are absolute values in the test set. Colorbar is the result normalized to the sum of the predictiona per-label

<img width="573" alt="Screenshot 2024-10-18 at 16 03 54" src="https://github.com/user-attachments/assets/2776e12a-ab76-4849-b514-5c55f0c872e4">

