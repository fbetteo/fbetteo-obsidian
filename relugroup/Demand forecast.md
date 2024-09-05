
Check if there is any answer [here](https://www.linkedin.com/feed/update/urn:li:activity:7224705784704610304?commentUrn=urn%3Ali%3Acomment%3A%28activity%3A7224705784704610304%2C7224733552578379777%29&dashCommentUrn=urn%3Ali%3Afsd_comment%3A%287224733552578379777%2Curn%3Ali%3Aactivity%3A7224705784704610304%29).
##### Terminology:
DD = Days to departure

# Goal

Given a flight (route + departure date)  we should be able to predict the _demand_ for any day in the future, at a given price.

If there is a flight in 20 days for Cairo to Jeddah we need to have an estimation of daily bookings, day after day until departure. Since the price can (will) change over time as fares are closed and open, we need to  be able to give an estimation for any price that would be _equivalent_ that estimating the demand for any given fare.


# Assumptions and restrictions
######  1)  Fare classes


*In general*
Fare class differ on price but maybe also by some attributes such as restrictions, cancellation policy, luggage. 
We started with FlyEgypt so we didn't have to care about this but in general I think we should have the attributes (or some engineering of them) as feature. Fare class itself can't be an attribute for DML since it's like having the price? See Feature Availability section.

*For Flyegypt:*
Initially we assume fares in a particular class (economy/business) are only different in price but don't have different characteristics as flexibility/baggage/etc. This should translate to: one fare is available at each time per class. (we need to validate this) If not, there is no incentive to pick the more expensive.  If this isn't true we still can estimate a model but it won't initially be taking into account the differences. 
If a set of products is available we could use some Consumer Choice model (Nikseresht 2017 - to be revised) to model the demand based on utility and availability.

###### 2) Prediction must be lower than maximum airplane capacity 
We need to make sure our estimates don't predict more sales than the maximum number of available seats. If we do daily independent predictions we have no guarantee that the sum will be lower than the max. If we add fares we will have more predictions for the same day (what if we simulate there are 5 fares the same day? we will sum 5 booking predictions if we don't control in any way) 
	**Options:** 
		* Use cumulative bookings as feature (should help but no guarantee the sum will be lower)
		* Post prediction adjustment: check available seats and constrain prediction when there are no more left (at the total plane level initially for FlyEgypt, per fare eventually if the airline has limits per fare)

More complex scenario (adding fares): 
Let's say we are 12 days from departure and the fare is $100.
With an independent model for bookings we will estimate some demand X, let's say 5 bookings. So far so good when there is only one fare.
What if we want to "simulate" there is another fare available. Same seat, all equal but price is $105.
If we add that row in our dataset, our model will predict 5 bookings for the $100 fare and some lower booking for the new one (but similar since the price is almost the same), let's say 4 bookings. 
Now by simulating this, we are saying that adding a fare increases the number of bookings. Our model doesn't know anything about the other availabilities and will predict, 5 and 4. We need some way to avoid that. Maybe the Consumer Choice model can fix that somehow in the sense that the new fare has a strictly lower utility (same product, higher price) and no customer would take it if both are available. 

###### 3) Price drift over time

