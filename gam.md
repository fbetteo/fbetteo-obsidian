# GAM
#datascience
Advanced data analysis from an elementary point of view. #Shalizi. Chapter 9.

"The **additive model** for regression is that the conditional expectation function is a sum of **partial response** functions, one for each predictor variable. Formally, when the vector X of predictor variables has p dimensions, $x_1, ..., x_p$ the model says that 
$$E[Y|\overrightarrow{X} = \overrightarrow{x}] = \alpha + \sum_{j=1}^p f_j(x_j) $$
This includes the linear model as a special case, where $f_j(x_j) = \beta_j x_j$, but it's clearly more general, because the $f_j$s can be arbitrary nonlinear functions."

Each partial response function is like the coefficient in a lineal regression, as they can tell how the prediction change when $x_j$ change although not that easily as a plain coefficient. Could be useful to plot the partial functions to visually understand. It's more complex but more rich.

### Estimation
Fitting the model is done via *backfitting* (or Gauss-Seidel), which is an iterative process that usually converges.

Each variable has an individual contribution so any method that can estimate a relation between two variables is accepted. You can use any non parametrical method as splines or kernel regression.
More complex things are allowed, such as using a parametric method for some variables and non parametric for others or even include interactions with a joint smoothing[^1]

Although non parametric method allow to fit *almost any* function without bias, their variance is reduced at a slower rate than linear regression. With many variables (curse of dimensionality) this can be really a problem -> Low precision due to high dimension. However, **GAM** can fit each variable individually  and avoid the dimensionality issue. Even more:
>"From a purely predictive point of view, the only time to prefer linear models to additive models is when *n* is so small than $O(n^{-4/5}) - O(n^{-1})$ exceeds this difference in approximation biases; eventually the additive model will be more accurate" (unless the true relationship is linear)




[^1]:How? More details?
