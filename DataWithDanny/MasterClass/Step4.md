## Step 4 - Transactions Table -

1. How many records are there in the trading.transactions table?

    SELECT COUNT(*) FROM trading.transactions;

    | count |
    | ----- |
    | 22918 |

1. How many unique transactions are there?

    SELECT COUNT(DISTINCT txn_id) 
    FROM trading.transactions;

    | count |
    | ----- |
    | 22918 |

1. How many buy and sell transactions are there for Bitcoin?

    SELECT txn_type, COUNT(*) 
    FROM trading.transactions
    WHERE ticker = 'BTC'
    GROUP BY 1;

    | txn_type | count |
    | -------- | ----- |
    | BUY      | 10440 |
    | SELL     | 2044  |

1. For each year, calculate the following buy and sell metrics for Bitcoin: total transaction count, total quantity, average quantity per transaction

    SELECT 
    	EXTRACT(YEAR FROM txn_date) AS txn_year,
        txn_type,
        COUNT(*) AS total_txn_count,
        ROUND(SUM(quantity)::NUMERIC,2) AS total_quantity,
        ROUND(AVG(quantity)::NUMERIC,2) AS avg_quantity
    FROM trading.transactions
    GROUP BY 2,1
    ORDER BY 1,2;

    | txn_year | txn_type | total_txn_count | total_quantity | avg_quantity |
    | -------- | -------- | --------------- | -------------- | ------------ |
    | 2017     | BUY      | 4064            | 21751.61       | 5.35         |
    | 2017     | SELL     | 854             | 4321.30        | 5.06         |
    | 2018     | BUY      | 3993            | 19922.07       | 4.99         |
    | 2018     | SELL     | 863             | 4320.24        | 5.01         |
    | 2019     | BUY      | 4041            | 20226.60       | 5.01         |
    | 2019     | SELL     | 867             | 4364.99        | 5.03         |
    | 2020     | BUY      | 4183            | 21033.04       | 5.03         |
    | 2020     | SELL     | 864             | 4328.17        | 5.01         |
    | 2021     | BUY      | 2600            | 12916.04       | 4.97         |
    | 2021     | SELL     | 589             | 2864.94        | 4.86         |

1. What was the monthly total quantity purchased and sold for Ethereum in 2020?

    SELECT
    	DATE_TRUNC('MON', txn_date)::DATE AS month,
        ROUND(SUM(CASE WHEN txn_type = 'BUY' THEN quantity ELSE 0 END)::NUMERIC, 2) AS buy_quantity,
        ROUND(SUM(CASE WHEN txn_type = 'SELL' THEN quantity ELSE 0 END)::NUMERIC, 2) AS sell_quantity
    FROM trading.transactions
    WHERE txn_date BETWEEN '2020-01-01' AND '2020-12-31'
    GROUP BY 1
    ORDER BY 1;

    | month                    | buy_quantity | sell_quantity |
    | ------------------------ | ------------ | ------------- |
    | 2020-01-01T00:00:00.000Z | 1882.70      | 349.37        |
    | 2020-02-01T00:00:00.000Z | 1531.01      | 343.51        |
    | 2020-03-01T00:00:00.000Z | 1693.57      | 366.96        |
    | 2020-04-01T00:00:00.000Z | 1725.63      | 422.64        |
    | 2020-05-01T00:00:00.000Z | 1761.69      | 350.47        |
    | 2020-06-01T00:00:00.000Z | 1682.28      | 387.64        |
    | 2020-07-01T00:00:00.000Z | 1887.23      | 271.07        |
    | 2020-08-01T00:00:00.000Z | 1817.68      | 413.45        |
    | 2020-09-01T00:00:00.000Z | 1800.52      | 313.55        |
    | 2020-10-01T00:00:00.000Z | 1849.57      | 371.81        |
    | 2020-11-01T00:00:00.000Z | 1698.41      | 347.54        |
    | 2020-12-01T00:00:00.000Z | 1702.75      | 390.16        |

