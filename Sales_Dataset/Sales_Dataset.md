# SALES DATASET  

**Entity Dataset Diagram**  
![Entity_Diagram](https://github.com/Temitope-Fabiyi/SQL_Projects/blob/main/Sales_Dataset/Entity%20Diagram.jpg)  

## QUESTIONS  
 <li><h5>1. How many users are in the database?</h5></li>

```sql
SELECT count(distinct user_id)
FROM baskets;
```  
Answer:  
 22,525  

<li><h5>2. How many baskets are in the database?</h5></li>  

```sql  
SELECT COUNT(distinct basket_id) 
FROM baskets;
```   
Answer:
25,240  

<li><h5>3. What is the percentage of baskets that were abandoned?</h5></li>  

```sql  
SELECT (SUM(abandond) /COUNT(abandond))*100 as percentage_abandoned 
FROM baskets;
```
Answer:
20.0016  

<li><h5>4. What is the percentage of baskets sold with discounts?</h5></li>  

```sql
SELECT ((SELECT COUNT(*) from baskets
WHERE discount > 0)/ COUNT(*)) * 100
AS discounted_basket_orders 
FROM baskets;
```  
Answer:  
29.8227  

<li><h5>5. What are the discounts?</h5></li>  

```sql
SELECT DISTINCT discount 
FROM baskets;
```  
Answer:  
0, 0.1, 0.5, 0.15, 0.25  

<li><h5>6. How many baskets are in each discount group?</h5></li>

```sql
SELECT discount, Count(basket_id) AS no_of_baskets 
FROM baskets
GROUP BY 1;
```  
Answer:  
|discount | no_of_baskets|  
|---------|--------------|  
|0.00|17971|  
|0.10|2566|  
|0.50|1300|  
|0.15|2561|  
|0.25|1210|

<li><h5>7. The highest revenue was recorded on which day of the week?</h5></li>  

```sql
SELECT DAYNAME(baskets.event_time) AS day_of_week, SUM(p.price - p.cost) AS revenue
FROM baskets 
JOIN basketproducts bp ON baskets.basket_id = bp.basket_id
JOIN products p ON bp.product_id = p.product_id
WHERE baskets.abandond = 0 -- Assuming 'abandond' is a flag indicating whether the basket was abandoned
GROUP BY day_of_week 
ORDER BY revenue DESC
LIMIT 1;
```  
Answer:
|day_of_week | Revenue|  
|---------|--------------|  
|Monday|4133.21| 


<li><h5>8. The highest revenue was recorded on which day?</h5></li>  

```sql  
SELECT DATE (baskets.event_time) AS date, SUM(p.price - p.cost) AS revenue
FROM baskets 
JOIN basketproducts bp ON baskets.basket_id = bp.basket_id
JOIN products p ON bp.product_id = p.product_id
WHERE baskets.abandond = 0 -- Assuming 'abandond' is a flag indicating whether the basket was abandoned
GROUP BY DATE(baskets.event_time)
ORDER BY revenue DESC
LIMIT 1;   
```    

Answer:
|date | Revenue|  
|---------|--------------|  
|2019-12-01|2434.32|   

<li><h5>9. Top 10 most profitable products and their brands</h5></li>  

```sql
SELECT products.product_id, products.brand, SUM(products.price - products.cost) AS profit
FROM products 
JOIN basketproducts bp ON products.product_id = bp.product_id
JOIN baskets b ON bp.basket_id = b.basket_id
WHERE b.abandond = 0 -- Assuming 'abandond' is a flag indicating whether the basket was abandoned
GROUP BY products.product_id, products.brand
ORDER BY profit DESC
LIMIT 10;  
```

Answer:
|product_id | brand| profit|  
|---------|--------|-------|  
|5560754|strong|96.59|
|5906221|strong|84.08|
|5910166|strong|74.14|
|5590822|strong|70.81|
|5676286|marathon|68.01|
|5560760|strong|57.04|
|5888097|shik|55.66|
|5646091|cnd|53.05|
|5888079|shik|49.42|
|5866135|s.care|49.10|  

<li><h5>10.	Top 5 most profitable brands</h5></li>  

```sql  
SELECT products.brand, SUM(products.price - products.cost) AS profit
FROM products 
JOIN basketproducts ON products.product_id = basketproducts.product_id
JOIN baskets b ON basketproducts.basket_id = b.basket_id
WHERE b.abandond = 0 -- Assuming 'abandond' is a flag indicating whether the basket was abandoned
GROUP BY products.brand
ORDER BY profit DESC
LIMIT 5;  
```  

Answer:  
|brand| profit|
|-----|-------| 
|runail|996.7|
|irisk|927.57|
|cnd|818.85|
|estel|798.91|
|masura|658.66|  





