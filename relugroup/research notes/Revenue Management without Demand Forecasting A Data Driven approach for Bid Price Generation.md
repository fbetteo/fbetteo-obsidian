Eren, PROS
## Abstract

Revenue management typically needs demand forecast and WTP and for that require data to generate those forecasts. It's inconvenient for companies with low amount of data and requires a good forecast and assumptions on the DGP.

The paper proposes an approach based on historical data but with less requirements and without explicitly forecasting demand nor optimizing anything to generate bid prices.

> ([[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=1&selection=140,1,154,50&color=important|Revenue Management without Demand A data driven approach for Bid Price Generation, p.1]])
> Our approach is an ex-post greedy heuristic to estimate proxies for marginal opportunity costs as a function of remaining capacity and time-to-departure solely based on historical booking data.


## Introduction


* ***Cargo demand is hard to predict** because it's really unstable. "Late and lumpy".

> ([[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=2&selection=89,0,123,0|p.2]])
>  The booking horizon is very short with most of the requests happening within the last few days before departures, hence the late arriving demand. Moreover, shipment sizes have a lot of variability in both volume and weight and are often lumpy


* **RM requires data for demand forecast.** The past data, which is used, is also a function of past prices, availability and capacity limits so usually an unconstraining / uncensoring methodology is required and needs to be user for future demand which will depend of future prices. [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=2&selection=238,0,239,30|p.2]]

* After demand forecast usually there is an optimization step. There is a feedback look in the sense that current recommendations will affect future bookings that will be used for further training and so on. If the forecasts are wrong you can get into a vicious deteriorating cycle. [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=2&selection=364,15,393,0|p.2]]

This paper proposes a methodology that doesn't require explicit demand forecasting and requires less historical data.
> [!PDF|red] [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=2&selection=420,1,464,11&color=red|p.2]]
>  Is it possible to directly prescribe control parameters of the RM system using historical data without any demand forecasting? We propose a novel methodology that does not rely on the abovementioned framework with demand forecasting and optimization. It utilizes historical booking data to directly generate the required RM output, skipping demand forecasting altogether. Our approach also alleviates the need of extensive historical data and eases the implementation of RM in practice

It focus on bid price and generates a robust one based on their simulations.


## Bid Price


Defines bid prices as:
> [!PDF|red] [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=3&selection=398,0,429,31&color=red|p.3]]
>  It represents the expected loss in future revenue from using the capacity now rather than reserving it for future use. Equivalently, bid price can be defined as the marginal value of capacity. This definition is also in line with expected marginal seat revenue (EMSR) (Belobaba, 1987; Belobaba, 1989

But also explains some benefits of using them such as:
* Economical interpretation
* Easy to implement, requires just a lookup table of bid price per remaining capacity and time to departure
* For future request one just looks for the bid prices associated and sum them all
* Very good performance as theoretically near optimal asymptotically as demand and capacities scale up.


## Problem description and methodology

Here it mentions and do a summary of other methods.
### EMSR 

There are multiple ways of calculating expected marginal revenue.

* Littlewood's rule is the foundation of EMSR. **It requires a probability distribution of demand**
> [!PDF|yellow] [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=6&selection=196,0,264,1&color=yellow|p.6]]
> Littlewood’s rule states that one should only sell the lower fare class when 𝑓2 ≥ 𝑓1𝑃(𝐷1 > 𝑠), in other words, if its fare is greater than or equal to the expected marginal revenue of 𝑠𝑡ℎ seat. This threshold is essentially a bid price as defined in Section 1 and is equal to the higher fare class’s fare multiplied by the probability of getting at least s passengers for that class


* EMSR is an extension of Littlewood's rule that can handle multiple fare classes. You also need the fixed fare classes prices and the demand forecast for each fare class.

### DP Generated Bid Price

* Demand for each departure date follows a Poisson distribution with arrival rate $\lambda(t)$ where *t* represents time to departure.
* Probability of purchase at a given price *p* and time *t* is: $P_w(p,t)$ 
* $P_w(p,t)$ is the probability of WTP of the arriving customer at time *t* being greater than offered price *p*
* We create intervals of length $\delta t$ such that probability of two arrivals happening is an interval is negligible.
* *x* is the number of available seats at any given time *t*

> ([[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=7&selection=20,1,28,8&color=yellow|p.7]])
> We can model the pricing process via a DP formulation as follows:
![[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=7&rect=70,530,553,690&color=red|p.7]]

*In (1).*

$\lambda (t) \delta (t) · P_w(p,t)$ is the probability of purchase in the current interval.
$\lambda (t) \delta (t)$ is the expected user arrival in the interval (less than 2 as per the assumptions)
$P_w(p,t)$ is the probability that customer purchases at the given price *p*

So, first term is what happens if a user comes and pays.
Is the probability of a purchase in the interval times *p* (payment received by the customer) + $V(x-1, t - \delta t)$ which is the recursive call to the function, but now with *x-1* remaining seats, because you sold one and one interval in the future $t-\delta t$.

