---
layout: post
title: "Portfolio Compression"
date: 2026-04-08
comments: true
tags:
- Trioptima Trireduce 
- OTC derivatives
- Exposure/ Risk Reduction
- CDS 
---

Portfolio compression is a risk reduction (**multilateral netting**) excercise where one tends to preserve the market risk position of each counterparty but reduce the settlement and counterparty risk by trade elimination / Notional reduction.
It cancels offsetting trades while keeping each party’s net exposure unchanged. Banks “tear up” redundant contracts so they carry the same credit risk at much lower gross notional. 

**Compression cycle (Banks do it quarterly)**
1. All relevant transactions. Post-2014 standardization helps – single-name CDS use fixed coupons (100 or 500 bps) and common IMM quarter dates (20 Mar/Jun/Sep/Dec) 4, so trades are grouped accordingly.
2. Match transaction details to each bilatral counterparty.
3. Run the trioptima's trireduce algorithm.
4. Result : Unwinding of transaction/ execution of new transactions / Novating transactions to other counterparty. 


**Compression Algorithm**:
Reduce the gross notional wihtout changing the net possition of any counterparty.
-  Compression engines model the network of obligations and identify closed loops. eg. a cycle (c1→c2→c3→c1) 
-  For each loop, subtract the minimum notional on that loop from every trade in it.
- In practice, TriOptima’s patented method uses a **depth-first-search to enumerate all cycles** and then **greedily “compress”** each one by the smallest edge.
- repeat till no closed ring left. 
<img src="https://raw.githubusercontent.com/seepls/seepls.github.io/main/img/Compression.jpg" alt="Compression" style="max-width:100%; height:auto;">

**Coupon Blending for non-standard CDS trades**: 
By its nature compression requires standard contracts--> fungible --> cashflows are easily netted. 

CDS contracts now trade with a fixed premium and upfront payment with fiex termination date of 20th of Quarter end month. 
Hence positions can be bucketed according to underlying refrence entity and maturity. 

However where not possible --> use of coupon blending. In effect, two parties with mismatched coupons can be offset by having a buyer pay/ recieve an upfront cash equalizing the difference in cashflows. 

**Benefits**: 
- Operational Cost --> reduction of cashflows
- Regulatory --> Reducing the gross notional has impact on the Reg capital requirement --> Leverage Ratio.

**Limitations**:
- Trioptima ignores zero sum loops.
- Pricise trade data requirement
  
