# Splines
#datascience
Advanced data analysis from an elementary point of view. #Shalizi. Chapter 8.

Based on: 

Starting from one dimensional data to get the intuition, we can think of splines as a regression where we control the smoothness directly (a parameter for *smoothness*) instead of indirectly controlling it through the bandwidth as in kernel regression.

The spline objective function is:

$$\varphi(m,\lambda) \equiv \frac{1}{n} \sum_i^n (y - m(x))^2 + \lambda \int(m''(x))^2 dx $$
The first term is the classic mean squared error. The second term is the second derivative of the function m with respecto to x. It's a measure of the *curvature* of m at x. If *m()* is linear, then this derivative is 0.  
It is squared because we don't care if the curvature is positive or negative (concave or convex) and  "we then integrate this over all x to say how curved m is, on average" [^1]
"Finally, we multiply by $\lambda$ and add that to the MSE" (Shalizi, 2015).  
Basically we are adding a curvature penalty to the MSE, where for two equal MSE , we prefer the function that has less curvature.

The function that minimizes this problem is a **smoothing spline** or **spline curve**.

* All solutions, no matter what the data might be, are piecewise cubic polynomials which are continuous with continuous first and second derivatives
* Splines are linear smoothers -> predicted values are linear combinations of the training set response values $y_i$.
* Boundary points are called **knots** and the extrapolation to beyond the smallest and larges points is linear.

$\lambda$ controls the curvature of the function and implies a bias-variance tradeoff. Higher values penalize more the curvature and hence results in a "more linear" function, with lower variance but highest bias. On the contrary, lower $\lambda$ values allow more wiggly curves with less bias and higher variance.   
Usually we can search for the optimal $\lambda$ via Cross-validation or Leave-one-out CV.

We said splines are piecewise cubic polynomials. To fit that we can define four **basis functions**.  
$$B_1(x) = 1$$$$ B_2(x) = x$$
$$ B_3(x) = x^2$$
$$ B_4(x) = x^3$$
For a typical cubic polynomial we would then choose a function that is linear combination of the basis functions such as:
$$\mu(x) = \sum_{j=1}^4 \beta_j B_j(x)$$
This could be solved with classical OLS.

However, splines are piecewise cubic polynomials so this is not so straightforward.  The knots define how many pieces there will be in our estimation and on top of that there must be continuity across segments. To account for that there are some rearrangements needed and the actual basis functions are not the ones we wrote above.[^2] In Shalizi you can see the actual new basis.

For multivariate regression the logic remains but the formulas get a bit more complicated. Shalizi is not really explicit about it. Maybe another resource is worth checking.

### Splines vs Kernel Regression

Splines are fast to run due to arithmetic tricks and it's really clear about the smoothing applied.

Kernel regression, although slower, is a simpler to program and results are more analyzable. It's also easier to expand to multiple independent variables and to combinations of numerical and categorial variables.



[^1]:Why average if it's integral? shouldn't it be a sum?
[^2]: The book mention that the basis election is not obvious and can take multiple forms (sine, cosine, other waves). Where do that impact? Not cubic but one of those? 