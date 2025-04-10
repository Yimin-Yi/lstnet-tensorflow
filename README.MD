# LSTNet TensorFlow

![license](https://img.shields.io/github/license/flaviagiammarino/lstnet-tensorflow)
![languages](https://img.shields.io/github/languages/top/flaviagiammarino/lstnet-tensorflow)

TensorFlow implementation of multivariate time series forecasting model introduced in Lai, G., Chang, W. C., 
Yang, Y., and Liu, H. (2018). Modeling Long- and Short-Term Temporal Patterns with Deep Neural Networks. In *The 41st 
International ACM SIGIR Conference on Research & Development in Information Retrieval (SIGIR '18)*. Association for Computing
Machinery, New York, NY, USA, 95–104. 

## Dependencies
```bash
pandas==1.5.2
numpy==1.23.5
tensorflow==2.11.0
plotly==5.11.0
kaleido==0.2.1
```
## Usage
```python
import numpy as np

from lstnet_tensorflow.model import LSTNet
from lstnet_tensorflow.plots import plot

# Generate some time series
N = 500
t = np.linspace(0, 1, N)
e = np.random.multivariate_normal(mean=np.zeros(3), cov=np.eye(3), size=N)
a = 10 + 10 * t + 10 * np.cos(2 * np.pi * (10 * t - 0.5)) + 1 * e[:, 0]
b = 20 + 20 * t + 20 * np.cos(2 * np.pi * (20 * t - 0.5)) + 2 * e[:, 1]
c = 30 + 30 * t + 30 * np.cos(2 * np.pi * (30 * t - 0.5)) + 3 * e[:, 2]
y = np.hstack([a.reshape(-1, 1), b.reshape(-1, 1), c.reshape(-1, 1)])

# Fit the model
model = LSTNet(
    y=y,
    forecast_period=100,
    lookback_period=200,
    kernel_size=3,
    filters=4,
    gru_units=4,
    skip_gru_units=3,
    skip=50,
    lags=100,
)

model.fit(
    loss='mse',
    learning_rate=0.01,
    batch_size=64,
    epochs=100,
    verbose=1
)

# Generate the forecasts
df = model.forecast(y=y)

# Plot the forecasts
fig = plot(df=df)
fig.write_image('results.png', scale=4, height=900, width=700)
```
![results](example/results.png)
