# SDV time series example 

## Overview

An example use of the **Synthetic Data Vault** (SDV) https://docs.sdv.dev/sdv/ toolkit to generate synthetic 'stock ticker' time series data. The code uses sample stock data (**from Kaggle**) with a 5 minute sample time to train a **Probabilistic AutoRegressive Neural Network** (PAR NN). This model is then used to generate synthetic data that consists of multiple sequences of stock data.  This example is partly based upon the [SDV sequence demo](https://colab.research.google.com/drive/1cT4-jFK2Bxe93QudC_CwHq_yVCcNcxal?usp=sharing) 

The PAR sequencer in SDV needs a multi sequence data set to work upon, which effectively means it needs data that consists of multiple time series samples for multiple stock symbols (where the stock symbol is the sequence)

To generate a multisequence file for training from the Kaggle data set,the sample data is concatenated together and a "Symbol" field created (as the sample data sets have the stock symbol represented in the filename only)   

The sample is presented as a set of numbered **Python notebook files** each of which should be run in sequential order if running for the first time. The files are broken down into each step to allow for effective experimentation with factors such as number & size of training data, number of epochs needed, date and time sequencing factors etc etc. 

**See below for notes on the IDE, links to the training data and runtime python libraries needed etc** 

### Data folder & file structure 
The example is presented as a set of related Python notebooks
-  The sample assumes the following folder structure is present `./KaggleDataSet/5 Min/Stocks/SubSet'  
- The `SubSet` folder should contain one or more of the 5 minute sample files from the [Kaggle Intraday sample data](https://www.kaggle.com/datasets/borismarjanovic/daily-and-intraday-stock-price-data)
- The files in this folder will used to train the models. Copy in files from the kaggle 5 minute folder to provide the notebook with the specific files for training.  
- The files are named `<stocksymbol>.us.txt` and are in CSV format
- The "5 minute" files contain intraday values taken at 5 minute intervals. Note that they do not cover a whole day, and the samples are spread across multiple 60 minute periods 
- The larger the number of files in the folder, the more training data will be used for the model. The number of files has a consequence both for model accuracy and the time to train the model
- As a rough ballpark, 10 files on a non-GPU enabled laptop took ~2 minutes to train, 200 files took ~30 minutes to train  

### Python notebook summary
- `Part1_0_concat_stock_data_samples` This notebook effectively does the pre-processing of the Kaggle data files. It generates a "multisequence" data file by concatenating each of the stock files in the 'SubSet' folder, adding a stock symbol, creating a composite datetime and dropping any un-needed columns. The resulting file is saved as `master_concatenated_stock_input_data.CSV` in the local dir
- `Part1_1_plot real_data` This notebook reads in & plots the sample data sets (one graph per symbol) to provide a visual benchmark upon which to compare the synthetic data sets
- `Part2_train_seq_model_on_stock_data` This notebook reads in the multi sequence training data constructed in Step 1 and constructs the necessary metadata to allow the training model to understad the sequence and the data types etc. Within this notebook are multiple parameters that maybe adjusted including the number of epochs used to train the model. Look at the SDV documentation for more notes on training. This notebook saves the trained model as `IntraDayStockSynthesiser.pkl` 
- `Part3_0_generate_syth_stock_from_model` This notebook loads the trained model from the `pkl` file into an SVD PAR synthesiser. The synthesiser is then used to generate a specified number of sequences (e.g. stock symbol sequences) of a N rows each. These parameters can be changed. The resulting output data frame ofconsisting of synthetic data is then sorted and saved to `synthetic_Intraday_PARstock.CSV`  
- `Part3_1_plot_synth_data` This notebook reads in the synthetic data and then plots it (one graph per symbol) to provide a visual benchmark for comparison with the real world training data

### Limitations
-- Sythetic timeseries limitations: The example code here does *not* correctly sequence the time series data. It is unclear to me if this is due to a failure to configure the PAR sythesizer with correct metadata or a limtation in the approach. To use the synthesised data with a strict timeseries it will be necessary to apply some post processing  
-- Time series plots: The plots that are rendered of the training data and the synthetic data ignore the fact that there time series data is incomplete (e.g. the training data contains  hours worth of data, and the synthetic data doesnt follow a precise time series interval). For this reason they are approximate representations    
 


### Suggested environment 
- VS Code (Opened from command line using "VSCode ." in an Anaconda prompt with the relevant Python virtual environemtn activated)
- Anaconda / Miniconda installed (see Python libraries below) 
- VS Code Jupytper extension 
- VS Code Python extension 


### Python libraries used 
Installed using `Conda install <package name>`
- pandas
- re
- SDV (If using Conda, this may need to be installed using `pip install svd` rather than via `conda / conda install -c conda-forge sdv` due to issues experienced installing from the conda locations)


### Source data
The source data used to train the model comes from the Kaggle Intraday sample data set and uses the 5 minute sample data 
- Kaggle Intraday Stock Data Set: https://www.kaggle.com/datasets/borismarjanovic/daily-and-intraday-stock-price-data