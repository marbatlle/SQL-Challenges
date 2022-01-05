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
