# CVA-calculation-with-QuantLib-and-Python

## Introduction
This code presents a way to calculate the credit value adjustment (CVA) for a netting set of plain vanilla interest rate swaps

The credit value adjustment is the difference between the risk-free price of a netting set and the price which takes the possibility of the default of the counterparty into account. A netting set is a portfolio of deals with one counterparty for which you have a netting agreement. That means you are allowed to set positive against negative market values. A netting agreement will reduce your exposure and therefore the counterparty credit risk. If the time of default and value of the portfolio are independent then the CVA is given by: 

$$ CVA = (1-R) \int_0^Tdf(t)EE(t)dPD(t)$$

with recovery rate R, portfolio maturity T (the latest maturity of all deals in the netting set), discount factor df(t) at time t, the expected exposure EE(t) at time t and the default probability PD(t).

Using Monte Carlo method to estimate the integral by the discrete sum

$$ CVA = (1-R) \sum_{i=1}^n df(t_i)EE(t_i)(PD(t_{i-1})-PD(t_i))$$

## Calculate the discounted expected exposure

Using the todays yield curve to calculate the discount factor for each point on the grid. With the numpy function vectorize we generate a vectorized wrapper of the discount method of the QuantLib YieldTermStructure. This vectorized version can be applied on a array of ql.Dates or times instead of a scalar input

After the generating the market scenarios and pricing the netting set under each scenario we will calculate the discounted NPVs for each deal in the portfolio

Using the discounted npv cube we can calculate the discounted expected exposure

## The default probability of the counterparty
To derive the default probability one could either use market implied quotes (e.g. CDS) or use rating information (e.g. based on historical observations). We assume that the survival probability is given by $sp(t)=\exp(-\int_0^t \lambda(t))$ with a deterministic piecewise-constant hazard rate function $\lambda(t)$. Given a grid of dates $d_0<\dots<d_n$ and the corresponding backward flat hazard rate $\lambda_0,\dots,\lambda_n$ we can use the Quantlib HazardRateCurve to build a default curve.

The QuantLib provides a real bunch of different types of DefaultTermStructures. You can either bootstrap a default curve from CDS quotes or you build a interpolated curve like we do here and combine one of the many interpolators (Linear, Backward Flat, etc.) with one of the possible traits (hazard rate, default probability, default density).

With the default termstructure we can calculate the probability for a default between the times $t_i$ and $t_{i+1}$ for all i in our time grid.

Again we use the numpy function vectorize to apply a scalar function on an array. The method defaultProbability takes two times as input, t and T. It returns the probability of default between t and T.
