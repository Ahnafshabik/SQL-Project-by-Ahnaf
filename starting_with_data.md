Question 1: Find out the top 3 out-of-stock products ordered from the United States.

SQL Queries:
SELECT al."productSKU", sr."stockLevel", al.country
FROM all_sessions as al
JOIN sales_report sr
ON al."productSKU" = sr."productSKU"
WHERE sr."stockLevel" = 0 AND al.country = 'United States'
LIMIT 10;

Answer: 

- Insulated Stainless Steel Bottle
- Android BTTF Moonshot Graphic Tee
- Men's Convertible Vest-Jacket Pewter

Question 2:How many customers have ordered products priced above $100 with a quantity exceeding 10?

SQL Queries:
SELECT COUNT("v2ProductCategory") as Num, "v2ProductCategory"
FROM public.all_sessions
WHERE "v2ProductCategory" LIKE 'Home/Apparel%'
GROUP BY "v2ProductCategory";

Answer: Men’s T-Shirt

Question 3: How many customers have ordered products priced above $100 with a quantity exceeding 10?

SQL Queries:  
select an.units_sold,al."visitId" from analyticis an
join all_sessions as al
on an."visitId"=al."visitId"
where an.units_sold>10 and an.unit_price <100;

Answer: 4 Customers.
