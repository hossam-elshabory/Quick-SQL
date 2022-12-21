# Quick Intro 

We saw that the `WHERE` clause can be used to filter SQL query result in the previous sections, For example, say we want to identify the total number of employees in a department, or the total number of people who are covered by a medical insurance. In such cases, more than the details, we are looking for a single calculated value. 

For such cases, where we need calculated results based on database rows, we use SQL aggregate functions. They can also be used to calculate values across subsets of query result rows. We will also look at the `GROUP BY` and the `HAVING` clauses.

# Aggregate Functions (SUM, COUNT, AVG, MIN, and MAX) and the `GROUP BY` Clause 

MySQL provides functions that return single calculated values. lets first look at a general way to use these functions :

- **SUM**: Adds, or sums, relevant values.
- **COUNT**: Returns a count of the relevant values.
- **AVG (average)**: Calculates the average of a set of relevant values.
- **MIN (minimum value)**: Returns the lowest value of a set of values.
- **MAX (maximum value)**: Returns the highest value of a set of values.

The store manger wants to know the average weight of the products in each product category, in `my_online_store` database each `ProductCategory` table maps to one or more product in the `Products` table; so we can start building our query like the following :

```SQL
USE my_online_store;

SELECT PC.ProductCategoryID, PC.ProductCategoryName, 
	   P.UnitKGWeight AS 'PRODUCT KG WEIGHT'
	
FROM ProductCategories PC 

INNER JOIN Products P 
	ON PC.ProductCategoryID = P.ProductCategoryID;
```

![Alt text](Images/before%20GROUP%20BY.png)

This query returned a row for every product in each product category, and every row shows the `UnitKGWeight` value for each individual product. This doesn't solve our problem. We need another way to gather, or aggregate, the average weights of all the products in each category. 

The SQL `AVG`, or average, function will solve the problem. Like all aggregate functions, `AVG` needs `GROUP BY` in the queries that use containing the `AVG` function. 

The `GROUP BY` clause separates the results of a `SELECT` statement into one or more row groups based on a specific criterion.

If we add the `AVG` function and the `GROUP BY` clause to the query, our result will be as the following :

```SQL
USE my_online_store;

SELECT PC.ProductCategoryID, PC.ProductCategoryName,
	   AVG(P.UnitKGWeight) AS 'AVERAGE PRODUCT KG WEIGHT'
	
FROM ProductCategories PC 
INNER JOIN Products P 
	ON PC.ProductCategoryID = P.ProductCategoryID
	
GROUP BY PC.ProductCategoryName, PC.ProductCategoryID;
```

![Alt text](Images/after%20GROUP%20BY.png)

In the above query, the `AVG` function calculates the average of the `UnitKGWeight` column in the `Products` table, But we need to collect the `Products` table rows into single product category groups for the aggregate function to make work.

The `GROUP BY` clause groups the `Products` table rows by category and the `AVG` function in the `SELECT` clause calculates the average `UnitKGWeight` for all the products in each group.

As a general rule we should place every `SELECT` clause column that is not an aggregate function in the `GROUP BY` clause. The `GROUP BY` clause will group by every unique value of the combination of `ProductCategoryNam`e and `ProductCategoryID`. 

This means that if two products have the same category name and the same category ID, they will be placed in the same group.

As in the following query :

```SQL
USE my_online_store;

SELECT PC.ProductCategoryID, PC.ProductCategoryName,
	   AVG(P.UnitKGWeight) AS 'AVERAGE PRODUCT KG WEIGHT',
	   MIN(P.NetRetailPrice) AS 'MINIMUM NET RETAIL PRICE'
	
FROM ProductCategories PC 
INNER JOIN Products P 
	ON PC.ProductCategoryID = P.ProductCategoryID
	
GROUP BY PC.ProductCategoryName, PC.ProductCategoryID;
```

![Alt text](Images/two%20aggregate%20functions.png)

In above query, the `MIN` function finds the minimum `NetRetailPrice` value in each product category group.

Also, we can use the `ORDER BY` clause with an aggregate function to order the columns. Say that we need to sort the preceding query by the `AVERAGE PRODUCT KG WEIGHT` column.

