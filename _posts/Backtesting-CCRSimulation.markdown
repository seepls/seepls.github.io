---
layout: post
title: "Backtesting in Exposure Simulation Models"
date: 2024-09-21
comments: true
tags:
- Counterparty credit risk
- Monte Carlo Simulation
- Risk factor model
- Simulation models
- Pricing models
---

Backtesting in CCR is needed to evaluate model performance, for both simulation and pricing models. 
This blog contains 2 parts :    
Part A: Key Risk factor simulation backtesting: for testing the simulation model suite    
Part B: NPV matrix and Portfolio exposure backtesting: for testing pricing models  


For testing following choice of input can be taken :  
a. Testing sample :   
In-sample test : sampling window is same or inside the calibration window.   
Out-of-sample test : Sampling window is outside calibration window.   

b. Risk horizons for various products:  
OTC derivatives : up to 1 year  
SFT(Bonds, CMBS, RMBS/TBA) : upto 3 months   
Prime Brokerage service : 5 day    

{: .center}
PART A : Risk factor backtesting 

Risk factor backtesting is performed for testing simulation model. That is to test unknown distributions. 
The idea is to prove the distribution of simulated path and historical path are of the same kind.  

For different asset class different risk factors are simulated :   
IR - IR Rate, IR Vol   
FX - FX Rate, FX Vol  
Equity - Index, Single name  
Commodity - Commodity Price, Commodity vol  
Credit - issuer spread , generic curves  

Null hypothesis is:   
H0: No statistic difference between the 1 risk driver’s historical path and the N simulated paths.  

Steps :
1. Get the rank of the historical path among the 5000 simulated path i.e. s_i , 
Rank : $s_i < his < s_i+1 $  
2. Compute the u-value : $(i +(his - s_i / s_i+1 - s_i)) /N $  
3. Sort the u-values and compute the distance using either AD/CM  
4. compute distribution distance with U(0,1) : d     
5. Get p-values from pre-calculated values for d   

Combine p-values from multiple simulation re-initialisation to get a unique p-value representing the overall backtesting result by Stouffer aggregation. 

{: .center}
PART B : Portfolio exposure backtesting 

Trade exposure backtesting is performed for testing pricing model.   
Here the null hypothesis is,   
H0 : Historically realised value should not show statistical difference than simulated exposure value. Predicted value should be >= realised.   

Count sample size and breach size, and test It against Binomial distribution.   
P-value = 1 - C(n,k,p)  

Traffic light criteria:   
P-value > 5% : green   
P-value <0.01% : red   
Rest is yellow   

Breach count is done as follows, for collateralised trades we backtest the MPoR profile and for uncollateralised trades we test the shift profile:   
a. For uncollateralized exposure : MtM > PFE profile   
b. For collateralised : MtM increase in MPOR > collateralised PFE profile   


It is important to note that the breaches can be aggregated on multiple portfolios but not on different risk horizons.   



Some important papers:   

1. Sound modelling and B.T framework for forecasting IM - Daniel Aziz   
2. Forecasting IM requirement : model evaluation - Paul Giltinan   


























<!--- Our experiments and approaches are detailed here- [Report]({{ site.baseurl }}/docs/pommerman_report.pdf)
{: .center}
![Pommerman]({{ site.baseurl }}/img/pommerman.gif "Pommerman"){:style="max-height: 500px;"}

To see our agent in action, watch the video below-

<iframe width="100%" height="400px" src="https://www.youtube.com/embed/DtiyIrMZ69A" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>

--->
