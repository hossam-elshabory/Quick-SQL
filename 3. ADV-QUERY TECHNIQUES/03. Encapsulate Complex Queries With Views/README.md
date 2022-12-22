# Quick Intro 

Every query we use on our databases to answer the questions we have will be different, but eventually we will notice that there are some `SELECT` statements that come up again and again. This statement can be complicated, including a lot of tables, joins, and `CASE` statements.

If we could save this `SELECT` statement somewhere and then use it like a table in other SQL queries we could save a lot of time and effort. SQL views will help solve this problem.

SQL views can encapsulate complex queries, exposing the columns in a much cleaner way for use by other queries. 

Think of a SQL view as a predefined `SELECT` statement with one or more tables, and at least one column from one table. A view returns only table columns. Once defined, other SQL queries and statements can use that view as another table. 

Additionally, if multiple database resources use the same view and the tables behind the view change, we would only need to change the view itself, that is, in a one-to-many way. Nothing that relies on the view would have to change. 

Without a view, we would have to change each of those resources, which would become a major pain point. 

The following code will convert the query into a view named `CUSTOMER_PRODUCT_VIEW` :

```SQL
USE my_online_store;

CREATE VIEW CUSTOMER_PRODUCT_VIEW
	AS
		SELECT 
			CONCAT(customers.FirstName, ' ',
				 customers.LastName) 
					AS 'CustomerName',
			orders.OrderDate, products.ProductName
			
		FROM customers 
		
		INNER JOIN orders 
			ON customers.CustomerID = orders.CustomerID
			
		INNER JOIN orderitems 
			ON orders.OrderID = orderitems.OrderID
			
		INNER JOIN products 
			ON orderitems.ProductID = products.ProductID;
```

![Alt text](Images/created%20a%20view.png)

Now, we can use the `customer_product_view` as a table :

```SQL
SELECT CustomerName, OrderDate, ProductName
FROM customer_product_view;
```

![Alt text](Images/using%20the%20view.png)

# Building a View

Let's create a view that shows `my_online_store` customers and their per-product spending by returning the `CustomerID`, `OrderDate`, `ProductID`, `ProductName`, and
`PerProductSpending` columns. :

```SQL
USE my_online_store;

CREATE VIEW STORE_VIEW_1
	AS
		SELECT customers.CustomerID, orders.OrderDate,
			products.ProductID, products.ProductName,
			orderitems.Quantity * orderitems.UnitPrice AS
				'PerProductSpending'
				
		FROM customers 
		
		INNER JOIN orders 
			ON customers.CustomerID = orders.CustomerID
			
		INNER JOIN orderitems 
			ON orders.OrderID = orderitems.OrderID
			
		INNER JOIN products 
			ON orderitems.ProductID = products.ProductID;
```

Let's test the STORE_VIEW_1 view :

```SQL
USE my_online_store;

SELECT CustomerID, OrderDate, ProductID, 
	   ProductName,
	   PerProductSpending
	   
FROM STORE_VIEW_1
WHERE PerProductSpending > 14.99;
```

![Alt text](Images/testing%20last%20view.png)