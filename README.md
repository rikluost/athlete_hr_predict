# Predicting Cardiac Activity during Variable-Intensity Exercise with Long Short-Term Memory

## Overview

The heart rate in humans intrinsically corresponds to the level of physical exertion, as the heart's responsibility is to supply oxygen to the active muscles; however, a time lag exists between the exertion and the corresponding heart rate (Noakes, T. 2003). This investigation seeks to explore the relationship between physical exertion and the associated delay in heart rate response, with a particular focus on devising a machine learning algorithm capable of predicting the heart rate at a future point in time, say $y$ seconds, based on previous $X$ seconds of activity.

The methodology entails training a machine learning model using data procured from several variable intensity workouts. The model applies the Long Short-Term Memory (LSTM) algorithm (Hochreiter, S. and Schmidhuber J. 1997) for heart rate prediction. The requisite data for this experiment is amassed from a range of sensors incorporated within a contemporary sports watch. These sensors encompass a barometer for altitude measurement, a speed sensor, heart rate monitor, cadence sensor, and GPS.

Recognizing the impending reach of a particular heart rate threshold, for example, in 20 seconds, could enable athletes to decrease their exertion prior to the threshold being crossed, thereby aiding in maintaining a desired heart rate. A pretrained model of this nature could potentially be utilized on various devices like a watch, treadmill, or cycling computer.

## Methodology for Data Collection

Modern sports watches are equipped with a myriad of sensors such as heart rate monitors, cadence trackers, barometers, GPS, and power sensors in the context of cycling. These devices typically record data at one-second intervals and save it to a file. In this instance, the selected input features for training the model comprise a subset of heart rate, cadence, speed, and the grade of the hill.

