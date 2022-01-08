## Step 5 - Let the Data Analysis Begin!
Even though we have been exploring our datasets and exploring a few of the basic SQL concepts required for data analysis - we have yet to combine our SQL queries into a single focused analytical process to solve a larger problem. This is our opportunity to try this now!

Let's say that we wish to analyse our overall portfolio performance and also each member's performance based off all the data we have in our 3 tables.

### Analyse the Ranges
Firstly - let's see what is the range of data we have to play with!

1. What is the earliest and latest date of transactions for all members?

    SELECT
    	MAX(txn_date) AS max_date,
        MIN(txn_date) AS min_date
    FROM trading.transactions;

    | max_date                 | min_date                 |
    | ------------------------ | ------------------------ |
    | 2021-08-27T00:00:00.000Z | 2017-01-01T00:00:00.000Z |

1. What is the range of market_date values available in the prices data?

    SELECT
    	MAX(market_date) AS max_date,
        MIN(market_date) AS min_date
    FROM trading.prices;

    | max_date                 | min_date                 |
    | ------------------------ | ------------------------ |
    | 2021-08-29T00:00:00.000Z | 2017-01-01T00:00:00.000Z |

## Joining our datasets
Now that we know our date ranges are from January 2017 through to almost the end of August 2021 for both our prices and transactions datasets - we can now get started on joining these two tables together!

Let's make use of our ERD shown above to combine the trading.transactions table and the trading.members table to answer a few simple questions about our mentors!

1. Which top 3 mentors have the most Bitcoin quantity as of the 29th of August?

    SELECT
    	members.first_name,
        SUM(CASE 
            WHEN txn_type = 'SELL' THEN -transactions.quantity 
            WHEN txn_type = 'BUY' THEN transactions.quantity
            END) AS balance
    FROM trading.transactions
    INNER JOIN trading.members
    ON transactions.member_id = members.member_id
    WHERE ticker = 'BTC'
    GROUP BY members.first_name
    ORDER BY balance DESC
    LIMIT 3;

    | first_name | balance            |
    | ---------- | ------------------ |
    | Nandita    | 4160.219869506644  |
    | Leah       | 4046.0908966725606 |
    | Ayush      | 3945.198083260507  |

## Calculating Portfolio Value
Now let's combine all 3 tables together using only strictly INNER JOIN so we can utilise all of our datasets together.

1. What is total value of all Ethereum portfolios for each region at the end date of our analysis? Order the output by descending portfolio value
    WITH cte_latest_price AS (
      SELECT
        ticker,
        price
      FROM trading.prices
      WHERE ticker = 'ETH'
      AND market_date = '2021-08-29'
    )
    SELECT
      members.region,
      SUM(
        CASE
          WHEN transactions.txn_type = 'BUY'  THEN transactions.quantity
          WHEN transactions.txn_type = 'SELL' THEN -transactions.quantity
        END
      ) * cte_latest_price.price AS ethereum_value,
      AVG(
        CASE
          WHEN transactions.txn_type = 'BUY'  THEN transactions.quantity
          WHEN transactions.txn_type = 'SELL' THEN -transactions.quantity
        END
      ) * cte_latest_price.price AS avg_ethereum_value
    FROM trading.transactions
    INNER JOIN cte_latest_price
      ON transactions.ticker = cte_latest_price.ticker
    INNER JOIN trading.members
      ON transactions.member_id = members.member_id
    WHERE transactions.ticker = 'ETH'
    GROUP BY members.region, cte_latest_price.price
    ORDER BY avg_ethereum_value DESC;

    | region        | ethereum_value     | avg_ethereum_value |
    | ------------- | ------------------ | ------------------ |
    | Australia     | 40076021.09227076  | 10752.89001670801  |
    | United States | 50688412.27725327  | 10549.097248127631 |
    | Asia          | 5011670.977699018  | 8933.459853295932  |
    | India         | 6276426.4827863695 | 8036.397545181011  |
    | Africa        | 2183933.3382704277 | 3899.8809611971915 |


1. What is the average value of each Ethereum portfolio in each region? Sort this output in descending order

    WITH cte_latest_price AS (
      SELECT
        ticker,
        price
      FROM trading.prices
      WHERE ticker = 'ETH'
      AND market_date = '2021-08-29'
    ),
    cte_calculations AS (
    SELECT
      members.region,
      SUM(
        CASE
          WHEN transactions.txn_type = 'BUY'  THEN transactions.quantity
          WHEN transactions.txn_type = 'SELL' THEN -transactions.quantity
        END
      ) * cte_latest_price.price AS ethereum_value,
      COUNT(DISTINCT members.member_id) AS mentor_count
    FROM trading.transactions
    INNER JOIN cte_latest_price
      ON transactions.ticker = cte_latest_price.ticker
    INNER JOIN trading.members
      ON transactions.member_id = members.member_id
    WHERE transactions.ticker = 'ETH'
    GROUP BY members.region, cte_latest_price.price
    )
    
    SELECT
      *,
      ethereum_value / mentor_count AS avg_ethereum_value
    FROM cte_calculations
    ORDER BY avg_ethereum_value DESC;

    | region        | ethereum_value     | mentor_count | avg_ethereum_value |
    | ------------- | ------------------ | ------------ | ------------------ |
    | Australia     | 40076021.09227075  | 4            | 10019005.273067687 |
    | United States | 50688412.27725326  | 7            | 7241201.753893323  |
    | India         | 6276426.482786364  | 1            | 6276426.482786364  |
    | Asia          | 5011670.977699024  | 1            | 5011670.977699024  |
    | Africa        | 2183933.3382704277 | 1            | 2183933.3382704277 |

 