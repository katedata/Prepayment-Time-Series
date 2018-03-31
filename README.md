# Prepayment-Time-Series Analysis
***
### Steven Glover
### Thomas Cook
### Kate Cunningham
### Julia Vaillancourt

## Time Series Analysis
							
Our time series analysis looked at the Darling Practicum Group’s data. The main goal of the Darling Group is to make a model to predict prepayment of commercial loans for small banks. The general response variable is total prepayment for a bank over time. Banks want to know what their prepayment amount is since every dollar of prepayment negatively impacts the revenue they receive from interest in the future. The Darling time series contains quarterly information from 2003 through 2017 for six different banks. The start and end date for reporting for each bank varies with bank four having the longest reporting period. The data for this project includes the sum of all prepayments for bank four on a quarterly basis from quarter one of 2003 through quarter three of 2017. We analyzed the time series of total prepayment using ARIMA, exponential smoothing, and Facebook’s Prophet. We left out the last seven time periods as our test and used the prior data for our train. 
	
### ARIMA
								
Our analysis using ARIMA focused on the five-step process featured in class where we:
	
1.	Visualize the time series
2.	Stationarize the time series
3.	Plot the ACF and PACF to get a sense of what types of models to explore. Also, use auto.arima()
4.	Build ARIMA Models
5.	Forecast using the best model

Visualizing the data:

![pic1](https://user-images.githubusercontent.com/37115533/38164377-9202b4d0-34d1-11e8-9eca-cbb1ae751003.jpg)


At first glance, our time series appears to have an upward trend with some seasonal tendencies and increasing variance. What is unclear is whether there is a change in behavior right at the beginning of 2010. As a result, we tried two timeframes for our model. One used all quarters of data and the other used only data starting at quarter one of 2010. Since our data is for an anonymous bank, we do not know if the bank purchased another bank at some point or underwent some other major change which would result in a difference in behavior over different time periods. 

Stationarize the time series:

To stationarize the data, we took a single difference of the data to remove the trend and did an adf test for the data using both the differenced data and the log of the differenced data since we were unsure as to whether there truly was a component of increasing variance. Both adf tests were sufficiently small to reject the null hypothesis. We decided to analyze the data with both the log of the differenced data and the differenced data to see which performed better. 
	
	
Plot the ACF and PACF:

Next, we implemented the ACF and PACF graphs. The logged version cuts off for the ACF and tails off for the PACF suggesting an autoregressive model. For the data that was simply differenced, we found the same behavior. As for seasonality, we honestly had a hard time interpreting the graph. We suspected they cut off for both methods. Here we also did auto.arima() for both the logged and unlogged data. For the logged data auto.arima() suggested that we use ARIMA(0,1,1)(0,0,1)[4]  and for the unlogged data we use ARIMA(1,0,0)(1,1,0)[4]. Of course, the 4 is included since one full sequence of time is 4, as our data is quarterly. Interestingly, our thought that the logged data would have an autoregressive model was contradicted by the auto.arima function(). To really know whether we were right we need to fit the models.
	
Build ARIMA Models:

Next, we fit a number of different models in addition to what auto.arima found and what we suspected would be the best model from our analysis of the ACF and PACF graphs. Ultimately, we found that for the logged data an AR=0, I=1, MA=1 model had the best AIC and BIC. For the unlogged data, we found the model suggested by auto.arima was best. For the logged data, we found the model suggested by auto.arima had a seasonal parameter with a p-value significantly over .05 suggesting that it was faulty despite having otherwise good graphs supporting the model’s fit. 
	
Forecast:

Lastly, we moved on to making forecasts. We focused primarily on the fit of the graph and the RMSE of the respective models. Our unlogged data performed significantly better than the logged data after rescaling the data. We are not clear as to whether we should simply ignore the unlogged data since the increasing variance could violate the rules associated with stationarity.  Below is the best fit model with the prediction in the color red.  

![pic2](https://user-images.githubusercontent.com/37115533/38164378-93e52e72-34d1-11e8-8fbf-7a17f63db994.jpg)

	
### Exponential Smoothing
							
For exponential smoothing we followed the below steps:
	
1.	Visualize (done before with ARIMA)
2.	Figure out if trend and/or seasonality exist and, if so, is seasonality multiplicative or additive
3.	Build the model
4.	Forecast and check residuals


For visualization, we already did this in the ARIMA model. Moving onto the next step, we seem to have a trend which may be either additive or exponential. The exact shape is not clearly one or the other. As for seasonality, we suspected we had multiplicative seasonality at first. So, we used the holt-winters model with seasonality = additive and exponential = TRUE. Then, we tried some other approaches and found that the exponential = FALSE and seasonal = additive was the best model in terms of visual graph and RMSE. The exponential smoothing model had better RMSE than ARIMA, but the prediction was firmly below the actual, whereas the ARIMA was more variable around the actual. Here, the red line is the actual and the blue line is the predicted. 

![pic3](https://user-images.githubusercontent.com/37115533/38164379-955d23c2-34d1-11e8-8313-6119ee75b34d.png)
	

### Prophet
								
Lastly, we fit a prophet model. The prophet model fits a timeseries model automatically given an input dataframe. The model worked very well and had both a good fit and the best RMSE of all of our prior models. The graph of the fit is below:

![pic4](https://user-images.githubusercontent.com/37115533/38164380-96940d46-34d1-11e8-9d24-bb8696d5c75f.png)


### Prophet Versus ARIMA
							
Prophet and ARIMA both fit time series and incorporate trends, seasonality, and changing variance albeit in different ways. According to the Prophet website, Prophet’s base functionality uses an additive regression model which is a sophisticated curve fitting model. ARIMA is a generative model which means a model for generating all values for a phenomenon. Generative models generate both inputs and outputs, whereas the Prophet model fits a curve and repeats. However, Prophet includes several other techniques in ways that are not totally clear from the literature that we reviewed. 
	
Prophet claims to have the ability to make predictions on outlier type events like the Super Bowl whereas ARIMA and Exponential Smoothing cannot handle predictions like these. 
	
Prophet is said to do well with a reasonable number of outliers and missing values. The other methods do not handle these as well and we’d need to impute the missing data. 
	
### Conclusion
							
We are not satisfied with the accuracy of the time series for the total prepayment for bank four. Ways to improve on this would be to use time series models that incorporate additional independent variables like a dynamic regression or RNN. Additionally, we could break up the loans within bank four to find segments with similar characteristics that may be more predictable with the time series tools with which we are familiar. Additionally, if possible, we would like to breakdown our time intervals into smaller segments, like monthly, in order to better understand prepayment behavior.  

