---
layout: post
title: "Initial Margin"
date: 2025-01-15
comments: true
tags:
- Margin
- Collateral
---

Initial Margin (IM) refers to collateral posted at the start of a trade to cover the potential future exposure of a counterparty defaulting before the final settlement of the contract. Unlike variation margin (VM), which is adjusted frequently to reflect changes in market value, the IM is posted upfront and is usually held until the trade's maturity or until risk reduces sufficiently.
The main purpose of initial margin is to act as a risk mitigant against market movements that could result in a loss in the event of a counterparty’s default. The IM is calculated based on expected worst-case scenarios and is designed to ensure that even during periods of market stress, sufficient collateral is available.

Monte Carlo simulation is widely used to compute counterparty exposures, due to the large number of risk factors underlying these portfolios.
Initial margin is posted upfront of the trade, however it is  reaccessed daily for cleared and less frequently for uncleared. If the IM recalculated demands more margin from the c/p, it is exchanged. Under the Uncleared Margin Rules (UMR), counterparties are required to exchange IM if the recalculated amount exceeds a certain threshold, ensuring adequate collateral is maintained.

**IM calculation:**: 

The BCBS-IOSCO rules state that the IM must be calculated as a netting set value-at-risk(VaR) for a 99% confidence level over a 10-day horizon. IM continuously changes due to market movements, amortisation or portfolio aging. Hence its important to model the dynamics of IM movements when capturing future exposure of portfolios. Once IM is  modelled it allows us to compute MVA, i.e, cost of funding the IM. 

Dynamic initial Margin(DIM) model’s purpose is to anticipate IM requirements for portfolio under simulated market condition. DIM can be modelled as:  
a). Embed a **nested Monte Carlo(MC)** simulation to estimate future IM requirements.   
b). Compute **dynamic risk factor sensitivities** for each scenario within an exposure simulation and use these sensitivities to estimate IM.     

ISDA SIMM uses first order sensitivities as I/P, having access to dynamic sensitivities in MC simulation allows direct calculation of simulated IM requirement.   

**In BCBS**:

Net Margin = (0.40 + 0.60 * NGR) * Gross Margin  

The 0.40 constant reflects the minimum risk that cannot be offset through netting. Even if a portfolio has high netting (a high NGR), there is still some residual risk that is not captured through netting, and the 0.40 is a floor to ensure that a minimum amount of margin is still required.  

This acknowledges that:  
**Unhedged Risk**: There are always some risks that are not fully offset, such as changes in volatility, liquidity risks, or model inaccuracies.  
**Future Risk**: Even with full netting, the portfolio still carries some risk of future exposure, which is why there’s a base margin requirement (0.40 or 40%).  


**DIM modelling:**

Uses regression that leverages existing simulation infrastructure.   
**Exposure = Valuation(t) - Variation Margin(t) + Any unpaid cashflow as of t - Initial Margin**   

Use of non-parametric **Nadarya-Watson Gaussian Kernel Regression**. But it requires too much data to produce stable result. 


**Reference papers/ books** 
1. Giovanni Cesari, John Aquilina, Niels Charpillon, Zlatko Filipovic, Gordon Lee, and Ion Manda's Modelling, Pricing, and Hedging Counterparty Credit Exposure: A Technical Guide (2009)  
2. Forecasting Initial Margin Requirements- A Model Evaluation - Peter Caspers

