---
layout: post
title: "Non Industry-specific Credit Spread Curve Construction"
date: 2024-09-10
comments: true
tags:
- Credit spread curve constructuion 
- Borrower Quality Rating
- Probability of default 
---

In CVA, **hazard curves** are typically used to infer probabilities that counterparties default. Built with a bootstrapping procedure, which derives constant hazard rates from CDS spreads under a given LGD or recovery rate. 
To obtain credit spread for each counterparty, we have most counterparties fall into three categories based on their liquidity:  
- Liquid  
- Observable  
- Illiquid   

<h4 style="text-align: center;"><strong>Generic spread curves for all public credit ratings: liquid counterparties</strong></h4>  

Here the credit spread curves observed from the CDS market can directly be used.   
**Step 1 - Market Curves:** Fetch Market curves i.e., CDX IG(Investment Grade, top 125), CDX HY(High Yield, 100), rolled every 6 months.   

**Step 2 - Grouping:** All entities in each CDX index are grouped by internal BQR rating. The mapping is based on historical probability of Default(PD) for internal BQRs and public ratings. It is consistent with the cross reference to Rating agency grades provided by commercial regulatory capital oversight.   

**Step 3- Removing outliers and averaging:** removal of CDS curves where the 5Y spread falls outside of 3 standard deviation from the mean. One possible reason of outliers is that agencies tend to be slower in updating their ratings than market movement.    


**Step 4- Interpolating and extrapolating on tenors** if their quotes are not available. The standard CDS market tenors include 6M, 1Y, 2Y, 3Y, 4Y, 5Y, 7Y, and 10Y. Generally quotes are needed on extra tenors 0D, 3M, 9M, 15Y, 20Y, and 30Y to mark a curve. Specifically:  
  - Mark on 9M  is equal to average of quotes for 6M and 1Y 
  - mark on 3M is 75% of the quote on 6M, 
  - mark on 0D is flat extrapolation of quote on 3M, 
  - and marks for tenors greater than 10Y are flat extrapolation of quote on 10Y.

<h4 style="text-align: center;"><strong>Spread curve for observable counterparties </strong></h4>  

A **traded market instrument** is selected, the spread curve then is constructed using selected instrument spread along with **slope of generic spread curve** corresponding to its rating. Observable counterparties do not have liquid CDS market, but they have reliable markets for bonds or loans. For such counterparties we derive credit spread from market quotes for the bonds or loans.   

**Step1: Select a bond of loan issued by the counterparty:**
The choice of bond must meet following criteria:  
  - Tenor: Maturity at or near 5Y tenor is preferred.   
  - Liquidity: Marking bond’s 30-day average Bloomberg sector Liquid Score(LQA) must be greater than or equal to 35.   
  - For names marked to Markit Partners loan prices, minimum of 3 quotes is required. 

**Step2: Obtain Spreads:**  
  - **Spread for Bonds:** these are spread over treasury obtained from Bloomberg.  
  - **Spread for loans:** these are discount margins provided by Markit Partner and are calculated as spreads over Alternative Reference Rates Committee(ARRC)’s fallback rate for legacy loans or spreads over SOFR term rate for new loans in discounting cashflows to match the corresponding price.   

**Step3: Map counterparty to a generic spread curve:** Rating based curves are constructed from the CDS quotes of the constituents in the on-the-run CDX IG and CDX HY.    

**Step 4:** Construct credit spread curve based on the bond or loan spread from step 2 or the rating based generic curve from step 3.   
a. Find the CDS whose tenor is the closest to the bond or loan maturity and use this CDS as the reference.   
b. Represent CDS curves as percentage of the spread of the reference CDS.  
c. Multiply the percentage curve with the spread of the bond or loan to produce spread curve. The resulting spread curve is counterparty-specific due to usage of its specific traded bond or loan, while the shape of the term structure is based on that of rating-based generic curve. These counterparty-specific credit curves are marked daily as bond or loan spread is updated daily.   

<h4 style="text-align: center;"><strong>Spread curves for illiquid counterparties</strong></h4>  

As no liquid CDS information can be obtained from the market directly, the solution is to construct credit spread curves indexed by BQR(Borrower Quality Rating). Map the illiquid names to the generic spread curves based on the internal rating and public rating.   

 |Internal Borrower Quality Rating (BQR)     | Rating | 
|---------------------|---------------------------------|
| 1-2                 | AAA- & A+                           | 
| 3            | A & A-                       | 
| 42             | BBB+                         | 
| 45             | BBB                        | 
| 48             | BBB-                         | 
| 52             | BB+                         | 
| 55             | BB                         | 
| 58             | BB-                         | 
| 6             | B+ & B                       |
| 7             | B- & CCC/C                         | 


<h4 style="text-align: center;"><strong>Some important papers</strong></h4>  

1. [Macroeconomic Determinants of Corporate Credit Spreads: Evidence from Canada- Nusrat Jahan](https://carleton.ca/economics/wp-content/uploads/cewp22-07.pdf)
2. [The credit spread curveI: Fundamental concepts, fitting, par-adjusted spread, and expected return- Richard Martin](https://arxiv.org/pdf/2201.01330)
