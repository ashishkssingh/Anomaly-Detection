# Implementing Anomaly Detection:
In this notebook, I'll try and implement Anomaly Detection using multiple different algorithms learned during my Post-Graduate courses.

### Algorithms Implemented:
1. Standard Deviation Method
2. Interquartile Range Method
3. Gaussian Distribution
4. SH-ESD (Seasonal Hybrid Extreme Studentized Deviate) 

## Anomaly-Detection Using Standard Deviation Method:

If we know that the distribution of values in the sample is **Gaussian or Gaussian-like**, we can use the standard deviation of the sample as a cut-off for identifying outliers.

The Gaussian distribution has the property that the standard deviation from the mean can be used to reliably summarize the percentage of values in the sample.

For example, within one **standard deviation of the mean will cover 68% of the data**.

So, if the mean is 50 and the standard deviation is 5, then all data in the sample between 45 and 55 will account for about 68% of the data sample. We can cover more of the data sample if we expand the range as follows:

- Standard Deviation from the Mean: 68%
- Standard Deviations from the Mean: 95%
- Standard Deviations from the Mean: 99.7%

A value that falls outside of 3 standard deviations is part of the distribution, but it is an unlikely or rare event at approximately 1 in 370 samples.

Three standard deviations from the mean is a common cut-off in practice for identifying outliers in a Gaussian or Gaussian-like distribution. For smaller samples of data, perhaps a value of 2 standard deviations (95%) can be used, and for larger samples, perhaps a value of 4 standard deviations (99.9%) can be used.

Let’s make this concrete with a worked example.

Sometimes, the data is standardized first (e.g. to a Z-score with zero mean and unit variance) so that the outlier detection can be performed using standard Z-score cut-off values. This is a convenience and is not required in general, and we will perform the calculations in the original scale of the data here to make things clear.

We can calculate the mean and standard deviation of a given sample, then calculate the cut-off for identifying outliers as more than 3 standard deviations from the mean.

![anomalies](https://user-images.githubusercontent.com/15687230/51702598-3728c700-1fe2-11e9-93c5-89c532391725.png)

So far we have only talked about univariate data with a Gaussian distribution, e.g. a single variable. You can use the same approach if you have multivariate data, e.g. data with multiple variables, each with a different Gaussian distribution.

You can imagine bounds in two dimensions that would define an ellipse if you have two variables. Observations that fall outside of the ellipse would be considered outliers. In three dimensions, this would be an ellipsoid, and so on into higher dimensions.

Alternately, if you knew more about the domain, perhaps an outlier may be identified by exceeding the limits on one or a subset of the data dimensions.


## Anomaly-Detection Using Interquartile Range Method:

Not all data is normal or normal enough to treat it as being drawn from a Gaussian distribution.

A good statistic for summarizing a non-Gaussian distribution sample of data is the Interquartile Range, or IQR for short.

**The IQR is calculated as the difference between the 75th and the 25th percentiles of the data and defines the box in a box and whisker plot.**

Remember that percentiles can be calculated by sorting the observations and selecting values at specific indices. The 50th percentile is the middle value, or the average of the two middle values for an even number of examples. If we had 10,000 samples, then the 50th percentile would be the average of the 5000th and 5001st values.

We refer to the percentiles as quartiles (“quart” meaning 4) because the data is divided into four groups via the 25th, 50th and 75th values.

The IQR defines the middle 50% of the data, or the body of the data.

The IQR can be used to identify outliers by defining limits on the sample values that are a factor k of the IQR below the 25th percentile or above the 75th percentile. The common value for the factor k is the value 1.5. A factor k of 3 or more can be used to identify values that are extreme outliers or “far outs” when described in the context of box and whisker plots.

On a box and whisker plot, these limits are drawn as fences on the whiskers (or the lines) that are drawn from the box. Values that fall outside of these values are drawn as dots.

![screen shot 2019-01-25 at 8 28 54 am](https://user-images.githubusercontent.com/15687230/51749142-9509ed00-207c-11e9-9d44-d4531c955f22.png)

## Anomaly-Detection Using Gaussian Distribution:

A normal distribution is a very common probability distribution that approximates the behavior of many natural phenomena. A data set is known as “normally distributed” when most of the data aggregates around its mean in a symmetric fashion. Values become less and less likely to occur the farther they are from the mean.

When a metric is normally distributed it follows some interesting laws, as in the sugar bag example.

The mean and the median are the same: both are equal to 1000 in this case. This is because of the perfectly symmetric “bell-shape”.
The standard deviation, called sigma (σ), defines how far the normal distribution is spread around the mean. In this example σ = 20.
68% of all values fall between [mean-σ, mean+σ]; for the sugar bag this is [980, 1020].
95% of all values fall between [mean-2*σ, mean+2*σ]; for the sugar bag, [960, 1040].
99,7% of all values fall between [mean-3*σ, mean+3*σ]; in the sugar bag example, [940; 1060].
The last 3 rules are also known as the 68–95–99.7 rule or the “three-sigma rule of thumb”.

Almost impossible values can be considered anomalies. When the value deviates too much from the mean, let’s say by ± 4σ, then we can considerate this almost impossible value to be anomalous. (This limit can also be calculated using the percentile.)

![Capture](https://user-images.githubusercontent.com/15687230/68154880-81968700-ff16-11e9-8aef-bea26ace1a19.PNG)


## Anomaly-Detection Using SH-ESD (Seasonal Hybrid Extreme Studentized Deviate) :

The primary algorithm, Seasonal Hybrid ESD (S-H-ESD), builds upon the Generalized ESD test for detecting anomalies. S-H-ESD can be used to detect both global and local anomalies.

This two step process allows SH-ESD to detect both global anomalies that extend beyond the expected seasonal minimum and maximum and local anomalies that would otherwise be masked by the seasonality.

This is achieved by employing time series decomposition and using robust statistical metrics, viz., median together with ESD. In addition, for long time series such as 6 months of minutely data, the algorithm employs piecewise approximation. 
This is rooted to the fact that trend extraction in the presence of anomalies is non-trivial for anomaly detection.

The figure below shows large global anomalies present in the raw data and the local (intra-day) anomalies that S-H-ESD exposes in the residual component via our statistically robust decomposition technique.

![screen shot 2019-02-06 at 2 51 08 pm](https://user-images.githubusercontent.com/15687230/52369402-ad83eb00-2a1e-11e9-895f-20e683b6ce86.png)
