## Step 3 - Daily Prices

1. How many total records do we have in the trading.prices table?

    SELECT COUNT(*) FROM trading.prices;

    | count |
    | ----- |
    | 3404  |

1. How many records are there per ticker value?

    SELECT ticker, COUNT(*)
    FROM trading.prices
    GROUP BY 1;

    | ticker | count |
    | ------ | ----- |
    | BTC    | 1702  |
    | ETH    | 1702  |

1. What is the minimum and maximum market_date values?

    SELECT MIN(market_date), MAX(market_date)
    FROM trading.prices;

    | min                      | max                      |
    | ------------------------ | ------------------------ |
    | 2017-01-01T00:00:00.000Z | 2021-08-29T00:00:00.000Z |

1. Are there differences in the minimum and maximum market_date values for each ticker?

    SELECT ticker, MIN(market_date), MAX(market_date)
    FROM trading.prices
    GROUP BY 1;

| ticker | min                      | max                      |
    | ------ | ------------------------ | ------------------------ |
    | BTC    | 2017-01-01T00:00:00.000Z | 2021-08-29T00:00:00.000Z |
    | ETH    | 2017-01-01T00:00:00.000Z | 2021-08-29T00:00:00.000Z |

1. What is the average of the price column for Bitcoin records during the year 2020?

    SELECT AVG(price)
    FROM trading.prices
    WHERE ticker = 'BTC'
    	AND market_date BETWEEN '2020-01-01' AND '2020-12-31';

    | avg                |
    | ------------------ |
    | 11111.631147540982 |


1. What is the monthly average of the price column for Ethereum in 2020? Sort the output in chronological order and also round the average price value to 2 decimal places

    SELECT
      DATE_TRUNC('MON', market_date) AS month_start,
      
      ROUND(AVG(price)::NUMERIC, 2) AS average_eth_price
    FROM trading.prices
    WHERE EXTRACT(YEAR FROM market_date) = 2020
    GROUP BY month_start
    ORDER BY month_start;

| month_start              | average_eth_price |
| ------------------------ | ----------------- |
| 2020-01-01T00:00:00.000Z | 4267.73           |
| 2020-02-01T00:00:00.000Z | 4937.66           |
| 2020-03-01T00:00:00.000Z | 3511.64           |
| 2020-04-01T00:00:00.000Z | 3691.16           |
| 2020-05-01T00:00:00.000Z | 4730.50           |
| 2020-06-01T00:00:00.000Z | 4858.88           |
| 2020-07-01T00:00:00.000Z | 4925.83           |
| 2020-08-01T00:00:00.000Z | 6020.07           |
| 2020-09-01T00:00:00.000Z | 5505.55           |
| 2020-10-01T00:00:00.000Z | 6132.07           |
| 2020-11-01T00:00:00.000Z | 8573.75           |
| 2020-12-01T00:00:00.000Z | 11302.20          |


1. Are there any duplicate market_date values for any ticker value in our table?

    SELECT 
    	ticker, 
    	COUNT(market_date) AS total_count, 
    	COUNT( DISTINCT market_date) AS unique_count
    FROM trading.prices
    GROUP BY 1;

| ticker | total_count | unique_count |
| ------ | ----------- | ------------ |
| BTC    | 1702        | 1702         |
| ETH    | 1702        | 1702         |

1. How many days from the trading.prices table exist where the high price of Bitcoin is over $30,000?

    SELECT 
    	COUNT(DISTINCT market_date)
    FROM trading.prices
    WHERE ticker = 'BTC' 
    	AND high > '30000';

    | count |
    | ----- |
    | 240   |

1. How many "breakout" days were there in 2020 where the price column is greater than the open column for each ticker?

    SELECT
      ticker,
      SUM(CASE WHEN price > open THEN 1 ELSE 0 END) AS breakout_days
    FROM trading.prices
    WHERE DATE_TRUNC('YEAR', market_date) = '2020-01-01'
    GROUP BY ticker;

| ticker | breakout_days |
| ------ | ------------- |
| BTC    | 207           |
| ETH    | 200           |

1. How many "non_breakout" days were there in 2020 where the price column is less than the open column for each ticker?

    SELECT
      ticker,
      SUM(CASE WHEN price < open THEN 1 ELSE 0 END) AS non_breakout_days
    FROM trading.prices
    
    WHERE market_date >= '2020-01-01' AND market_date <= '2020-12-31'
    GROUP BY ticker;

    | ticker | non_breakout_days |
    | ------ | ----------------- |
    | BTC    | 159               |
    | ETH    | 166               |

1. What percentage of days in 2020 were breakout days vs non-breakout days? Round the percentages to 2 decimal places

    SELECT
      ticker,
      ROUND(
        SUM(CASE WHEN price > open THEN 1 ELSE 0 END)
          / COUNT(*)::NUMERIC,
        2
      ) AS breakout_percentage,
      ROUND(
        SUM(CASE WHEN price < open THEN 1 ELSE 0 END)
          / COUNT(*)::NUMERIC,
        2
      ) AS non_breakout_percentage
    FROM trading.prices
    WHERE market_date >= '2020-01-01' AND market_date <= '2020-12-31'
    GROUP BY ticker;

| ticker | breakout_percentage | non_breakout_percentage |
| ------ | ------------------- | ----------------------- |
| BTC    | 0.57                | 0.43                    |
| ETH    | 0.55                | 0.45                    |