The dataset is composed of .fit files gathered from approximately 50 runs undertaken in 2021. These runs encompassed diverse environments, including both hilly and flat terrains, and incorporated a variety of exercise intensities, such as extended, slow-paced runs and interval training sessions. The data was collected using the Garmin Fenix 6s (https://buy.garmin.com/en-NZ/NZ/p/641501) in conjunction with the Polar OH1 sensor (https://www.polar.com/en/products/accessories/polar-verity-sense). The fitdecode library (https://github.com/polyvertex/fitdecode) was employed for decoding the sample files.

Portions of the code are inspired by the Keras tutorials found at https://keras.io/examples/timeseries.

The following diagram illustrates the features collected by the Garmin Fenix S6 sports watch during a single run on a hilly terrain. Additionally, an engineered field has been included that depicts the rolling 5-second average altitude difference between the 1-second timestamps, denoted as 'rolling_ave_alt'.

![EDA Graph](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_eda.png)

The units in the measurements here are 
- heart rate, beats per minute
- enhanced_speed, meters per second
- rolling_ave_alt, altitude change metres per second, 5-second moving average
- cadence, steps per minute
- distance, metres
- enhanced_altitude, metres above the sea level

The data was used as collected, without the need for any cleaning transactions, and no missing data was encountered.

## Training, validation, testing datasets

Before proceeding with the training of the machine learning algorithms, a naive model was established as a benchmark. In this simplistic model, it is hypothesized that the heart rate 20 seconds into the future will be identical to the current heart rate. The performance of the machine learning model will be evaluated against this baseline performance.

The fit files were divided into three categories: around 40 files were allocated for training the model, 10 files were set aside for validation purposes, and the remaining 5 files were designated for testing.

## Model

Neural Networks (NNs), a prominent family of machine learning algorithms that can be trained, are currently considered the state-of-the-art, despite their conceptual proposal dating back to the 1940s (McCulloch & Pitts, 1943). The inception of the first neural networks can be traced back to 1965, courtesy of Ivakhnenko & Lapa (1965). Following Seppo Linnainmaa's thesis (1970), Stuart Dreyfus (1973) implemented backpropagation to adjust parameters in response to error gradients. A specific strategy involves employing Long Short-Term Memory (LSTM) (Hochreiter, S. and Schmidhuber J. 1997), a type of recurrent neural network, for predicting future trends based on time-series data.

The selected features were the model were the heartrate, enhanced_speed, rolling_ave_alt and cadence. Simple LSTM model with one hidden 4 neuron LSTM layer was selected. The model architecture is shown below:

- Layer (type),                 Output Shape,              Param #   
- input_2 (InputLayer),         [(None, 60, 4)],           0         
- lstm_1 (LSTM),                (None, 4),                 144       
- dense_1 (Dense),              (None, 1),                 5         

Total params: 149, Trainable params: 149, Non-trainable params: 0

Additionally, exponential learning rate scheduling was used.

The following graph presents the model's training and validation loss during the process of training. It provides an example from the historical training phase where the model was instructed to predict the heart rate 30 seconds into the future based on the sensor readings from the preceding 60 seconds. The graph shows minimal model overfitting, demonstrating that the model has learned effectively without being overly tailored to the training data.

![History](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_his_t30.png)

To circumvent the issue of overfitting, where the model becomes excessively complex and starts to capture noise or random fluctuations in the training data, a technique known as 'early stopping' was employed. This approach ceases the training process before the model begins to overfit, thereby ensuring the model's generalizability to unseen data.

## Model validation

he subsequent graph provides a visual comparison between the observed heart rate and the heart rate predicted by our model. The orange line represents the predicted heart rate for a period 30 seconds into the future. This comparison assists in understanding the performance and accuracy of our model in predicting future heart rates.

![t10](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_ex_t20.png)


### Model validation plots

The model validation plots for 10-second, 20-second, and 40-second future predictions are presented below. The distribution of residuals on the left-hand side plot appears to be roughly Gaussian (Gauss, C. F., 1809) for all prediction time frames, although the distributions widen as the prediction period extends. Minimal skewness is observed, and the mean is centered close to zero. The QQ plots' shape is consistent across all models, indicating a heavy-tailed distribution of residuals, with the scale varying between the models.

The distribution of residuals across the range of predicted values and over the duration of data collection is also illustrated below. It is evident that the residuals are homogeneously distributed across the prediction range and throughout the entire data collection period.

 **10-second prediction from 120-second history**
 
 ![t10v](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t10-120.png)
 
 ![t10r](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_res_t10.png)
 
 **20-second prediction from 120-second history**

 ![t20v](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t20-120.png)
 
 ![t20r](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_res_t20.png)
 
  **40-second prediction from 120-second history**
 
 ![t40v](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_t40-120.png)
 
 ![t40r](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/HR_res_t40.png)

### Results

Initially, the size of the training history was evaluated with durations ranging from 15 seconds to 5 minutes. The Mean Absolute Error (MAE) in the training data demonstrated a reduction up until a 120-second timeframe, after which it began to increase significantly. Therefore, a training history size of 120 seconds was selected for subsequent analysis.

MAE for 20-second prediction, with different training history periods with testing dataset:
- 15-sec training window, trained model 2.45
- 30-sec training window, trained model 2.44
- 60-sec training window, trained model 2.37
- 90-sec training window, trained model 2.27
- 120-sec training window, trained model 2.21
- 150-sec training window, trained model 2.27
- 180-sec training window, trained model 2.28
- 240-sec training window, trained model 2.31
- 300-sec training window, trained model 2.31

Subsequently, both the naive model and the LSTM model were subjected to prediction testing with the test dataset. These tests were aimed at predicting heart rates n-seconds into the future, allowing for a comparative analysis of their performances.

MAE figures for models predicting to future from 5 seconds to one minute. These are all with 120-second training window: 
- 5-sec naive base line 1.19, trained model 0.89
- 10-sec naive base line 2.27, trained model 1.38
- 15-sec naive base line 3.26, trained model 1.81
- 20-sec naive naive base line 4.16, trained model 2.23
- 25-sec naive naive base line 5.00, trained model 2.82
- 30-sec naive base line 5.76, trained model 3.22
- 35-sec naive base line 6.44, trained model 3.73
- 40-sec naive base line 7.06, trained model 4.18
- 50-sec naive base line 8.24, trained model 5.05
- 60-sec naive base line 9.39, trained model 6.06

## Analysis and discussion

The Mean Absolute Error (MAE) represents the absolute difference between the predicted and actual heart rate at a given future time point, n seconds. The predictive model, in this case, exhibits markedly superior performance compared to the naive model, approximately halving the error in the 15 to 25-second range. The model seems to yield the most significant relative performance in the 10 to 35-second prediction range. This interval is where the predicted values offer the most substantial improvement over the naive assumption, highlighting the efficacy of our predictive model.

![t40r](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/SUMMARY.png)

The model was trained using varying lengths of training history to understand the extent to which past activity influences future heart rate. The graph below represents a 20-second future prediction, with a moving history window ranging from 15 seconds to 5 minutes. The most optimal performance was observed with a 120-second history window, indicating that this duration of past activity has the most significant influence on predicting heart rate 20 seconds into the future.

![t40r](https://github.com/rikluost/athlete_hr_predict/blob/master/graphs/SUMMARY_t.png)

## Conclusion

The proposed method appears to be effective, reducing the mean absolute error of the naive assumption (which holds that the heart rate will remain constant over the next 20 seconds) by 50% when a training history of 120 seconds is employed. This study can be expanded further by exploring different deep learning architectures, which could potentially enhance the performance of the simple model currently in use. This preliminary study, however, already demonstrates the potential of machine learning in predicting heart rate during variable intensity exercises.

## References

Hochreiter, S., & Schmidhuber, J. (1997). Long Short-Term Memory. Neural Computation, 9(8), 1735–1780. https://doi.org/10.1162/neco.1997.9.8.1735

Linnainmaa, S. (1970). The representation of the cumulative rounding error of an algorithm as a Taylor expansion of the local rounding errors. University of Helsinki.

McCulloch, W. S., & Pitts, W. (1943). A logical calculus of the ideas immanent in nervous activity. The Bulletin of Mathematical Biophysics, 5(4), 115–133. https://doi.org/10.1007/BF02478259

Noakes, T. (2003). Lore of running (4th ed). Human Kinetics.

Ivakhnenko, A. G., & Lapa, V. G. (1965). Cybernetic Predicting Devices. CCM Information Corporation.

Dreyfus, S. (1973). The computational solution of optimal control problems with time lag. IEEE Transactions on Automatic Control, 18(4), 383–385. https://doi.org/10.1109/TAC.1973.1100330

Gauss, C. F. (1809). Theoria motus corporum coelestium in sectionibus conicis solem ambientum.
