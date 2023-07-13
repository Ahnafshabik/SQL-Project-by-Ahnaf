Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries: 
SELECT city, country, MAX("totalTransactionRevenue") AS highest_revenue
FROM all_sessions
WHERE city IS NOT NULL
GROUP BY city, country
HAVING MAX("totalTransactionRevenue") IS NOT NULL
ORDER BY highest_revenue DESC;

Answer: City: Atlanta, Country: United States.


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
SELECT al.city, al.country, ROUND(AVG(an.units_sold), 2) as average_units_sold
FROM all_sessions as al
JOIN analyticis as an
ON al."visitId" = an."visitId"
WHERE al.city IS NOT NULL AND al.country IS NOT NULL AND an.units_sold IS NOT NULL
GROUP BY al.city, al.country;

Answer: Top five are in Chicago, Pittsburg, New York, Huston- United States, and Toronto-Canada.

**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

WITH product_rank AS (
  SELECT al.city, al.country, p.name, COUNT(*) as product_count,
         ROW_NUMBER() OVER (PARTITION BY al.city, al.country ORDER BY COUNT(*) DESC) AS rank
  FROM all_sessions as al
  JOIN products as p 
  ON al."productSKU" = p."SKU"
  WHERE al.city IS NOT NULL AND al.country IS NOT NULL
  GROUP BY al.city, al.country, p.name
)
SELECT city, country, name, product_count
FROM product_rank
WHERE rank <= 3
ORDER BY city, country, rank;


Answer: 
The Twill Cap is popular in warmer cities like Seattle, Bangkok, and Chennai, while the Men's 100% Cotton Short Sleeve Hero Tee White is ordered more frequently in cities like Los Angeles, Pune, and San Francisco. North American cities show higher demand for Men's Zip Hoodie and Android Men's Zip Hoodie.


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
SELECT al.city, al.country, p.name AS top_selling_product
FROM all_sessions AS al
JOIN products AS p ON al."productSKU" = p."SKU"
WHERE al.city IS NOT NULL AND al.country IS NOT NULL
GROUP BY al.city, al.country, p.name
HAVING COUNT(*) = (
    SELECT MAX(product_count)
    FROM (
        SELECT al2.city, al2.country, p2.name, COUNT(*) AS product_count
        FROM all_sessions AS al2
        JOIN products AS p2 ON al2."productSKU" = p2."SKU"
        WHERE al2.city = al.city AND al2.country = al.country
        GROUP BY al2.city, al2.country, p2.name
    ) AS counts
)
ORDER BY al.city, al.country;

Answer:
Adelaide, Australia: The top-selling product is "Men's Watershed Full Zip Hoodie Grey".
Ahmedabad, India: The top-selling product is "Trucker Hat".
Akron, United States: The top-selling product is "Men's 100% Cotton Short Sleeve Hero Tee Navy".
Amsterdam, Netherlands: The top-selling products are "Canvas Tote Natural/Navy" and "Device Holder Sticky Pad".
Amsterdam, United States: The top-selling product is "Men's Long Sleeve Raglan Ocean Blue".
Ann Arbor, United States: The top-selling products are "Men's 100% Cotton Short Sleeve Hero Tee Navy" and "Men's Convertible Vest-Jacket Pewter".
Asuncion, Paraguay: The top-selling product is "Men's 100% Cotton Short Sleeve Hero Tee Navy".
Auckland, New Zealand: The top-selling product is "G Noise-reducing Bluetooth Headphones".
Baltimore, United States: The top-selling product is "Women's Short Sleeve Hero Tee Sky Blue".
Bangkok, Thailand: The top-selling products are "Twill Cap" and "Keyboard DOT Sticker".
Barcelona, Spain: The top-selling products range from "Laptop and Cell Phone Stickers" to "Tube Power Bank".


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
SELECT al.city, al.country, SUM(an.revenue) AS total_revenue
FROM all_sessions AS al
JOIN analyticis AS an ON al."visitId" = an."visitId"
WHERE al.city IS NOT NULL AND al.country IS NOT NULL AND an.revenue IS not NULL
GROUP BY al.city, al.country
ORDER BY total_revenue DESC;

Answer:
Most of the cities are from the USA, and there are cities from Europe and one from Asia. (Tel Aviv-Yafo).






