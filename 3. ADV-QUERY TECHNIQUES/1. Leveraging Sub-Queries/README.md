# Quick Intro


As we saw in [Joining Tables](), we can connect more than one table in a single query. The JOIN statement gives us control over the data that our SQL queries return. However, SQL has much more to offer.

In this section, we will look at sub-queries and how they can boost the flexibility and power of our SQL queries. 

Similar to JOINS, sub-queries can connect multiple tables in the same SQL query but the difference is that  sub-queries offers a simpler and cleaner structure to the query; which makes the development and maintenance of the code easier.

# Sub-queries

In a SQL statement, a sub-query operates like a joined table. It gathers data in a structured way and makes that data available for its outer SQL query. 

Placed in parentheses, a sub-query, or an inner query, nests inside a parent, or an outer query.

A sub-query can be nested in the following clauses of the parent query or another sub-query :

- SELECT
- FROM 
- WHERE

A sub-query can certainly return two or more columns, **but if the outer query expects only one column, the sub-query must return only one column**.

Assume the manger wants a list of the products that have zero sales. we can get this information from the `Products` and `OrderItems` tables because the `OrderItems` table connects specific products to specific orders. Let's first answer this question using SQL JOINS :

```SQL
USE my_online_store;

SELECT Products.ProductID, Products.ProductName,
	   Products.ProductCategoryID
	   
FROM Products 

LEFT OUTER JOIN OrderItems
	ON Products.ProductID = OrderItems.ProductID
	
WHERE OrderItems.ProductID IS NULL

ORDER BY Products.ProductID;
```

![Alt text](Images/Leveraging%20Sub-Queries.png)

In the above query, we placed the Products table columns we wanted in the `SELECT` clause. The `LEFT OUTER JOIN` matches the rows between the `Products` and `OrderItems` tables.

The `LEFT OUTER JOIN` also joins the rows in the `Products` table that don't match the `OrderItems` tables. 

In the `WHERE` clause we used `IS NULL` to filter out the rows of the `Products` table that have a match in the `OrderItems` table.

Because we want the `Products` that doesn't appear in the `OrderItems` table. The `ORDER BY` clause sorts the results by `ProductID`. 

We can also solve this problem with a sub-query :

```SQL
USE my_online_store;

SELECT Products.ProductID, Products.ProductName, 
	   Products.ProductCategoryID
	   
FROM Products

WHERE Products.ProductID NOT IN
		(SELECT ProductID FROM OrderItems)
		
ORDER BY Products.ProductID;
```

![Alt text](Images/using%20sub%20quries.png)

The sub-query is the SQL statement between parentheses `SELECT ProductID FROM OrderItems`. This sub-query will return all the `ProductID`s values in the `OrderItems` table. 

In the outer query, the `SELECT` and `FROM` clauses have the columns we require.

```SQL
SELECT Products.ProductID, Products.ProductName, Products.ProductCategoryID
```

The outer query `WHERE` clause uses `NOT IN` to filter out the `Products` table rows that are not in the sub-query returned column. 

Finally, the `ORDER BY` clauses sorts the results by the `ProductID` column.

# Working with sub-queries

Let's get more practice with sub-queries, and use them to find the orders (`OrderID` values) for the orders with no related rows in the `OrderItems` table:

```SQL
USE my_online_store;

SELECT O.OrderID

FROM Orders O

WHERE O.OrderID NOT IN 
		(SELECT OrderID FROM ORDERITEMS)
		
ORDER BY O.OrderID;
``` 

![Alt text](Images/order%20with%20not%20orderitems.png)

In the sub-query `SELECT OrderID FROM ORDERITEMS` we return all the `OrderID` values, and use `NOT IN` in the outer query `WHERE` clause to return only the orders (`OrderID` values) that are not in the `ORDERITEMS` table. 

# Finding the Product Category Name Using a sub-query

The store manger wants to know the product category name for the '*habanero peppers*' product. to get this information we will use a sub-query as follows :

```SQL
USE my_online_store;

SELECT PC.ProductCategoryName

FROM ProductCategories PC

WHERE ProductCategoryID IN
		(SELECT ProductCategoryID 
		 FROM Products 
		 WHERE ProductName = 'habanero peppers');
```

![Alt text](Images/papper%20products%20cat.png)