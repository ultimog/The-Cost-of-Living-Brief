# Problem Set 4: The Model That Fit Too Well

## The question
A store revenue model was handed over with the note "R-squared 0.94. Validated." Since then, it has been over-forecasting the biggest stores. I found out what "validated" meant, measured what the model can actually do, and built a version whose stated accuracy is honest.

## What I did
- Simulated 400 stores with a known truth: revenue that grows more slowly per square foot above 25k sq ft, errors that grow with store size, six irrelevant predictors, and one predictor that is nearly a copy of two others.
- Reproduced the high R-squared, then measured the model on stores it never saw.
- Diagnosed the residuals: non-constant variance and a missed bend in floor area.
- Measured the over-forecast on the biggest stores, and checked collinearity with variance inflation factors.
- Compared six models from simple to very flexible, chose one by cross-validation, and used nested cross-validation to report an honest error.
- Used AI to build a learning curve, and tested its conclusion against my own results.
- Reviewed the departing analyst's validation code without AI.

## Key findings
- The model's R-squared was 0.9294 on the stores it was fitted to and 0.9228 on held-out stores. Adding three columns of pure noise still raised R-squared, which is why it can't choose between models.
- Errors grow with store size (Breusch-Pagan p ≈ 3e-20), which makes classical standard errors too small. The straight-line model also misses the bend at 25k sq ft, which biases its predictions.
- The biggest 10% of stores were over-forecast by $58.2k a month on average, and 75% of them were over-forecast.
- sales_capacity had a VIF of 1,353: it damages the individual coefficients, not the predictions.
- Held-out error was lowest for the model with squared terms ($59.3k) and rose sharply with more flexibility, up to $207.2k.
- Cross-validation chose the squared-terms model, with an expected error of $58.0k a month (95% interval $35.2k to $80.9k). Nested cross-validation gave the same result, because the same model won every time.
- The AI's learning curve called the remaining error "likely noise." That was wrong: the true noise is about $39k, and better features had already cut the error well below the plain model's.

## What I'd recommend
Invest in better features, not more stores: model the bend directly, report error separately for the biggest stores, and check forecasts against actual revenue every month.

## How to run it
Open `Econ_3916_PS4.ipynb` in Google Colab and run Runtime › Restart and run all. It simulates all its own data with seed 42, so nothing needs to be downloaded.
