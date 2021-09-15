# Predicting heart rate in advance during excercise with LSTM

A working methodology for training a LSTM model for predicting heart rate from multiple sensors data of a sports watch equipped with barometer, heart rate monitor, optionally power, cadence and GPS. The method allows predicting heart rate e.g. 30sec from now based on the past e.g. 60sec of data from various sensors.

Knowing that a certain heart rate limit is going to be met e.g. in 30 seconds time could help athlete reducing the load already before the threshold is met and help maintaining a wanted heart rate. A pretrained model could run e.g. in a watch, treadmil or cycling computer.

Modern sports watches contain many sensors, e.g. heart ratem cadence, barometer, in case of cycling also power sensor, GPS sensors, and the readings are typically saved every once per second in a file. Here, the input features for training the model consist of a subset of heart rate, cadence, speed and the grade of the hill.

The fit data here was collected using Garmin Fenix 6s accompanied with Polar HO2 sensor. The decoding of the example files was done by using https://github.com/polyvertex/fitdecode library.

The code is an initial version, which works, but additional work is required for improved performance and analysis:
- data celansing
- further model architecture exploration
- short EDA
- model evaluation improvements
- make multi-file validation & test datasets possible - even though training can use multiple files, validation and testing are still restricted to one fit file .

Parts of the code is based on examples in https://keras.io/examples/timeseries
