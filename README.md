
## Prediction of future stock fluctuations with CNN

work in progress

Goal: To construct a classification model to identify stocks' future price range based on technical data (later, fundemental data will be added into the model), using historical 1hr price data to train the CNN.

5000 1hr time points per stock, downloaded from TradingView

NASDAQ + NYSE tickers: Total of ~5500 stocks.

Tech used:

<img alt="Python" src="https://img.shields.io/badge/Python-3776ab?logo=python&logoColor=white&style-flat"><img alt="Pytorch" src="https://img.shields.io/badge/PyTorch-EE4C2C?logo=pytorch&logoColor=white&style-flat">
<img alt="scikit-learn" src="https://img.shields.io/badge/Scikit-f7931e?logo=scikit-learn&logoColor=white&style-flat">
<img alt="NumPy" src="https://img.shields.io/badge/NumPy-013242?logo=numpy&logoColor=white&style-flat">
<img alt="Pandas" src="https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white&style-flat">
<img alt="TradingView" src="https://img.shields.io/badge/TradingView-131622?logo=tradingview&logoColor=white&style-flat">
<img alt="Google Colab" src="https://img.shields.io/badge/GoogleColab-f9ab00?logo=googlecolab&logoColor=white&style-flat">
<img alt="Jupyter" src="https://img.shields.io/badge/Jupyter-f37626?logo=jupyter&logoColor=white&style-flat">


### Data construction:
Stocks over a threshold marketcap have a 12week candlestick plot and volume plot generated, in 1hr resolution.
Plots are generated at a stride of 1week.
#### Price target is the stock price after two weeks from the end of the 12week period image range.

Example of data images:

<img width="810" alt="Screenshot 2024-10-18 at 16 04 25" src="https://github.com/user-attachments/assets/be22dd4b-3221-4fe7-8a61-08c41d0e01a6">

Calssification CNN is trained with a 2-channel image containing the candlestick and volume plots per single timepoint. 
Additional indicators will be introduced into the model in the future via more channels, as well as fundemental data.

Data is divided into 6 datasets, each containing all stocks and all date ranges, but with different offsets of dates.


## Network architecture:

### 1. Parallel Training of Ensemble

An ensemble is generates such that 6 networks are constructed and trained on a different dataset. Each dataset contains every 6th image, with different starting indice 1-6. The test set is identical across training datasets, and the final results is the mean of individual networks' prediction score or a majority vote across the ensemble. The outputs are cross entropy classification with 8 labels.

Each network in the ensemble contains:
  5 convolution layers, and 3 linear layers, with batch normalization and max pooling
  weight decay and dropout rate for regularization 
  weighted loss matrix to penalize extreme prediction errors:
  <img width="511" alt="image" src="https://github.com/user-attachments/assets/976fbbda-a0a2-4cd5-990e-aa6f4a8a21a4">



Target and label distribution, 8 labels. Value in the upper right box indicate the realtive frequency of each label. These values were used to normalize the labels predictions during trianing (classWeights):

![image](https://github.com/user-attachments/assets/0aa95058-0f33-40cb-95e0-192a5a14c0fa)


Example training results for one dataset (out of the 6) for 10 epochs:

![image](https://github.com/user-attachments/assets/0c421378-60d4-4ccf-8f90-2424453d6e3f)


Confusion matrices of each of the training datasets' results. The colorbar in each true-prediction pair is the absolute count of each pair normalized to the total prediction sum of that label (precision if true=prediction)

![image](https://github.com/user-attachments/assets/f02cef81-0d2c-4221-89e0-8958818aa4ea)

Summary confusion matrix where the prediction score of each test instance is averaged across the ensemble. Number indicates absolute counts. Color is the count normalized to total prediction sum of that label:

![image](https://github.com/user-attachments/assets/a2b3674e-a7db-4c2f-9f81-9c9e0cc94896)

Summary confusion matrix where the majority vote of each of the networks in the ensemble is taken for each test instance. Number indicates absolute counts. Color is the count normalized to total prediction sum of that label:

![image](https://github.com/user-attachments/assets/a2b4d64d-da69-4c9b-be7c-caa816532658)

### 2. Sequential Training of Different Datasets on the Same Network

The six dataset are used to train the network, same architecture as before, each for 2 epochs. After going through all of the 6 datasets the process is repeated for a total of 5 times. so there are a total of 30 training epochs in this network, comparable to the 6x10 epochs in the ensemble. The test set is identical across training datasets, and the final results is the mean of individual networks / majority vote.

Confusion matrices of each second epoch (out of 30). The colorbar in each true-prediction pair is the absolute count of each pair normalized to the total prediction sum of that label (precision if true=prediction)

![image](https://github.com/user-attachments/assets/cb9e6623-f41b-4adb-9019-ff9257dbc858)

Summary confusion matrix where the prediction score of each test instance is averaged across the final epoch (out of total 2) of each of the datasets in the last run through the entire data (out of total 5). Number indicates absolute counts. Color is the count normalized to total prediction sum of that label:

![image](https://github.com/user-attachments/assets/a4873db6-4a01-4a84-870d-1b2697120214)

Summary confusion matrix where the majority vote of each test instance is averaged across the final epoch (out of total 2) of each of the datasets in the last run through the entire data (out of total 5). Number indicates absolute counts. Color is the count normalized to total prediction sum of that label:

![image](https://github.com/user-attachments/assets/1a7bdd86-657e-409e-975b-53180ece53e2)
