# Problem Set 1: The Cost of Living Brief

## Objective

I built a price index for one specific household and compared it against four official CPI series, to test a client's claim that the headline CPI overstates what their workers actually pay.

## Methodology

- Constructed a seven category household basket by hand as a list of dictionaries, with a 2019 price, a 2024 price, and a share of monthly spending for each category. Weights sum to 1.0, checked in code.
- Wrote a `laspeyres()` function that computes a base weighted price index, and compared its result against the plain unweighted average of the same price changes.
- Pulled four series from FRED through its public CSV endpoint, which needs no API key: the headline CPI, rent of primary residence, food at home, and used cars.
- Rebased every series to 2019 = 100 so the official data and my household index sit on the same footing, then plotted them on one chart.
- Audited each series before using it: first observation, last observation, frequency, and the count of missing months on a complete monthly grid.
- Computed the common sample across all four series and recomputed the headline comparison twice, once on each series' own history and once restricted to the common sample.
- Pulled the unadjusted twin of the headline CPI and compared the two over the last five years, to see what seasonal adjustment actually removes.
- Used an AI generated weight sensitivity sweep to test how much the result depends on any single category's weight, then verified its output against my own figures.

## Key Findings

**The household index rose 24.6% between 2019 and 2024, against 22.7% for the headline CPI.** The client's instinct is directionally right, but the gap is about two percentage points over five years rather than the large divergence they suspected.

**Weighting matters, and it cuts against the intuitive answer here.** The unweighted average price change across the seven categories is 26.1%, higher than the weighted index at 24.6%. The unweighted figure treats an 8.7% rise in phone and internet as equally important to a 20% rise in rent, even though rent is 47% of this household's spending and phone is 4%.

**The common sample discards most of the available history.** Rent data begins in 1981, while the headline CPI reaches back to 1947. Restricting to the overlap leaves 548 months and throws away 408 months of the longest series. Average annual inflation for the headline CPI falls from 3.51% on its full history to 2.99% on the common sample, so the choice of window is not neutral.

**Seasonal adjustment is a smaller effect than expected at this horizon.** The adjusted and unadjusted CPI series differ by under one index point over the last five years, because the seasonal pattern largely cancels in a 2019 to 2024 comparison.

**The result is most sensitive to the smallest category, not the largest.** A weight sensitivity sweep shows phone and internet moves the index by 0.166 points per percentage point of weight, more than any other category, because its price barely rose while everything else did. Rent carries 47% of the weight but ranks third, because its price moved roughly in line with the rest of the basket. Sensitivity comes from how far a category's price move sits from the basket average, not from how much it is weighted.

## Limitations

The basket is a constructed illustration of one household rather than survey data, so the spending shares are my own estimates. The FRED series are national averages standing in for local prices. A Laspeyres index holds 2019 quantities fixed, so it overstates the increase for any category the household substituted away from as prices rose.
