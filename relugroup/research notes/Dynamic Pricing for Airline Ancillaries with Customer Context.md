
Yellepeddi 2019, DeepAir

Shorter (and published?) version of *Galactic Air* improves ancillary revenues with dynamic personalized pricing*
The longer has the same technical models and insights but has more explanations about the development of the experiments and the context of the application. It's a nice read.

### Abstract
Dynamic pricing by customer.  Three (3) different models to reach that goal

### Intro
* 5% or less conversion rate for ancillary
* Legacy airlines didn't sell ancillary as individual product so not much history about it (it's a new thing to unpack right to flight from ancillary)
* Ancillary pricing affects the other ancillary products, they compete for shelf space and customer budget. Tricky thing, cannibalization.
* Ancillary. Identical product, unique clients. You need to price differently the same product to different users.

### Pricing
#### Demand Function
Evaluate the demand D at different price points. Then you can maximize revenue based on that curve.

For Ancillary, the demand curve depends on the price but also of customer attributes (user and context)

#### Customer Attributes
* Days to departure
* Departure date and time. Not only seasonal effect but also *quality effect*. Some flights are associated with higher budget passengers (and more expensive fares) because of time of the day, business trips, etc.
* Route/Market. Some routes have better conversion. More business trips, more familiy trips, etc
* Length of stay

#### Models
The paper proposes 3 models, 1 and 2 predict the probability of purchase and then price accordingly following some approach.
Model 3 do both things simultaneously using a custom loss (I'm not sure about the details)

1) Naive Bayes for probability + logistic mapping with some parameters to calibrate. Kind of rule of thumb that provides different strategies (more or less aggressive) for different probabilities. I'm not sure what's the theoretical foundation of why those curves and not others but feels intuitively right.
2) Neural Network for probability and then grid search on discrete range of prices to find the one maximizing revenue.
3) Neural Network with custom loss to do both tasks simultaneously. Revisit. Complex. In online evaluation it was the best but with marginal improvement.

#### Evaluation
##### Training
* **Price Decrease Recall (PDR)**: how likely the recommended prices are lower than the current offered prices for non purchased ancillary
* **Price Decrease Precision (PDP)**: Percentage of non purchased ancillary when the recommended price is lower than the current offered price
* **AUC** 
* **Regret Score**: On average, how close the recommended price was to the true purchase price P

##### Online
* **Conversion rate**
* **Revenue per offer**


#### Other considerations

* They AB testeed the model
* The model should give inference in seconds and integration was done to the direct site of the company
* Required a lot of tight work with other areas such as marketing, finance, legal to make everything work, respect the law and give the customer experience people the right inputs to answer questions about prices were different online and on the airport, why prices changed, etc.

