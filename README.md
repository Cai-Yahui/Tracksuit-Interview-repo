# Tracksuit Survey Allocation Optimisation

**Author:** Yahui Cai   

## Overview

This project demonstrates a data science approach to **optimising survey allocation** for Tracksuit, a survey-based insights platform.  

The main goal is to **minimise the total number of respondents surveyed** while ensuring that each category receives at least 200 qualified respondents per month, respecting the **8-minute per-respondent survey limit**, and maintaining a **nationally representative demographic distribution**.

The project compares multiple approaches, including:

1. **Greedy Packing** – A simple heuristic that prioritises categories by expected survey time.  
2. **Demand Weighted Packing** – An improved heuristic that also accounts for categories that require many respondents due to low incidence rates.  
3. **Linear Programming (LP) Optimisation** – A theoretical lower bound on the number of respondents required.  
4. **Monte Carlo Simulation** – Validates all strategies under stochastic qualification probabilities.

---

## Project Structure

```
tracksuit-survey-optimisation/
│
├── README.md                         <- This file
├── data/
│   └── fake_category_data.csv        <- Example survey category data
├── report/
│   ├── survey_optimisation.Rmd       <- R Markdown source
│   └── survey_optimisation.html      <- Rendered HTML report
```

## How to Use

1. Open `report/survey_optimisation.Rmd` in **RStudio**.  
2. Install required R packages:

```r
install.packages(c("tidyverse", "lpSolve"))
```
3. Ensure the data file path is correct (data/fake_category_data.csv).
4. Click Knit → Knit to HTML to generate the final report.
5. Open report/survey_optimisation.html to view the report with all results, simulations, and visualisations.

## Methodology

1. Greedy Packing
+ A simple heuristic that packs categories based on expected survey time.
+ Ensures total survey time ≤ 480 seconds per respondent.

2. Demand Weighted Packing

+ Improves the heuristic by also considering categories that require more respondents due to low incidence rates.
+ Produces a more efficient allocation while respecting time constraints.

3. Linear Programming (LP) Optimisation

+ Computes a theoretical lower bound of respondents needed.
+ Ignores stochastic qualification and represents the ideal scenario.

4. Monte Carlo Simulation

+ Validates all strategies under stochastic respondent qualification.
+ Confirms practical number of respondents required to achieve quotas while keeping survey length below 480 seconds.

## Key Insights

+ LP provides a benchmark, but simulation shows that real-world randomness requires significantly more respondents.
+ Weighted heuristic slightly outperforms Greedy, approaching the LP lower bound while remaining practical for production.
+ Simulation is critical because survey qualification is a stochastic stopping process, making deterministic solutions insufficient.
+ The approach is extensible, including clustering categories with similar audiences or adaptive allocation during the survey period.

## Result

| Method         | Mean Respondents Required |
| -------------- | ------------------------- |
| Greedy         | ~18,760                   |
| Weighted       | ~18,783                   |
| LP lower bound | ~2,092                    |

+ The LP solution represents an ideal, theoretical minimum.
+ Simulation-based heuristics reflect realistic stochastic performance.

## Visualisation

+ The report includes histograms of respondents required for each strategy.
+ Visual comparisons demonstrate efficiency gains from the weighted heuristic.

## Real-World Considerations

+ Incidence Uncertainty: Estimated incidence rates may differ from reality; simulations help quantify this risk.
+ Adaptive Allocation: Future improvements could dynamically adjust survey assignments during the month.
+ Audience Clustering: Combining categories with similar audiences may improve efficiency but must be validated to avoid demographic bias.

## Conclusion

+ Heuristic and LP approaches provide complementary insights: LP = theoretical benchmark, Weighted heuristic = scalable, practical solution.
+ Monte Carlo simulation validates stochastic outcomes and ensures survey quality and quota fulfilment.

