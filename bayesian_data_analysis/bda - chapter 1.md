#bda #datascience #bayesian

## prior predictive distribution

The probability distribution for a variable where the parameters distribution are solely based on prior. No actual data involved. 
If we model Y as Normal with unknown mu, we can have a distribution for Y based on the prior used for mu.

## posterior predictive distribution

The probability distribution for a variable where the parameters distribution is now updated by data.
If we model Y as Normal with unknown mu, we can have a distribution for Y based on the prior + information from data. $p(\tilde y| y)$. 
Under the hood, $y$ is updating the $\theta$s and that modifies the distribution of the variable modeled.

## odds ratio

The ratio of the posterior density $p(\theta | y)$ evaluated at the points $\theta_1$ and $\theta_2$ under a given model is called the posterior odds for $\theta_1$ compared to $\theta_2$.
In words, the posterior odds are equal to the prior odds multiplied by the _likelihood ratio_, $p(y|\theta_1) / p(y|\theta_2)$
