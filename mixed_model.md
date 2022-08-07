# Mixed Models
#datascience
Mixed model are also called hierarchical models, multilevel,  or fixed-random effects models. Those names refer to different characteristics of these kind of models.  
The main idea is that observations correspond to different groups. Those groups can have more that one level and hence called multilevel or hierarchical.  

###### For ex: 
We want to model sales of a brand using price as a predictor. We have observations at store level. Keeping it simple,  can we assume all the observations from the same store are independent? Probably not. But observations from store A are a priori independent from observations from store B. What if store A and B are from the same neighborhood? Should we think they are more similar between them than to store C which is in another county? If we think the former is true, then store A and B are not completely independent.  
We can say that observations from each store should share some properties (coefficients?) and that stores from the same county should also tend to be more similar.
Hence we have a stores into county nested model. Or multilevel, or hierarchical..

Fixed-random effects comes from other perspective of the same scenario. Continuing with the example, we might have initially a coefficient for price. But then as we include the stores-county structure in the model we end up having a "fixed" coefficient for price and then "random" effects for each county, and for each store, which leads to have a different price coefficient for each store (the lowest level of the structure) made by the fixed effect and the random effect. There are different flavors of model structures (like different random effects not nested) but you get the main idea.

**So, why not creating a categorial variable for each store and/or county like in regular regressions?**

If you have lots of data (many observations per store and many stores per county[^1]) you would end with similar results. So, it was a good suggestion.  
The main benefits is when you don't have many observations per store (or even better, some yes but not all).  
If you think of putting an interaction for a store with few observations, the coefficients (price and intercept for example) would probably have high variance because they would be based on few data points (probably overfitting). The mixed model structure assumes a distribution for the coefficients of all the stores. More precisely, the model assumes all the price coefficients are sampled from a common distribution and this generates a regularization behavior that is not present in plain categorical interaction.  
The price coefficient for the store with few observations would be shrunk towards the average price coefficient of the stores in the county avoiding extreme results for that store. Same applies for counties.  


##### Other point of view
I don't remember the exact source of this but I read that:
>Fixed effects are the parameters we believe are general to the population.  
Random effects allow to estimate the variance of some parameters at some particular level and estimate how each subject deviates from the population mean.

###### For ex:
Subjects receive treatment X and get response Y. This is measured multiple times during some span.  
We can run a linear regression with a fixed intercept and a single coefficient for X but we won't be taking into account that each subject was measured many times.  
We could instead add a random effect for the intercept and slope per subject. Each subject will then have their own coefficients but they will all be centered around the estimated at the fixed level.

#### Tips and Miscellanea
* Scaling data do help reduce the runtime of fitting a model. Depending on the complexity of the model I saw up to 66% reduction (on models with more random effects). Centering the data might help but I didn't see an impact in my case.
* If you scale and want to interpret coefficients remember to de-scale. [See 1](https://stackoverflow.com/questions/24268031/unscale-and-uncenter-glmer-parameters?rq=1) and [See 2](https://stackoverflow.com/questions/23642111/how-to-unscale-the-coefficients-from-an-lmer-model-fitted-with-a-scaled-respon/23643740)Can Scikit learn handle this automatically?



I would suggest going to resources to understand:
* How the regularization works and deep dive into the assumption of a common distribution for the parameters
* How to handle and interpret other predictors that are not at the most granular level, for instance, a variable per county (GDP of the county) that would be identical for all the stores of that county. Those variables can be included but I'm not going through it here.
* 

#### Resources

Formal:
* https://arxiv.org/abs/1406.5823 (Bates et al)
* Gelman
* https://www.cmm.bris.ac.uk/lemma/login/index.php

Intuition:

* https://ourcodingclub.github.io/tutorials/mixed-models/
* https://eshinjolly.com/2019/02/18/rep_measures/


[^1]: How many?
