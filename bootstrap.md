# Bootstrap
#datascience
Advanced data analysis from an elementary point of view. #Shalizi. Chapter 6.

The usual bootstrap concept is the **resampling bootstrap** or **non-parametric bootstrap** where we treat the original data as a population and we sample multiple datasets where each observation has equal probability and we run our estimation in each of those new datasets.
A related version is the **smoothed bootstrap** where after resampling we perturb the observations with a small amount of noise.

Resampling bootstrap is the most conservative one since we don't need to assume nothing from the model or data. Model bootstrapping (Shalizi 6.2) gives more accurate results that resampling bootstrap at same N -- as long as the model is correctly specified! If wrong, it will give converge to wrong results... So, resampling is usually preferred unless we are really certain about the model .

We can use bootstrap to compute confidence intervals, variance and std, bias correction (?).

### Watch outs.
* Take care with dependent data since we can't sample without taking that into account (will revisit this in chap 26-27)
* If the DGP is really sensitive to small changes, bootstrap can fail big time. For resampling this means that adding or removing a few data points must change the functionals only a little. Special attention to extreme values.

### Conclusion

> "To sum up: resampling cases is safer than resampling residuals, but gives wider, weaker bounds. If you have good reason to trust a model’s guess at the shape of the regression function, then resampling residuals is preferable. If you don’t, or it’s not a regression problem so there are no residuals, then you prefer to resample cases. The model-based bootstrap works best when the over-all model is correct, and we’re just uncertain about the exact parameter values we need."