1. Summarise all buy and sell transactions for each member_id by generating 1 row for each member with the following additional columns: Bitcoin buy quantity, Bitcoin sell quantity, Ethereum buy quantity, Ethereum sell quantity

    SELECT
    	member_id,
        SUM(CASE WHEN ticker = 'BTC' AND txn_type = 'BUY' THEN quantity ELSE 0 END) AS btc_buy_quantity,
        SUM(CASE WHEN ticker = 'BTC' AND txn_type = 'SELL' THEN quantity ELSE 0 END) AS btc_sell_quantity,
        SUM(CASE WHEN ticker = 'ETH' AND txn_type = 'BUY' THEN quantity ELSE 0 END) AS btc_buy_quantity,
        SUM(CASE WHEN ticker = 'ETH' AND txn_type = 'SELL' THEN quantity ELSE 0 END) AS btc_sell_quantity
    FROM trading.transactions
    GROUP BY member_id;

    | member_id | btc_buy_quantity   | btc_sell_quantity  | btc_buy_quantity   | btc_sell_quantity  |
    | --------- | ------------------ | ------------------ | ------------------ | ------------------ |
    | c20ad4    | 4975.750641191644  | 929.659744519077   | 2187.1154401373137 | 610.0470610814083  |
    | c51ce4    | 2580.4064599247727 | 1028.7200828179675 | 2394.7300314796344 | 1076.700582569547  |
    | c4ca42    | 4380.44293157246   | 1075.5626055691553 | 4516.597248410067  | 1011.5619313973583 |
    | aab323    | 3491.8873912094978 | 916.3032786678012  | 4373.762101490244  | 811.3775177368207  |
    | c9f0f8    | 4572.880084238887  | 852.3638794847988  | 2343.469079013971  | 254.26718075171567 |
    | eccbc8    | 2844.6515509972596 | 305.34548935523316 | 2573.7547576415836 | 794.0567116424365  |
    | a87ff6    | 5023.705687783489  | 863.4858182768505  | 3822.0371970017663 | 318.1506358514526  |
    | e4da3b    | 3567.3882471515076 | 998.3785353595931  | 2053.983325296016  | 581.7120621124521  |
    | 45c48c    | 3814.2424689381746 | 198.13102225001106 | 4442.136854227907  | 216.4978255494174  |
    | 6512bd    | 4031.6925788360772 | 574.7827987664845  | 2941.2223099752014 | 966.1617396549941  |
    | c81e72    | 2600.9308762349483 | 974.0950235235426  | 4852.521571017203  | 729.4207172459708  |
    | d3d944    | 4270.857382342532  | 735.872222173432   | 1744.6542567360968 | 1057.4159633448544 |
    | 8f14e4    | 2647.0768334782097 | 445.74386254752034 | 3233.476850395781  | 663.7834260899181  |
    | 167909    | 4448.238806248943  | 503.0407229884322  | 1119.7353314008792 | 707.0007288859948  |

1. What was the final quantity holding of Bitcoin for each member? Sort the output from the highest BTC holding to lowest

    SELECT
    	member_id,
    	ROUND(SUM(quantity)::NUMERIC,2) AS quantity_BTC
    FROM trading.transactions
    WHERE EXTRACT(YEAR FROM txn_date) = '2020'
    AND ticker = 'BTC'
    GROUP BY 1
    ORDER BY 2 DESC;

| member_id | quantity_btc |
| --------- | ------------ |
| c20ad4    | 1344.19      |
| c4ca42    | 1281.75      |
| c9f0f8    | 1206.24      |
| 167909    | 1168.78      |
| a87ff6    | 1145.90      |
| d3d944    | 1101.74      |
| 6512bd    | 975.75       |
| aab323    | 951.48       |
| e4da3b    | 949.48       |
| c51ce4    | 922.43       |
| 45c48c    | 904.99       |
| c81e72    | 769.73       |
| eccbc8    | 683.77       |
| 8f14e4    | 644.52       |

1. Which members have sold less than 500 Bitcoin? Sort the output from the most BTC sold to least

    SELECT 
    	member_id,
        SUM(quantity) AS BTC_quantity
    FROM trading.transactions
    WHERE ticker = 'BTC' 
    	AND txn_type = 'SELL'
    GROUP BY member_id
    HAVING SUM(quantity) < 500
    ORDER BY BTC_quantity DESC;

    | member_id | btc_quantity      |
    | --------- | ----------------- |
    | 8f14e4    | 445.7438625475204 |
    | eccbc8    | 305.3454893552332 |
    | 45c48c    | 198.131022250011  |

