# Predicting heart rate in advance during excercise with LSTM

## Introduction

A working methodology for training a LSTM model for predicting heart rate from multiple sensors data of a sports watch equipped with barometer, heart rate monitor, optionally power, cadence and GPS. The method allows predicting heart rate e.g. 30sec from now based on the past e.g. 60sec of data from various sensors.

Knowing that a certain heart rate limit is going to be met e.g. in 30 seconds time could help athlete reducing the load already before the threshold is met and help maintaining a wanted heart rate. A pretrained model could run e.g. in a watch, treadmil or cycling computer.

Modern sports watches contain many sensors, e.g. heart ratem cadence, barometer, in case of cycling also power sensor, GPS sensors, and the readings are typically saved every once per second in a file. Here, the input features for training the model consist of a subset of heart rate, cadence, speed and the grade of the hill.

The fit data here was collected using Garmin Fenix 6s accompanied with Polar HO2 sensor. The decoding of the example files was done by using https://github.com/polyvertex/fitdecode library.

The code is an initial version, which works, but additional work is required for improved performance and analysis:
- further model architecture exploration
- improve EDA

Parts of the code is based on examples in https://keras.io/examples/timeseries

## EDA

Under works

![EDA Graph](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_eda_t40.png)

## Training, validation, testing datasets

Under works


## Model

Layer (type)                 Output Shape              Param #   
input_2 (InputLayer)         [(None, 60, 4)]           0         
lstm_1 (LSTM)                (None, 4)                 144       
dense_1 (Dense)              (None, 1)                 5         

Total params: 149
Trainable params: 149
Non-trainable params: 0


## Model training

![History](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_eda_t5.png)


## Evaluation

Under works

![t5](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t5.png)

![t10](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t10.png)

![t20](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t20.png)

![t30](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t30.png)

![t40](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t40.png)


## 


