#bda #datascience #bayesian

## posterior interval

*  central interval of posterior probability. 
A 95% central interval is a continuous one that bounds exactly 95% of the posterior probability.

* HPDR (highest posterior density region).
Like the central one  in the sense that it contains X% of the distribution but also the density inside the interval is never lower than outside. For unimodal and symmetric densities, the two intervals are equal but for more weird ones they differ.

Central interval is more easily computed but for skewed or multimodal distributions it's worth having also HPDR or other kind of summaries too.

## conjugacy

The property that the posterior distribution follows the same parametric form as the prior distribution. The beta prior distribution is a conjugate family for the binomial likelihood.
Easier computations but not always the best choice.

## non informative and weakly priors

Non informative can be applied quick, when it's not worth to quantify one's real prior knowledge. Also, it's not worth to try different non informative priors since the likelihood should dominate all of them.

Weakly informative priors are proper interval that convey **weaker** information that the actual prior knowledge available. It's usually a small amount of real-world information to make sure the posterior makes sense. Like putting limits to the region the parameter can belong to. Uniform in some interval, being sure a parameter is positive, etc