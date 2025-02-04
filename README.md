# CVA-calculation-with-QuantLib-and-Python

## Introduction
This code presents a way to calculate the credit value adjustment (CVA) for a netting set of plain vanilla interest rate swaps

The credit value adjustment is the difference between the risk-free price of a netting set and the the price which takes the possibility of the default of the counterparty into account. A netting set is a portfolio of deals with one counterparty for which you have a netting agreement. That means you are allowed to set positive against negative market values. A netting agreement will reduce your exposure and therefore the counterparty credit risk. If the time of default and value of the portfolio are independent then the CVA is given by: 

$$ CVA = (1-R) \int_0^Tdf(t)EE(t)dPD(t)$$

with recovery rate R, portfolio maturity T (the latest maturity of all deals in the netting set), discount factor df(t) at time t, the expected exposure EE(t) at time t and the default probability PD(t).
