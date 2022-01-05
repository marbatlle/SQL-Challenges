# SQL Masterclass for Data Scientists
Free PostgreSQL course followed from [DataWithDanny](https://github.com/DataWithDanny/sql-masterclass), focused on a Cryptocurrency Trading Case study based on a single schema called [trading](https://www.db-fiddle.com/f/cnLCK4ChsNfr5ViG6vzePg/7)

## Step 1 - Introduction
There are 3 data tables available to us in this schema which we can use to run our SQL queries with:

1. Members
1. Prices
1. Transactions

## Step 2 - Exploring The Members Data

1. Show only the top 5 rows from the trading.members table

    SELECT * FROM trading.members LIMIT 5;

    | member_id | first_name | region        |
    | --------- | ---------- | ------------- |
    | c4ca42    | Danny      | Australia     |
    | c81e72    | Vipul      | United States |
    | eccbc8    | Charlie    | United States |
    | a87ff6    | Nandita    | United States |
    | e4da3b    | Rowan      | United States |

1. Sort all the rows in the table by first_name in alphabetical order and show the top 3 rows

    SELECT * FROM trading.members 
    ORDER BY first_name ASC
    LIMIT 3;

    | member_id | first_name | region        |
    | --------- | ---------- | ------------- |
    | c9f0f8    | Abe        | United States |
    | 8f14e4    | Alex       | United States |
    | 167909    | Ayush      | United States |

1. Which records from trading.members are from the United States region?

    SELECT * FROM trading.members 
    WHERE region = 'United States';

    | member_id | first_name | region        |
    | --------- | ---------- | ------------- |
    | c81e72    | Vipul      | United States |
    | eccbc8    | Charlie    | United States |
    | a87ff6    | Nandita    | United States |
    | e4da3b    | Rowan      | United States |
    | 167909    | Ayush      | United States |
    | 8f14e4    | Alex       | United States |
    | c9f0f8    | Abe        | United States |


1. Select only the member_id and first_name columns for members who are not from Australia

    SELECT member_id, region FROM trading.members 
    WHERE region != 'Australia';

    | member_id | region        |
    | --------- | ------------- |
    | c81e72    | United States |
    | eccbc8    | United States |
    | a87ff6    | United States |
    | e4da3b    | United States |
    | 167909    | United States |
    | 8f14e4    | United States |
    | c9f0f8    | United States |
    | d3d944    | Africa        |
    | 6512bd    | India         |
    | c20ad4    | Asia          |

1. Return the unique region values from the trading.members table and sort the output by reverse alphabetical order

    SELECT DISTINCT region FROM trading.members
    ORDER BY region DESC;

    | region        |
    | ------------- |
    | United States |
    | India         |
    | Australia     |
    | Asia          |
    | Africa        |

1. How many mentors are there from Australia or the United States?

    SELECT COUNT (*) FROM trading.members
    WHERE region = 'Australia'
    OR region = 'United States';

    | count |
    | ----- |
    | 11    |

1. How many mentors are not from Australia or the United States?

    SELECT COUNT (*) FROM trading.members
    WHERE region != 'Australia'
    AND region != 'United States';

    | count |
    | ----- |
    | 3     |

1. How many mentors are there per region? Sort the output by regions with the most mentors to the least

    SELECT region, COUNT(*) 
    FROM trading.members
    GROUP BY 1
    ORDER BY 2 DESC;

    | region        | count |
    | ------------- | ----- |
    | United States | 7     |
    | Australia     | 4     |
    | India         | 1     |
    | Africa        | 1     |
    | Asia          | 1     |

1. How many US mentors and non US mentors are there?

    SELECT
    	CASE
        WHEN region != 'United States' THEN 'non-US'
        ELSE region
        END, 
        COUNT(*)
    FROM trading.members
    GROUP BY 1
    ORDER BY 2 DESC;

    | region        | count |
    | ------------- | ----- |
    | United States | 7     |
    | non-US        | 7     |

1. How many mentors have a first name starting with a letter before 'E'?

    SELECT
      COUNT(*) AS mentor_count
    FROM trading.members
    WHERE LEFT(first_name, 1) < 'E';

    | mentor_count |
    | ------------ |
    | 6            |

## Step 3 - Daily Prices

1. How many total records do we have in the trading.prices table?
1. How many records are there per ticker value?
1. What is the minimum and maximum market_date values?
1. Are there differences in the minimum and maximum market_date values for each ticker?
1. What is the average of the price column for Bitcoin records during the year 2020?
1. What is the monthly average of the price column for Ethereum in 2020? Sort the output in chronological order and also round the average price value to 2 decimal places
1. Are there any duplicate market_date values for any ticker value in our table?
1. How many days from the trading.prices table exist where the high price of Bitcoin is over $30,000?
1. How many "breakout" days were there in 2020 where the price column is greater than the open column for each ticker?
1. How many "non_breakout" days were there in 2020 where the price column is less than the open column for each ticker?
1. What percentage of days in 2020 were breakout days vs non-breakout days? Round the percentages to 2 decimal places



## Step 4

## Step 5

## Step 6

## Step 7

## Step 8

## Step 9

## Step 10

## Step 11

## Step 12

## Step 13

## The End