The second term in what happens if we don't have a sale in the interval
$1 - \lambda (t) \delta (t) · P_w(p,t)$ is the probability of no purchase and it's multiplied by $V(x-1, t - \delta t)$ because there is no *p*, as no fare was collected but the recursive call is done, now one interval into the future but with the same amount of remaining seats.

**V(x,t) is a value function of the expected revenue to come from future sales given x units of remaining inventory and t time units to departure.**

----
#### **I think (2) is wrong and should be**

$V(x,t) = \max_{p} \{\lambda (t) \delta (t) · P_w(p,t) (p - b(x, t-\delta t))\} + V(x, t-\delta t)$  

This is also validated in the appendix A where it seems to use p-b..

----

The problem can be solved recursively with boundary conditions.
$V(x,0) = 0$ and $V(0,t)=0$ 
Which means, there is no  value if you have remaining seats but no time to sell them and there is no value if you don't have anymore seats to sell but still time to departure.

> [!PDF|red] [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=7&selection=311,73,385,67&color=red|p.7]]
> Equation (3) provides a mathematical representation of how bid price measures exactly the expected revenue loss from having one less seat at a given inventory level and time-to-departure, and can be solved iteratively using the boundary conditions given by 𝑉(𝑥, 0) = 0 ∀𝑥, and V(0, 𝑡) = 0 ∀𝑡. Conceptually, 𝑏∗(𝑥, 𝑡) is related to EMSR of the 𝑥𝑡ℎ unit of resource (seat), with EMSR not capturing the time component


For all of this you need Demand forecast and WTP distribution to find the optimal values.
> ([[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=7&selection=462,0,476,1&color=yellow|p.7]])
>  Equation (3) provides the theoretical optimal when those parameters are estimated with 100% accuracy, which is never feasible in practice. Therefore, parameter and model misspecifications frequently result in suboptimal decisions (Nambiar, Simchi-Levi, & Wang, 2019).


> [!PDF|yellow] [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=7&selection=492,29,561,60&color=yellow|p.7]]
>  For any fixed remaining inventory level, 𝑥, the optimal bid price 𝑏∗(𝑥, 𝑡) increases as time-to-departure, 𝑡, increases. Similarly, for any fixed time remaining to departure, 𝑏∗(𝑥, 𝑡) decreases as remaining inventory level, 𝑥, increases. These monotonicity properties are intuitive, as the inventory tends to become more valuable as the remaining inventory level decreases, and it becomes less valuable as the departure time approaches. As remaining inventory usually keeps decreasing as the departure date approaches, these two opposing effects are expected to counterbalance each other to produce stable bid price values along the booking horizon


## Data drive approach

#### Data

For each flight of a leg, it uses the sequence of prices and time to departures of each booking.
Sometimes this could be aggregated into time intervals (days for examples)

> [!PDF|red] [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=8&selection=291,0,337,1&color=red|p.8]]
> Our objective is to generate a bid price matrix, 𝑩 = [𝑏(𝑥, 𝑡)], from historical data 𝑷 and 𝑻, where 𝑏(𝑥, 𝑡) is the bid price, or the opportunity cost for remaining capacity of 𝑥 at the remaining time of 𝑡.

#### Methodology

*Observation building step* consists of transforming the historical data into a proxy of bid prices, which is then used to train a model (NN) to fix its parameters so the model can be used to estimate future bid prices.


First, without taking time into account, it explains that the current prices of a flight could be used as proxy of bid price. The highest paid fare could be you bid price if one seat was remaining, the 2 highest if 2 seats remained, and so on. The logic is that, you know there is someone willing to pay the highest fare, so if you had one seat left, you could save it for him (I guess you can't sell it before?).
So, it starts creating a new array of prices $\widetilde{P}^i$   that is the ordered list of prices and any remaining seat greater than actual bookings (if the capacity is larger than the actual bookings) is set to 0, as those seats have no value if not sold.

We do this for all flights independently.

Adding the remaining capacity as X we can create a simple ML model. Check appendix B for numerical example comparing to EMSR.


Then it includes Time to departure as feature. Aggregating into some interval, in this case days for instance.
Then for each day, it includes all the prices up until departure and creates a matrix. Then, for the sake of getting a suitable table for modeling, it expands the matrix into two columns of T and Capacity index, and a Y column with prices.

![[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=9&rect=66,76,547,289&color=red|p.9]]
![[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=10&rect=51,275,553,750&color=red|p.10]]
#### Model

Then using this data it generates a model to learn the bid price relation given inventory level and time to departure.

To differentiate with previous optimal bid prices, it's called here $b^{dd}(x,t)$ 

There is no need to use a Neural Network as in the paper.

**Characteristics** [[Revenue Management without Demand A data driven approach for Bid Price Generation.pdf#page=11&selection=289,0,319,14&color=yellow|p.11]]
* Doesn't require demand forecast
* No assumptions on the data
* Greedy
* No extrapolation
* You can extend it to more features (seasonality, etc)


Isn't this similar to the price estimation in Double Machine Learning ? Although here we re arrange prices in sorted order

* We can add seasonality and other features to the X matrix.
* We can use this for group sales
* Can be extended to network with some preprocessing