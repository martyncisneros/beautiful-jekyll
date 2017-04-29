---
layout: post
title: Visualizing Changes in US Healthcare Insurance Coverage (Part I)
subtitle: Insured Rates Before and After the Affordable Care Act
bigimg: /img/insured-rates/aca1.png
published: true
---

The Affordable Care Act (ACA) is the name for the 2010 health care reform law which addressed health insurance coverage, costs, and preventative care. On March 24th 2017, House Republican leaders pulled legislation to repeal the Affordable Care Act. While a rewrite to the law appears to be on hold, the debate continues. In light of these recent developments, I wanted to dig into the ACA's impact on US health care thus far.

In part 1 of this analysis, I explore data compiled by the _US Department of Health and Human Services and US Census Bureau_. This dataset includes uninsured rates before and after the ACA, estimates of individuals covered by employer and marketplace healthcare plans, and enrollment in Medicare and Medicaid programs.

The following questions were addressed:

-  How has the Affordable Care Act changed the rate of citizens with health insurance coverage?
-  Which states observed the greatest increase in their insured rate?
-  Did those states expand Medicaid program coverage and/or implement a health insurance marketplace?

** The Python code and data used for this post can be found <a href="https://nbviewer.jupyter.org/github/martyncisneros/visualizing_changes_insured_rates/blob/master/Healthcare%20Insurance%20Coverage.ipynb" target="_blank">here</a>.**

The Affordable Care Act increased the rate of citizens with health insurance coverage by an <strong>average of 5.43% across all states</strong>. The states with the greatest increase in their insured rate were <strong>Nevada, Oregon, California, Kentucky, New Mexico, and West Virginia</strong>.

![alt text][logo]

A central goal of the ACA was to reduce the number of uninsured by increasing access to affordable coverage options through Medicaid expansion and the Health Insurance Marketplace. States with the greatest increase in their insured rate also expanded Medicaid programs. Roughly <strong>25%</strong> of states with Medicaid expansion had higher insured rate increases than <strong>all</strong> states without Medicaid expansion.

![alt text][logo2]

Medicaid Enrollment Change Per 100,000 People (2013-2016)
![alt text][logo6]

Refundable tax credits were designed to help eligible individuals and families with low or moderate income afford health insurance purchased through the Health Insurance Marketplace, beginning in 2014. States that did not expand Medicaid instead show greater numbers of individuals covered through the Health Insurance Marketplace per capita and higher tax credits per capita. 

Marketplace Tax Credits Per 100,000 People (2016)
![alt text][logo4]

Number of Individuals Covered by the Insurance Marketplace Per 100,000 People (2016)
![alt text][logo5]

On June 28, 2012, the U.S. Supreme Court issued its decision making Medicaid expansion optional for states. There is no deadline for states to implement the Medicaid expansion. 

![alt text][logo3]

Future decisions by those 19 states without Medicaid expansion programs and continued funding of premium tax credits will contribute to the continued reduction of their uninsured rates.

In part 2 of this analysis, I will look at an dataset originally prepared and released by the _Centers for Medicare & Medicaid Services (CMS)_. This dataset includes extensive information on health and dental plans offered to individuals and small businesses through the Health Insurance Marketplace. I will look to shed some light into how the ACA has effective healthcare costs. 

[logo]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/insured-rates.png "Insured Rates Deltas by State"
[logo2]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/box-plot.png "Medicaid Expansion Box Plot"
[logo3]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/medicaid_expansion.png "Medicaid Expansion by State"
[logo4]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/tax-credits1.png "Tax Credits Per Capita by State"
[logo5]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/marketplace2.png "Marketplace Insured Per Capita by State"
[logo6]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/medicaid-expansion-scatter.png "Medicaid Expansion and Insured Rate"
