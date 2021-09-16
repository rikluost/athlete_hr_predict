# Predicting heart rate in advance during exercise  with LSTM

## Introduction

This study explains and discusses a working methodology for training an LSTM model (Hochreiter, S. and Schmidhuber J.  1997) for predicting heart rate from multiple sensors data of a sports watch equipped with a barometer, speed information, heart rate monitor, optionally power, cadence and GPS. The method allows predicting heart rate, e.g. 30-seconds to the future, based on the past, e.g. 60sec of data from various sensors.

Knowing that a specific heart rate limit will be met, e.g. in 30 seconds, could help athletes reduce the load before the threshold is met and help maintain a wanted heart rate. A pre-trained model could run, e.g. on a watch, treadmill or cycling computer.

Modern sports watches contain many sensors, e.g. heart rate, cadence, barometer, GPS, and in the case of cycling, a power sensor. The readings are typically saved every once per second in a file. Here, the input features for training the model consist of a subset of heart rate, cadence, speed, and the hill's grade.

The fit data was collected using Garmin Fenix 6s (https://buy.garmin.com/en-NZ/NZ/p/641501) accompanied by a Polar OH1 sensor (https://www.polar.com/en/products/accessories/polar-verity-sense). The decoding of the example files was done by using the fitdecode (https://github.com/polyvertex/fitdecode) library.

Parts of the code is based on Keras tutorials in https://keras.io/examples/timeseries

## EDA

The data consists of fit files collected on approximately 50 runs during 2021. The runs were made in various environments, i.e. hilly and flat. Also, various efforts, e.g. long and slow runs and interval training, are included.

Below graph shows the features as collected by the Garmin Fenix S6 sports watch during a single run on hills, but with an additional engineered field of the rolling 5 seconds average altitude difference between the 1 second timestamps 'rolling_ave_alt'.

![EDA Graph](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_eda_t40.png)

The units in the measurements here are 
- heart rate, beats per minute
- enhanced_speed, meters per second
- rolling_ave_alt, altitude change metres per second, 5-second moving average
- cadence, steps per minute
- distance, metres
- enhanced_altitude, metres above the sea level

## Training, validation, testing datasets

The fit files were split into 10 validation files, approximately 40 model training files, and 5 for testing purposes.

## Model

Neural networks (NN) are the current state of the art family of trainable machine learning algorithms, even though they were proposed as early as the 1940s (McCulloch & Pitts, 1943). First neural networks were proposed in 1965 by Ivakhnenko & Lapa (1965). Following Seppo Linnainmaas thesis (1970), Stuart Dreyfus (1973) used backpropagation to adapt parameters to error gradients. A specific approach is to use Long Short Term Memory (LSTM) (Hochreiter, S. and Schmidhuber J. 1997) for predicting the future from time series of data.

The selected features were the model were the heartrate, enhanced_speed, rolling_ave_alt and cadence. Simple LSTM model with one hidden 4 neuron LSTM layer was selected with a drop-out layer for model regularisation. The model architecture is shown below:

- Layer (type),                 Output Shape,              Param #   
- input_2 (InputLayer),         [(None, 60, 4)],           0         
- lstm_1 (LSTM),                (None, 4),                 144       
- dense_1 (Dense),              (None, 1),                 5         

Total params: 149, Trainable params: 149, Non-trainable params: 0



## Model training

The model training and validation loss graph below indicates no or very small model overfitting. The graph is an example from the history of training the model to predict heart rate in 30 seconds.

![History](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_his_t30.png)

Early stopping was used to prevent the overfitting of the model.

## Model validation

An example graph below shows the observed heart rate and the predicted heart rate. Prediction in orange here is one for 30 seconds to the future.

![t10](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_ex_t20.png)


### Model validation plots

 The model validation plots are shown for 10-second, 20-second and 40-second predictions below. The distribution of residuals on the left-hand side plot seems to follow near-Gaussian (Gauss, C. F., 1809) distribution for all prediction times; however the distributions are getting wider the further we are trying to predict. There is very little notable skewness, and the mean is near to the centre. The shape in the QQ plot is similar between the models to all previous models indicating that the distribution of the residuals is tail heavy at different scales between the models. 
 
 The residuals' distribution over the range of the predicted values and the data collection duration are shown below. The residuals seem homogenous across the range of predictions and over the whole data collection period.

 **10-second prediction**
 
 ![t10v](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t10.png)
 
 ![t10r](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_res_t10.png)
 
 **20-second prediction**

 ![t20v](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t20.png)
 
 ![t20r](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_res_t20.png)
 
  **40-second prediction**
 
 ![t40v](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t40.png)
 
 ![t40r](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_res_t40.png)

## Discussion and analysis

Under works

## References

Hochreiter, S., & Schmidhuber, J. (1997). Long Short-Term Memory. Neural Computation, 9(8), 1735–1780. https://doi.org/10.1162/neco.1997.9.8.1735

Linnainmaa, S. (1970). The representation of the cumulative rounding error of an algorithm as a Taylor expansion of the local rounding errors. University of Helsinki.

McCulloch, W. S., & Pitts, W. (1943). A logical calculus of the ideas immanent in nervous activity. The Bulletin of Mathematical Biophysics, 5(4), 115–133. https://doi.org/10.1007/BF02478259

Ivakhnenko, A. G., & Lapa, V. G. (1965). Cybernetic Predicting Devices. CCM Information Corporation.

Dreyfus, S. (1973). The computational solution of optimal control problems with time lag. IEEE Transactions on Automatic Control, 18(4), 383–385. https://doi.org/10.1109/TAC.1973.1100330

Gauss, C. F. (1809). Theoria motus corporum coelestium in sectionibus conicis solem ambientum.