Price for a given fare in  a given route might change over time, not only because of the time of the year but maybe because of inflation or business decision. We need to take that into account. 
Options:
	* normalize each fare with respect to the maximum fare so the model has a sense of relation (I wouldn't remove the actual price because if everything goes up demand should decrease even if a given fare has the relation to the maximum fare)
	* use real USD and not nominal (adjust prices by inflation). If prices increase more than inflation demand should decrease (except some shift in demand curve)

###### 4) Constrained data

The sales/booking data is by definition constrained if a fare is closed some day. This means that that day and next days, potential customers _wanted_ to buy that fare and couldn't because it wasn't there and that's not reflected in the data.
More in specific, maybe there were 10 daily bookings on average for a fare, and on a given day the airline closes the fare after 2 bookings. That day we will have only 2 bookings recorded despite having probably more customers interested (next days will have 0 bookings recorded..).

Options:
* estimate unconstrained demand. EM algorithm is usually suggested although there are other alternatives. Unconstrained demand could be (I think) estimated as part of the modelling itself if we use a model that returns easily probabilities of being greater than > threshold (Poisson for example).
Alternatively we can estimate the unconstrained demand for the days affected with EM and a simple model such as the Poisson one but then use that "unconstrained" data as input for any other model.


# Feature availability

This is crucial, we need to create models having in mind what data will we have and how much in advance. Different goals might have different availabilities.

**Warning 1**:
If we want to predict the daily bookings for a flight departing in one month. And having all those predictions now, we can't use any feature that involves lags of sales (for DD=2, using DD=3 bookings) since we don't have that info now (we could recursively estimate it but in the end we depend on the initial data - like time series do).
If we want to predict on a daily basis we could use it, because if we are on DD=3 we can have today's booking and predict DD=2 (hypothetically, maybe the data is not available the same day.)

**Warning 2**:
For Double Machine Learning (and others?) we shouldn't use the fare class as input, because DML requires an estimation of the bookings without price, and fare class is one to one tied to the price.
I think we could use attributes of the fare class if we had, like, extra luggage, cancellation policy, etc because many fare classes might have those. For FlyEgypt fare class is only a price category so it's like inputting price.

**Warning 3**
For DML, we need to use crossfitting to remove bias. Since we will split data in folds, we discussed if we can do it shuffling or the time series aspect of data would impact. We concluded that each flight is independent of each other and time series will exist *inside* each flight but not across flights. There is seasonality but we won't use other flights sales to estimate bookings. The only thing I think we might need to avoid is to use trend or features similar since that carries information from flights in different time periods.


**Note**: 
Should we have a model for total flight bookings or sum daily predicted bookings?

**Note2**: 
Is it worth (how much complexity adds?) to predict daily bookings using lagged data that we don't have and hence we need to recursively (predicting row by row) forecast compared to have a model that predicts without the need of lagged data?




# How

ML Model  defined as regression problem (some literature suggests time series model but it's not clear that's what we need to forecast bookings given days to departure and price)

**Chosen option:**
Double Machine Learning based on [Kumar 2022](https://arxiv.org/abs/2205.01875)

Some notes:
* We need to use crossfitting
* The second stage model needs to include the intercept (not obvious in the paper) to recover the true parameters (see DML_simulation.py)
* For prediction, we can't use the original equation that only depends on price. We need to use the reduced form (Price - expected price) since we don't have an estimation of the parameters that only depend on X. We have a estimation of  E(Y|X) but not the parameters. That model is not exactly that part of the equation and hence using it as if it was is not good enough. In the simulation we get much worse results. So, the prediction also depends on the price estimation.

Other Options:
- Poisson regression (regression for count data, rather simple and works well)
- Consumer Choice Model (maybe this can incorporate different fares )
- Xgboost/NN/Any model 
- Bayesian modeling to work with probabilities straightforward
	- Hierarchical model? Specific flight -> all flights in the same route in the time of day -> all flights in the same route in that day -> all flights in the same route -> all flights ?



# Data

Some inputs:
* days to departure
* price (nominal / real)
* price ratio to highest fate
* lagged bookings 
* cumulative bookings so far
* departure_week_of_year
* departure_day_of_week
* departure_is_holiday
* departure_specific_more_important_holidays?
* departure_time_of_day
* destination_event (near the landing date)
* ...


# Evaluation

Metric at the observation level across all flights such as:
* RMSE
* WMAPE for more interpretability

Metric at the total flight level
* RMSE
* WMAPE


Important: We should have this same metrics splitted by days to departure to see how we perform at different intervals. 
Days to departure way before the flight usually have 0 sales and forecast will be close to 0 for all those dates, which are the majority if the flight is offered 1 year in advance. This will inflate our metrics because we will correctly predict all those days, which is trivial.