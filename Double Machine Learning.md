#datascience #relugroup #pricing
# Goal

Perform estimation of model with a causal framework in mind. The idea is to estimate parameters in a unbiased way, retrieving the correct parameters on top of having good predictions. They show an example of a ML model that has good performance but the parameters of interest are totally biased and hence, can't be used for decisions or down the pipeline for other tasks. We can't know the true effect of a feature in the response variable.

There are multiple kind of data generating process that can be modeled with this. In general we need to have a treatment (some parameter of interest) and nuisance parameters (the ones that we want to use as control/confounders but are not the main goal). Those nuisance features have an effect in the outcome but can also have impact on the treatment itself.

ML methods out of the box usually give biased estimates because of regularization in some way or another to avoid overfitting.

[Chernozhukov](https://www.youtube.com/watch?v=eHOjmyoPCFU) is the main founder of this methodology

### Why

ML and regression without proper handling introduce bias in the coefficient estimation.

#### PLM 
Basic example is with partially linear model structure

Simple naive 2 stages modeling where we model the impact of X on Y with some ML model and then use that estimate in the next model where we model the effect of the treatment coefficients on Y with a parametric model ends up in biased coefficients, not showing the true parameters.


### Solution to regularization bias: Orthogonalization

Frisch-Waugh-Lovell Theorem for linear regression states that given a treatment , we can estimate consistently the parameter if we partialize out X.
This means:
* running  a regression of Y on X. Get the residuals
* running a regression of D (treatment) on X. Get the residuals
* running a regression of the first residuals on the second residuals.

Orthogonalization -> FWL theorem can be generalized to ML estimators instead of OLS


### Sample Splitting

Not using the same fold for both stages to avoid biases from overfitting.
Cross-fitting: swapping the roles of train and hold out.
For each fold, we train the first stage (nuisance model + treatment model (price in this case)) on the hold out, and the second stage model (the one for the causal parameter) on the rest of the data. We do this for each split and average the parameter results.
This reduces the bias from overfitting and has a relatively low variance (as in crossvalidation)

Kumar 2022
>Similar to [19, 27, 31], and [28], we use cross-fitting approach in our two-stage scheme to prevent overfitting, where the data is split into K-folds and for each fold the first stage predictions used in the second stage estimation are from first-stage models trained on other folds. As shown in [19] prevention of over-fitting in the construction of first stage estimators for E[Yit|Xit] and E[Pit|Xit] plays a key role in getting √ n-consistent two-stage estimators for θ

>The first-stage estimators for E[Pit|Xit] and E[Yit|Xit] are constructed using a random forest regressor where the number of decision trees to be ensembled are fixed to 100 and use a mean squared error loss function. The features are the same as the ones used in the direct method explained in the previous Section. Moreover, to avoid overfitting, we use a cross-fitting approach and split the data into 5-folds with random shuffling. When estimating the second-stage model on each fold, we use the predictions from the first-stage models trained on other folds

Chernozhukov 2016
>While sample splitting allows us to deal with remainder terms such as c ∗ , its direct 4Each of these works differs in terms of detail but can be viewed through the lens of either “debiasing” or “orthogonalization” to alleviate the impact of regularization bias on subsequent estimation and inference. 6 CCDDHNR application does have the drawback that the estimator of the parameter of interest only makes use of the main sample which may result in a substantial loss of efficiency as we are only making use of a subset of the available data. However, we can flip the role of the main and auxiliary samples to obtain a second version of the estimator of the parameter of interest. By averaging the two resulting estimators, we may regain full efficiency. Indeed, the two estimators will be approximately independent, so simply averaging them offers an efficient procedure. We call this sample splitting procedure where we swap the roles of main and auxiliary samples to obtain multiple estimates and then average the results cross-fitting. We formally define this procedure and discuss a K-fold version of cross-fitting in Section 3.


Averaging formula in pg 24
### DOUBLE ML (library)

COMO LO ESTIMO? NO ME DEJA SETEAR VARIOS PARAMETROS DE INTERES
NO SE COMO DECIRLE QUE ES UNA POISSON Y NO LINEAL