# Training a LSTM model for predicting heart rate in n seconds time

The code attempts to predict a runners heart rate in n seconds time based on the previous m seconds

Knowing that a certain heart rate limit is going to be met e.g. in 20 seconds time could help athlete reducing the load already before the threshold is met and help maintaining a wanted heart rate. A pretrained model could run e.g. in a watch, treadmil or cycling computer.

Inputs for the prediction are the previous, e.g. 60 seconds of the below predictors:
- heart rate
- cadence
- speed
- altitude

The fit data here is collected using Garmin Fenix 6s accompanied with Polar HO2 sensor. The decoding of the example files was done earlier with https://github.com/polyvertex/fitdecode library.

The code is an initial version, which works, but requires additional work on the following for improved performance and analysis:
- data celansing
- feature engineering for altitude gains/losses
- short EDA
- improved analysis of the results
- make multi-file validation datasets possible - currently works only with on fit file.

Some of the code is based on examples in https://keras.io/examples/timeseries

Similarly, the approach could be used for cycling with a bike equipped with power sensor.
