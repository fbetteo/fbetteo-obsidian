PAPER : https://academic.oup.com/aje/article/177/4/292/147738

Interpretation of coefficients in regression is not equal for all of them. It highly depends on the DAG and readers might interpret all the coefficients as total effects when they are not, confusing total effects with direct effects.

Total effect : net of all associations of a variable through all causal pathways to the outcome
Direct effect: an association after blocking or controlling some of those pathways.

I understand confounding as the situation when 2 variables   X, Y both affect Z but on top of that X affects Y too. That path gets blocked if we control by X when estimating Y.

In a linear model without interaction:
To interpret a coefficient as a direct effect of the variable  after blocking it's effect on the exposure variable (the one we care the most) we must adjust for all confounders of both the exposure-outcome relationship and the mediator-outcome relationship.

In Fig2 of the paper we see that not controlling for U will change the interpretation of the coefficient $\beta_2$  because smoking is confounded by U. 
Despite that, $\beta_1$ is still a valid estimate because all of its confounders are blocked. U doesn't affect the outcome through HIV.
I don't completely understand why $\beta_3$ is no longer valid if we don't adjust for U. They are connected through smoking (although not directionally) but I don't fully get the intuition. 
* Is it that we don't fully know how much of smoking is affected by Age and how much by U if we don't know U?



In case of a model with interactions, the situation remains but we need more careful interpretation. The 0 value is important for interactions. 

##### If there are no uncontrolled confounders:
$\beta_4$ is the change in the HIV effect by smoking one additional pack holding age fixed but also the change in the direct effect of smoking by changing HIV. 
**The important thing remains to distinguish between total and direct effects**. Have a DAG in mind.

##### If there are  uncontrolled confounders:
if the dag and model are right, then the effect of HIV in stroke would still be valid because we are controlling by smoking and age. ($\beta_1 + \beta_4 * smoking + \beta_5 *  age$) but the individual interpretation of $\beta_4$ and $\beta_5$ is not valid anymore because we don't know how much the uncontrolled variable U is affecting "$\beta_4$ would be biased by U effects as a measure of modification of the HIV-stroke log odds ratio by smoking."
The same way, $\beta_2$ and $\beta_3$ can no longer be interpreted as the direct effects of smoking and age (because of U not being controlled)