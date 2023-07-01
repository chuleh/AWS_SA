# Forecast

* fully managed service that uses ML to deliver accurate forecast
* eg: predict the future sales of a raincoat
* 50% more accurate than looking at the data itself
* reduce forecasting time from month to hours
* use cases:
  * product demand planning
  * financial planning
  * resource planning
* how it works
  * gather historical time-series data
  * upload to S3
  * start Forecast service => produces forecasting model => returns future sales of x item
