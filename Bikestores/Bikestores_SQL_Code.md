<h1><a name="introduction">Introduction</a></h1>
<p>Bikestores is a leading global brand in bicycle production and distribution, serving millions of customers across various countries with a diverse range of models. In today’s era of rapid technological advancement, gaining a comprehensive understanding of consumer behavior and market dynamics is critical for sustained success. The Bikestores Executive Tableau Dashboard has been developed to offer a detailed analysis of the company's profitability, providing actionable insights specifically tailored for the executive team. This tool empowers decision-makers to make data-driven choices that enhance strategic planning and drive business growth.</p>

<h1><a name="problemstatement">Problem Statement</a></h1>
<p>Firstly, I need to know what exactly the Management want, they want to know the condition of the sales activities within the company and gain insight into the various trends happening in the sales volume over the 2016 - 2018 period. They also want to know the revenues per region, per store, per product category & per brand, a list of the top customers, sales rep could also prove insightful.</p>


<h1><a name="Solution">Solution</a></h1>

<ol>

  <li><h5>To go further, see below the code to extract the data needed from the dataset</h5></li>
	
```sql
SELECT   
	ord.order_id,  
	CONCAT(cus.first_name,'',cus.last_name) AS 'customers',
	cus.city,
	cus.state,
	ord.order_date,
	SUM(ite.quantity) AS 'total_units',
	SUM(ite.quantity * ite.list_price) AS 'revenue',
	pro.product_name,
	cat.category_name,
	sto.store_name,
	CONCAT(sta.first_name,'', sta.last_name) AS 'sales_rep'
FROM sales.orders ord
JOIN sales.customers cus
ON ord.customer_id = cus.customer_id
JOIN sales.order_items ite
ON ord.order_id = ite.order_id
JOIN production.products pro
ON ite.product_id = pro.product_id
JOIN production.categories cat
ON pro.category_id = cat.category_id
JOIN sales.stores sto
ON ord.store_id = sto.store_id
JOIN sales.staffs sta
ON ord.staff_id = sta.staff_id
GROUP BY 
	ord.order_id,
	CONCAT(cus.first_name,'',cus.last_name),
	cus.city,
	cus.state, 
	ord.order_date,
	pro.product_name,
	cat.category_name,
	sto.store_name,
	CONCAT(sta.first_name,'',sta.last_name)  
```