1. What is the total Bitcoin quantity for each member_id owns after adding all of the BUY and SELL transactions from the transactions table? Sort the output by descending total quantity

    SELECT
    	member_id,
        SUM(
          CASE 
            WHEN txn_type = 'SELL' THEN -quantity 
            WHEN txn_type = 'BUY' THEN quantity
        END) AS balance
    FROM trading.transactions
    WHERE ticker = 'BTC'
    GROUP BY member_id
    ORDER BY balance DESC;

    | member_id | balance            |
    | --------- | ------------------ |
    | a87ff6    | 4160.219869506643  |
    | c20ad4    | 4046.090896672561  |
    | 167909    | 3945.198083260507  |
    | c9f0f8    | 3720.5162047540916 |
    | 45c48c    | 3616.1114466881622 |
    | d3d944    | 3534.985160169097  |
    | 6512bd    | 3456.9097800695936 |
    | c4ca42    | 3304.880326003308  |
    | aab323    | 2575.584112541697  |
    | e4da3b    | 2569.0097117919117 |
    | eccbc8    | 2539.3060616420257 |
    | 8f14e4    | 2201.3329709306895 |
    | c81e72    | 1626.8358527114085 |
    | c51ce4    | 1551.6863771068054 |

1. Which member_id has the highest buy to sell ratio by quantity?

    SELECT
    	member_id,
        SUM(CASE WHEN txn_type = 'BUY' THEN quantity END) /
        SUM(CASE WHEN txn_type = 'SELL' THEN quantity END)
        AS ratio_balance
    FROM trading.transactions
    GROUP BY member_id
    ORDER BY ratio_balance DESC;

    | member_id | ratio_balance      |
    | --------- | ------------------ |
    | 45c48c    | 19.912698711113308 |
    | a87ff6    | 7.486010484765202  |
    | c9f0f8    | 6.249914187095622  |
    | 8f14e4    | 5.300053224554441  |
    | eccbc8    | 4.928502329467622  |
    | c20ad4    | 4.652097435222711  |
    | 167909    | 4.6014738825886505 |
    | aab323    | 4.552721491764275  |
    | 6512bd    | 4.525091406569532  |
    | c81e72    | 4.375335236929055  |
    | c4ca42    | 4.262821897975362  |
    | e4da3b    | 3.557626114250054  |
    | d3d944    | 3.354458969649687  |
    | c51ce4    | 2.3630130420937574 |

1. For each member_id - which month had the highest total Ethereum quantity sold`?

    WITH cte_ranked AS (
    SELECT
      member_id,
      DATE_TRUNC('MON', txn_date)::DATE AS calendar_month,
      SUM(quantity) AS sold_eth_quantity,
      RANK() OVER (PARTITION BY member_id ORDER BY SUM(quantity) DESC) AS month_rank
    FROM trading.transactions
    WHERE ticker = 'ETH' AND txn_type = 'SELL'
    GROUP BY member_id, calendar_month
    )
    SELECT
      member_id,
      calendar_month,
      sold_eth_quantity
    FROM cte_ranked
    WHERE month_rank = 1
    ORDER BY sold_eth_quantity DESC;

    | member_id | calendar_month           | sold_eth_quantity  |
    | --------- | ------------------------ | ------------------ |
    | c51ce4    | 2017-05-01T00:00:00.000Z | 66.09244042953502  |
    | d3d944    | 2020-04-01T00:00:00.000Z | 60.41736997398335  |
    | 6512bd    | 2018-05-01T00:00:00.000Z | 47.932857149515904 |
    | 167909    | 2020-12-01T00:00:00.000Z | 45.92423664055218  |
    | c81e72    | 2018-08-01T00:00:00.000Z | 41.26728177476413  |
    | aab323    | 2018-09-01T00:00:00.000Z | 41.175076337098375 |
    | c4ca42    | 2021-04-01T00:00:00.000Z | 40.11347472402258  |
    | c20ad4    | 2017-12-01T00:00:00.000Z | 37.71908498970638  |
    | eccbc8    | 2021-03-01T00:00:00.000Z | 36.485934922948275 |
    | 8f14e4    | 2017-07-01T00:00:00.000Z | 36.17383743681231  |
    | e4da3b    | 2019-01-01T00:00:00.000Z | 30.484426410776322 |
    | a87ff6    | 2020-07-01T00:00:00.000Z | 27.28919836842423  |
    | 45c48c    | 2020-01-01T00:00:00.000Z | 20.2152340642537   |
    | c9f0f8    | 2020-11-01T00:00:00.000Z | 15.931855129247868 |