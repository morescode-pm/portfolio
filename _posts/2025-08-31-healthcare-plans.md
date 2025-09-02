---
layout: post
tags: [python, tableau]
title: American Health Insurance - Graphed!
published: true
---

Health insurance; Open Enrollment. At least once a year, all Americans are offered a chance to sign up for health insurance through their employer, or through the healthcare marketplace.  

Monthly premiums, Deductibles, Out-of-pocket maximums, HSAs, FSAs, Co-insurance, Copay.. forcing everyone to _GAMBLE_ on what they think they might need - and try to do the math to understand what their best options are.  

It sucks.

This project aims to make it suck just a bit less.

[Link to Live Tableau Dashboard / Calculator Here][5]
<div class="full-width">
    <div id="vizWrapper">
        <tableau-viz
            id="tableauViz"
            src="https://public.tableau.com/views/heath_insurance_calculator/InsurancePlanComparison"
            toolbar="bottom"
            hide-tabs
            device="desktop">
        </tableau-viz>
    </div>
</div>

>`**Important Note**` : Every plan is different, so your monthly medical costs could be much lower or higher based on what your insurance covers. Some plans have copays, others don't, others still have expense limits - your situation is going to be unique and this content is created for general information purposes.  

> _This is not financial advice_. Seek advice from a qualified financial professional.

### Introduction
If you're like me: whether you're scrolling on the healthcare marketplace or reviewing a PDF of options from employer plans - before you've made it through page 4 you just want it to be over. It's a lot to manage - and something you REALLY don't want to get wrong. High stakes gambling with your health and your finances.  

So, I set out to use my skills to turn another PDF into something useful: A tableau dashboard about comparing Health Insurance options.
The goal is to visualize a comparison of plan annual costs VS. what you expect you might need to pay monthly - your Est. Monthly Medical Costs.  

**Est. Monthly Medical Costs**: If you estimate needing to spend $1200 per year, this is ($1200 / 12) $100.  


### Data Sources
The solution here is entirely algebra based. The variables make the plan and the equation doesn't change.

First some definitions:
- **Monthly Premium**: What you owe every month of the year to be enrolled in the plan.
- **Deductible**: How much you will pay (in addition to the premiums) before any benefits kick in.
- **Out-of-pocket Max (OOP)**: According the the Affodable Care Act, after hitting this amount you plan will cover 100% of covered services.
- **Coinsurance**: Your share of the costs of a covered service as a percentage. ex 20% of $100 you will owe $20.
- **Health Savings / Employer Benefit**: Some companies will contribute to a health savings account if you're enrolled in a high deductible plan.
- **CDHP**: Consumer Driven Healthcare Plan
- **HMO**: Health Maintenance Organization
- **In Network / Out of Network**: Doctors and hospitals have some choice to work with providers. If they do, they get the network negotiated costs & theoretically access to more patients from that network. It's a gentle form of negotiated threat.

Then some plan costs - we'll use some examples from the publicly accessible State Of Illinois - Deparment of Centra Management Services - Bureau of Benefits FY2026 documentation. [PDF Link Here][1]  
Generally speaking, government services are REALLY good.

For a salary range of $45,601-$60,700 (Assuming everything is In network):  
| Provider | Monthly Premium | Deductible | OOP Max | Coinsurance | Employer HSA Deposit|
|----------|-----------------|------------|---------|-------------|---------------------|
| [AETNA HMO][2] | $176      | $0         | $3000   | mostly copay but we'll use 15% for example       | $0                  |
| [Blue Advantage HMO][4] | $150   | $0   | $3000   | mostly copay but we'll use 10% for example | $0
| [AETNA CDHP][3] | $151     | $1650      | $3000   | 10%         | $550


Then since we're just going to solve for Y given a series of Xs - we can just quickly generate that as a series in python:
```python
import numpy as np
import pandas as pd

data = np.arange(0, 800 + 5, 5)
df = pd.DataFrame({'Est. Monthly Medical Costs': data})
df.to_csv('generated_data.csv', index=False)
```

### Health Plan Cost Calculations

Here are the key calculations for determining the overall cost of a health plan.

#### 1. Annual Medical Costs

This is a straightforward calculation based on the estimated monthly medical costs.
```SQL
-- Annual Medical Costs = 
[Est. Monthly Medical Costs] * 12
```

#### 2. Estimated Annual Plan Costs

This formula calculates the total estimated cost of the health plan, combining premiums, out-of-pocket expenses, and any employer contributions.

```SQL
-- Estimated Annual Plan Costs =
( [Plan 1 Monthly Premium] * 12 ) + 

MIN(
    -- Medical expenses before and after deductible
    IF [Annual Medical Costs] <= [Plan 1 Deductible] THEN [Annual Medical Costs]
    ELSE [Plan 1 Deductible] + ([Annual Medical Costs] - [Plan 1 Deductible]) * [Plan 1 Coinsurance] 
    END,

    -- Out-of-pocket maximum
    [Plan 1 OOP Max]
) - [Plan 1 ER HSA Deposit]
```

### Dashboard Example
1. Enter your plan comparisons
2. Enter you estimated monthly costs

Looking at the graph and how the plans compare, give your costs a +20% buffer to the right (more expensive) and pick a plan based on those expectations.

<img src="/assets/images/healthcare/1-plan_compare.png">

In this examples, if the monthly costs were anticipated at $100 - the Blue Advantage HMO plan would be the best option.  

In fact, with these examples, Blue Advantage is the right plan until about $2000/mo costs.  

However, you will not get any HSA unless you go wit AETNA CDHP - so if you're expecting minimal or no expenses (risky assumption), then maybe the CDHP would make sense.  

__Don't make this assumption.__


[1]: https://cms.illinois.gov/content/dam/soi/en/web/cms/benefits/stateemployee/documents/fy2026/26_SEGIP_ADA_041725.pdf

[2]: https://www.aetnastateofillinois.com/application/files/1317/4558/8088/State_HMO_2025.pdf

[3]: https://www.aetnastateofillinois.com/application/files/2317/4558/8087/SEGIP_CDHP_PPO_2025.pdf

[4]: https://cms.illinois.gov/content/dam/soi/en/web/cms/benefits/college/documents/fy2026/sbc/SBC_IL6800_B06803_0049_State%20of%20Illinois%20CIP%20_BAHMO_07-01-2025%20to%2006-30-2026_2012-07-01_v1.pdf

[5]: https://public.tableau.com/app/profile/paul.moresco/viz/heath_insurance_calculator/InsurancePlanComparison