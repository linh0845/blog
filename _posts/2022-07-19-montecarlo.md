---
title:  "Monte Carlo Simulation For A New Product"
mathjax: true
layout: post
categories: media
---

![Swiss Alps](https://user-images.githubusercontent.com/4943215/55412536-edbba180-5567-11e9-9c70-6d33bca3f8ed.jpg)

## Monte Carlo Simulation

Monte Carlo Simulation is common method in financial analysis. It is based on probabilistic technique to estimate the outcome in uncertain condition. More reference as below:

[Published in Toward Data Science](https://towardsdatascience.com/monte-carlo-simulation-a-practical-guide-85da45597f0e/) by Robert Kwiatkowski

[Published in ScienceDirect](https://www.sciencedirect.com/topics/economics-econometrics-and-finance/monte-carlo-simulation/) by multiple authors


## Define Problem

ABC Company has a new-product development decision which involves a lot of uncertainty. The company is interested in evaluating the risk associated with the project and need to answer 3 questions as follows:

1. What is the risk that the net present value (NPV) over the five years will not be positive?
2. What are the chances that the product will show a positive cumulative net profit in the third year?
3. In the fifth year, what minimum amount of cumulative profit are we likely to achieve with a probability of at least 90%?

Suppose that the Company has identified the following uncertain variables in the model and the distributions and parameters that describe them, as follows:

a. Market size: normal with mean of 2,000,000 units and standard deviation of 400,000 units. 
b. R&D costs: uniform between $600,000,000 and $800,000,000. 
c. Clinical trial costs: normal with mean of $150,000,000 and standard deviation $30,000,000. 
d. Annual market growth factor: triangular with minimum = 2%, maximum 6%, and most likely = 3%. 
e. Annual market share growth rate: triangular with minimum = 15%, maximum = 25%, and most likely = 20%. 


## Code

I run the folowing code in Python.

{% highlight c %}

import numpy as np
import pandas as pd
import statistics
import random
import matplotlib.pyplot as plt
%matplotlib inline
discount_rate = 0.09
unit_sales = 130
unit_cost = 40
unit_margin = unit_sales - unit_cost
market_size_mean = 2000000
market_size_sd = 40000
clinical_trial_cost_mean = 150000000
clinical_trial_cost_sd = 30000000
rd_cost_lower = 600000000
rd_cost_upper = 800000000
annual_mk_growth_min = .02
annual_mk_growth_mode = .03
annual_mk_growth_max = .06  
share_growth_min = .15
share_growth_mode = .2
share_growth_max = .25
iteration = 1000
project_year = 5
def project ():
    outputs = []
    for i in range(iteration):
        rd_cost = np.random.uniform(rd_cost_lower,rd_cost_upper)
        clinical_trial_cost = np.random.normal(clinical_trial_cost_mean,clinical_trial_cost_sd)
        project_cost = round((rd_cost + clinical_trial_cost)/1000000)
        share = .08
        mksize = np.random.normal(market_size_mean,market_size_sd)
        share_growth = list(np.random.triangular(share_growth_min,share_growth_mode,share_growth_max,project_year))
        annual_mk_growth = list(np.random.triangular(annual_mk_growth_min,annual_mk_growth_mode,annual_mk_growth_max,project_year))
        yearly_cum_profit = []
        cumulated_profit = 0 - project_cost
        npv_profit = 0
        year = 0
        while year < project_year:
            profit = round((share * mksize *  unit_margin * 12)/1000000)
            cumulated_profit = cumulated_profit + profit
            yearly_cum_profit.append(cumulated_profit)
            npv_profit = npv_profit + round(profit/(1+discount_rate)**(year+1))
            share = share * (1+share_growth[year])
            mksize = mksize * (1+ annual_mk_growth[year])
            year += 1
        npv_project = round((npv_profit - project_cost))
        yearly_cum_profit.append(npv_project)
        outputs.append(yearly_cum_profit)
    return outputs
    
{% endhighlight %}

## Result

With the `jekyll-gist` plugin, which is preinstalled on Github Pages, you can embed gists simply by using the `gist` command:

<script src="https://gist.github.com/5555251.js?file=gist.md"></script>

## Images

Upload an image to the *assets* folder and embed it with `![title](/assets/name.jpg))`. Keep in mind that the path needs to be adjusted if Jekyll is run inside a subfolder.

A wrapper `div` with the class `large` can be used to increase the width of an image or iframe.

![Flower](https://user-images.githubusercontent.com/4943215/55412447-bcdb6c80-5567-11e9-8d12-b1e35fd5e50c.jpg)

[Flower](https://unsplash.com/photos/iGrsa9rL11o) by Tj Holowaychuk

## Embedded content

You can also embed a lot of stuff, for example from YouTube, using the `embed.html` include.

{% include embed.html url="https://www.youtube.com/embed/_C0A5zX-iqM" %}
