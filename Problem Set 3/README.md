# Econ 3916 Problem Set 3: The Number Nobody Can Reproduce

## The question
A dashboard reported average revenue per West-region customer as $1,240, and Finance got $812 from the same warehouse. I worked out which number is right, why they differ, and whether the West's higher revenue was caused by a new account-management programme.

## What I did
- Simulated 3,000 customers (2,097 in the West) and 20,584 orders with DuckDB, with revenue rising with the number of orders.
- Reproduced both numbers: $814.43 from customers alone, and $1,250.67 after joining orders.
- Wrote a check that warns when a join changes the row count.
- Set 15% of regions to missing and showed how a WHERE filter silently drops those customers.
- Used a window function to put each customer's revenue next to their region's average without losing any rows.
- Simulated the programme with a known true effect, and compared estimates with and without controls.
- Used AI to write a SQL reviewer, then tested what it caught and what it missed.

## Key findings
- **$814 is the right number.** The question is about the average customer, so each customer should count once. The $1,240 query joined orders first, which turned 2,097 customers into 14,191 rows. Customers who order more also spend more, so they got counted many times and pulled the average up.
- **A filter on region silently dropped 401 customers.** `WHERE region != 'West'` returned 777 customers instead of 1,178, because SQL drops rows where the region is missing. Every regional comparison that does this quietly leaves those customers out.
- **The programme works much less than the regional gap suggests.** With a true effect of $50 per customer, comparing the West with other regions said $374, because bigger accounts were more likely to be in the West. Controlling for account size gave $47. Adding support tickets as a control made it -$194, because the programme itself changes how many tickets people file.
- **The AI's reviewer caught the $1,240, but only by luck.** It flagged the fan-out as the query was written, but missed the same bad join when the ON line was written the other way round, gave false alarms on two correct queries, and crashed on a query using WITH.

## What I'd recommend
Check that every join keeps one row per customer before averaging per customer, make queries keep customers with a missing region, and have a second analyst rerun any number before it goes on a slide.

## How to run it
Open `Econ_3916_PS3.ipynb` in Google Colab and run Runtime › Restart and run all. It installs DuckDB and simulates all its own data with seed 42, so nothing needs to be downloaded.
