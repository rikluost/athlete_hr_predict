# Training a LSTM model for predicting heart rate in n seconds time

The code attempts to predict the heart rate in n seconds time based on the previous m seconds
- heart rate
- cadence
- speed
- altitude

Knowing that a certain heart rate limit is going to be met could help athlete reducing the load already before the point is met and help maintaining wanted heart rate.

The fit data is collected using Garmin Fenix 6s accompanied with Polar HO2 sensor. The decoding was done earlier with https://github.com/polyvertex/fitdecode library.

The code is an initial version, which works, but requires
- feature engineering for altitude gains/losses
- short EDA
- improved analysis of the results

Some of the code is based on examples in https://keras.io/examples/timeseries

