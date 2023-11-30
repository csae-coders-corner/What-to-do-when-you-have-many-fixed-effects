# What to Do When You Have Many Fixed Effects
Using **regress** to estimate models with a large number of fixed effects runs into Stata memory problems. Plus, the regression takes a long time to execute. Here I outline an alternative that speeds up your regressions.

Let’s say we want to regress average village consumption (y) on a set of village controls (x1 x2 x3), and include district fixed effects. This is easy to implement with 20 districts: 

**regress y x1 x2 x3 i.district, vce(robust)**

But the estimation becomes time consuming with 20,000 districts. Using **reghdfe** makes this estimation faster. 

**reghdfe** allows for two levels of fixed effects, interactions between fixed effects, and multi-way clustering. It is a generalisation of the commands **areg** and **xtreg** – so if you’ve been using these, switch to reghdfe to gain speed and increased functionality. **reghdfe** is faster than r**egress, areg** and **xtreg** even with only one fixed effect, and orders of magnitude faster with multiple fixed effects. 

Its syntax:

**reghdfe depvar indepvars, absorb(i.fixed-effect1 i.fixed-effect2) vce(type)**  


The command can generate different types of standard errors through the vce option: ols (default), robust, clustered, boostrap and jackknife. It also allows multi-way clustering. 

In terms of our example with 20000 district fixed effects, you could estimate this using the following: 

**reghdfe y x1 x2 x3, absorb(i.district) vce(robust)**

Now suppose you want to include both district and year fixed effects. reghdfe allows you to include both:

**reghdfe y x1 x2 x3, absorb(i.district i.year) vce(robust)**


You can also integrate the functionality of the ivreg2 package with the ability to handle large numbers of fixed effects and two-way clustering using the command ivreghdfe. The syntax is:

**ivreghdfe depvar exogenousvars (endogenousvars=instruments), absorb(i.fixed-effect1 i.fixed-effect2) vce(type)**



**AUTHOR: Karan Nagpal, Economist, IDinsight**

**21 January 2019**


