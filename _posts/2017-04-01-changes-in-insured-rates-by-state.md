---
layout: post
title: Visualizing Changes in Healthcare Insurance Coverage (2010-2015)
subtitle: Insured Rates Before and After the Affordable Care Act
bigimg: /img/insured-rates/aca2.jpg
published: true
---

The Affordable Care Act (ACA) is the name for the health care reform law which addresses health insurance coverage, costs, and preventive care. The ACA was signed into law on March 23, 2010.

** The Python code and data used for this post can be found <a href="https://nbviewer.jupyter.org/github/martyncisneros/visualizing_changes_insured_rates/blob/master/Healthcare%20Insurance%20Coverage.ipynb" target="_blank">here</a>.**

In this analysis, I looked at data compiled by the US Department of Health and Human Services and US Census Bureau. This dataset provides health insurance coverage data for each state and includes variables such as the uninsured rates before and after the ACA, estimates of individuals covered by employer and marketplace healthcare plans, and enrollment in Medicare and Medicaid programs.

The following questions were addressed:

-  How has the Affordable Care Act changed the rate of citizens with health insurance coverage?
-  Which states observed the greatest increase in their insured rate?
-  Did those states expand Medicaid program coverage and/or implement a health insurance marketplace?

The Affordable Care Act increased the rate of citizens with health insurance coverage by an average of <strong>5.43%</strong> across all states. The states with the greatest increase in their insured rate were <strong>Nevada, Oregon, California, Kentucky, New Mexico, and West Virginia</strong>.

![alt text][logo]

A central goal of the ACA was to reduce the number of uninsured by increasing access to affordable coverage options through Medicaid and the Health Insurance Marketplace. On June 28, 2012, the U.S. Supreme Court issued its decision regarding the constitutionality of the individual mandate and state-level Medicaid expansion mandate. The ruling made the Medicaid expansion optional for states. Roughly <strong>25%</strong> of states with Medicaid expansion had higher insured rate increases than all states without Medicaid expansion.

![alt text][logo2]

Since there is no deadline for states to implement the Medicaid expansion, future decisions by those 19 states without Medicaid expansion programs will contribute to the continued reduction of their uninsured rates.

![alt text][logo3]

[logo]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/insured-rates.png "Insured Rates Deltas by State"
[logo2]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/box-plot.png "Medicaid Expansion Box Plot"
[logo3]: https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/insured-rates/medicaid-expansion.png "Medicaid Expansion by State"