```SQL
USE my_online_store;

SELECT PC.ProductCategoryID, PC.ProductCategoryName,
	   AVG(P.UnitKGWeight) AS 'AVERAGE PRODUCT KG WEIGHT',
	   MIN(P.NetRetailPrice) AS 'MINIMUM NET RETAIL PRICE'
	
FROM ProductCategories PC 
INNER JOIN Products P 
	ON PC.ProductCategoryID = P.ProductCategoryID
	
GROUP BY PC.ProductCategoryName, PC.ProductCategoryID
ORDER BY 'AVERAGE PRODUCT KG WEIGHT';
```

![Alt text](Images/ordering%20by%20group%20by.png)

The `ORDER BY` clause will accept a column alias name, or the actual aggregate function itself (in this case, `AVG(P.UnitKGWeight))`, as seen in this query :

```SQL
USE my_online_store;

SELECT PC.ProductCategoryID, PC.ProductCategoryName,
	   AVG(P.UnitKGWeight) AS 'AVERAGE PRODUCT KG WEIGHT',
	   MIN(P.NetRetailPrice) AS 'MINIMUM NET RETAIL PRICE'
	
FROM ProductCategories PC 
INNER JOIN Products P 
	ON PC.ProductCategoryID = P.ProductCategoryID
	
GROUP BY PC.ProductCategoryName, PC.ProductCategoryID
ORDER BY AVG(P.UnitKGWeight);
```

![Alt text](Images/order%20by%20with%20no%20aliase.png)

To sum things up lets look at query that has all five MySQL aggregate functions :

```SQL
USE my_online_store;

SELECT PC.ProductCategoryID, PC.ProductCategoryName,
	   AVG(P.UnitKGWeight) AS 'AVERAGE PRODUCT KG WEIGHT',
	   MIN(P.NetRetailPrice) AS 'MINIMUM NET RETAIL PRICE',
	   MAX(P.WholesalePrice) AS 'MAX WHOLESALE PRICE',
	   COUNT(P.AvailableQuantity) AS 'COUNT AVAILABLE QUANTITY',
	SUM(P.AvailableQuantity) AS 'SUM AVAILABLE QUANTITY'
	
FROM ProductCategories PC 
INNER JOIN Products P 
	ON PC.ProductCategoryID = P.ProductCategoryID
	
GROUP BY PC.ProductCategoryName, PC.ProductCategoryID;
```

![Alt text](Images/all%20five%20aggregate%20functions.png)

# Implementing Aggregate Functions

Let's use SQL SQL aggregate functions to find the price of the highest and lowest products price, we can do so by the following query :

1. Start by the basic query that joins all the tables we need

```SQL
SELECT O.OrderID, C.LastName
FROM Customers C 
INNER JOIN Orders O 
	ON C.CustomerID = O.CustomerID
INNER JOIN OrderItems OI 
	ON O.OrderID = OI.OrderID;
```

2. Then add the aggregate functions

```SQL
SELECT O.OrderID, C.LastName,
	MIN(OI.UnitPrice) AS 'LOWEST PRICED PRODUCT PRICE,
	MAX(OI.UnitPrice) AS 'HIGHEST PRICED PRODUCT PRICE'
FROM Customers C 
INNER JOIN Orders O 
	ON C.CustomerID = O.CustomerID
INNER JOIN OrderItems OI 
	ON O.OrderID = OI.OrderID;
```

3. Grouping by the `GROUP BY` :

```SQL
SELECT O.OrderID, C.LastName,
	   MIN(OI.UnitPrice) AS 'LOWEST PRICED PRODUCT PRICE',
	   MAX(OI.UnitPrice) AS 'HIGHEST PRICED PRODUCT PRICE'

FROM Customers C 
INNER JOIN Orders O 
	ON C.CustomerID = O.CustomerID
INNER JOIN OrderItems OI 
	ON O.OrderID = OI.OrderID
	
GROUP BY C.LastName, O.OrderID;
```

![Alt text](Images/highest%20and%20lowest%20product.png)