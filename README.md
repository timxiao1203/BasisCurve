# Basis Curve


Overview

	The term structure of an interest rate basis curve is defined as the relationship between the basis zero rate and it’s maturity. Basis curves are used as the forecast curves for pricing interest rate products. The increase in basis spreads has resulted in large impacts on non-standard instruments.
	The basis curve construction methodology is based on the most liquid market instruments. Normally a basis curve is divided into two parts. The short end of the term structure is determined using LIBOR rates and the remaining is derived using basis swaps.

	Summary

	Basis Curve Introduction
	Basis Curve Construction and Bootstrapping Overview
	Interpolation
	Optimization
	Yield Curve

Basis Curve Introduction
	The term structure of an interest rate basis curve is defined as the relationship between the basis zero rate and it’s maturity.
	Basis curves are used as the forecast curves for pricing interest rate products.
	Typical basis curves are 1-month LIBOR, 6-month LIBOR or 12-month LIBOR, FedFund and Prime Rate curves.
	The 3 month LIBOR curve is usually referred to as the base curve in the market. 
	The increase in basis spreads has resulted in large impacts on non-standard instruments.

Basis Curve Construction Overview

	The term structure of a basis curve is constructed from a set of market quotes of some liquid market instruments 
	Normally a basis curve is divided into two parts. The short end of the term structure is determined using LIBOR rates. The remaining is derived using basis swaps.
	A basis swap is quoted on the spread of the basis leg as follows

r_t^basis= r_t^base+s_t^ 
where 
	r_t^basis 	the zero rate of the basis curve at time t. 
	r_t^base 	the zero rate of the base curve at time t.
	s_t^  		the quoted spread of the basis swap at time t. 

	The objective of the bootstrap algorithm is to find the zero yield or discount factor for each maturity point and cash flow date sequentially so that all basis curve instruments can be priced back to the market quotes.
	All bootstrapping methods build up the term structure from shorter maturities to longer ones.
	The basis swap valuation model can be found at https://finpricing.com/lib/IrSwap.html
	First you need to construct the base curve as guided at https://finpricing.com/lib/IrCurve.html
	Assuming that we have had all the yields of a 6-month LIBOR curve up to 4 years and now need to derive up to 5 years. 
	Let x be the yield at 5 years.
	Use an interpolation methd to get yields at 4.5 years as Ax
	Given the 5 year market basis swap spread, we can use a root-finding algorithm to solve the x that makes the value of the 5 year inception basis swap equal to zero.
	Now we get all yields or equivalent discount factors up to 5 years
	Repeat the above procedure till the longest basis swap maturity.
	There are two keys in yield curve construction: interpolation and optimization for root finding.

Interpolation

	Most popular interpolation algorithms in curve bootstrapping are linear, log-linear and cubic spline.
	The selected interpolation rule can be applied to either zero rates or discount factors.
	Some critics argue that some of these simple interpolations cannot generate smooth forward rates and the others may be able to produce smooth forward rates but fail to match the market quotes.		
	Also they cannot guarantee the continuity and positivity of forward rates.
	The monotone convex interpolation is more rigorous. It meets the following essential criteria:
	Replicate the quotes of all input underlying instruments.
	Guarantee the positivity of the implied forward rates
	Produce smooth forward curves.
	Although the monotone convex interpolation rule sounds almost perfectly, it is not very popular with practitioners.

Optimization

	As described above, the bootstrapping process needs to solve a yield using a root finding algorithm. 
	In other words, it needs an optimization solution to match the prices of curve-generated instruments to their market quotes.
	FinPricing employs the Levenberg-Marquardt algorithm for root finding, which is very common in curve construction.
	Another popular algorithm is the Excel Solver, especially in Excel application.


You can find more details at
https://finpricing.com/lib/IrCurveIntroduction.html

