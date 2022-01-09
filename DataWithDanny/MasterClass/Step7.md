## Step 7 - Answering Data Questions

1. What is the total portfolio value for each mentor at the end of 2020?

SELECT
  base.first_name,
  ROUND(
    SUM(base.cumulative_quantity * prices.price),
    2
  ) AS portfolio_value
FROM temp_cumulative_portfolio_base AS base
INNER JOIN trading.prices
  ON base.ticker = prices.ticker
  AND base.year_end = prices.market_date
WHERE base.year_end = '2020-12-31'
GROUP BY base.first_name
ORDER BY portfolio_value DESC;

1. What is the total portfolio value for each region at the end of 2019?

SELECT
  base.region,
  ROUND(
    SUM(base.cumulative_quantity * prices.price),
    2
  ) AS portfolio_value
FROM temp_cumulative_portfolio_base AS base
INNER JOIN trading.prices
  ON base.ticker = prices.ticker
  AND base.year_end = prices.market_date
WHERE base.year_end = '2019-12-31'
GROUP BY base.region
ORDER BY portfolio_value DESC;

1. What percentage of regional portfolio values does each mentor contribute at the end of 2018?

WITH cte_mentor_portfolio AS (
  SELECT
    base.region,
    base.first_name,
    ROUND(
      SUM(base.cumulative_quantity * prices.price),
      2
    ) AS portfolio_value
  FROM temp_cumulative_portfolio_base AS base
  INNER JOIN trading.prices
    ON base.ticker = prices.ticker
    AND base.year_end = prices.market_date
  WHERE base.year_end = '2018-12-31'
  GROUP BY base.first_name, base.region
),
cte_region_portfolio AS (
SELECT
  region,
  first_name,
  portfolio_value,
  SUM(portfolio_value) OVER (PARTITION BY region) AS region_total
FROM cte_mentor_portfolio
)
-- final output
SELECT
  region,
  first_name,
  ROUND(100 * portfolio_value / region_total, 2) AS contribution_percentage
FROM cte_region_portfolio
ORDER BY region_total DESC, contribution_percentage DESC;

1. Does this region contribution percentage change when we look across both Bitcoin and Ethereum portfolios independently at the end of 2017?

WITH cte_mentor_portfolio AS (
  SELECT
    base.region,
    base.first_name,
    base.ticker,
    base.cumulative_quantity * prices.price AS portfolio_value
  FROM temp_cumulative_portfolio_base AS base
  INNER JOIN trading.prices
    ON base.ticker = prices.ticker
    AND base.year_end = prices.market_date
  WHERE base.year_end = '2017-12-31'
),
cte_region_portfolio AS (
SELECT
  region,
  first_name,
  ticker,
  portfolio_value,
  SUM(portfolio_value) OVER (
    PARTITION BY region, ticker
  ) AS region_total
FROM cte_mentor_portfolio
)
-- final output
SELECT
  region,
  first_name,
  ticker,
  ROUND(100 * portfolio_value / region_total, 2) AS contribution_percentage
FROM cte_region_portfolio
ORDER BY ticker, region, contribution_percentage DESC;