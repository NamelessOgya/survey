# Introduction to probability  
# probability  
skip
# probability mass function  
- randon variable  
  - A random variable is a numerical outcome of an experiment
  - descrete or continuous
- PMF(probability mass function)  
  - A probability mass function evaluated at a value corresponds to the probability that a random variable takes that value  
    - it must always be lager than or equal to 0
    - the sum of the possible values that the random variable can take has to add up to one  
- PDF(probability density function)  
  - pdf is a function associated with a continuous random variable
  - larger than or equal to zero everywhere
  - the total area under it must be one
- CDF(cumulative distribution function)  
  - F(x) = P(X <= x>)
- Quantiles  
  - the ath quantile of a distribution with PDF(F) is the point x so that
    - F(x) = a
- R tips
  - calc probability
    - add "p" in front of distiribution name
      - p(0.75, 2, 1) #2,1 is a parametor
  - calc quantiles
    - add "q" in front of distribution name
      - qbeta(0.5, 2, 1) #2,1 is a parametor
  - 