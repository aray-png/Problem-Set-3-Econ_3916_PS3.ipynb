# Abigail Ray || Problem Set 3: The Number Nobody Can Reproduce

**ECON 3916 · Applied Data Analytics · Module 2: Causal Thinking & SQL**

Two analysts reported different average revenues for the same West-region customers: $824 and $1,257. This notebook rebuilds the warehouse in DuckDB, finds why the numbers differ, checks what silently disappears when a filter meets missing data, and tests whether the West account-management programme actually works. It closes with an AI-built query reviewer that catches these mistakes before they reach a slide.

## Memo to the Director

**Re:** West revenue: which number is right, and does the programme work?

**The right number is $824**, the average revenue of a West customer. The $1,257 came from the same customers after joining the orders table, which repeats each customer once per order: 2,000 customers became 14,004 rows. Our biggest accounts also place the most orders, so the repeats pushed the average up about 50%. The query quietly switched from averaging customers to averaging orders.

**The programme works, but its effect is small.** West customers earn about $205 more, but they had much larger accounts before it started. Comparing accounts of similar size, the programme adds about $27, close to the $25 built into our test data. Controlling for support tickets made the answer worse (−$3): both the programme and higher revenue generate tickets, so holding tickets fixed compares weaker West accounts with stronger ones elsewhere.

**Before a number goes on a slide, I recommend:**

1. **State the unit.** Each metric names what one row is (customer or order). Our new query reviewer flags averages taken after a join has duplicated customers; it would have stopped the $1,257.
2. **Count the unknowns.** 457 customers (15%) have no region, and "not West" filters silently drop them. Use NULL-safe filters and report what was excluded.
3. **Adjust only for what existed before the programme**, never its consequences.
4. **Rerun before publishing.** A second analyst reproduces the figure from the saved query, starting from a clean run.

## Running the notebook

Open it in Google Colab and choose **Runtime › Restart session and run all**. Every random draw is seeded, so a single top-to-bottom run reproduces each number above. Dependencies: `numpy`, `pandas`, `scipy`, `statsmodels`, `matplotlib` and `duckdb` (installed by the first code cells).
