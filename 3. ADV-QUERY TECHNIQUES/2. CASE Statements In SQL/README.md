# Case Statements

Often, when we write SQL quires and statements we find ourselves in need of considering a set of conditions and then returning different values based one those conditions.

The `CASE` statement in MySQL and SQL Server does exactly this. The SQL `CASE` statement will step through a list of conditions and then return a result based on the first matching condition it finds. 

The `CASE` statements work in the `SELECT`, `UPDATE`, `DELETE`, `WHERE`, and `HAVING` clauses, and they operate a lot like simple IF…ELSE statements.

To show case the `CASE` statement lets say we want to label each product as *CHEAP*, *Mid-Price*, *Expensive*, or *Very Expensive* according to their `NetRetailPrice` using these rules :

- If `NetRetailPrice` is less than or equal to **24.99**, then label it *Cheap*.

- If `NetRetailPrice` is more than **24.99** and less than or equal to **79.99**, then label it *Mid-price*.

- If `NetRetailPrice` is more than **79.99** and less than or equal to **2499.99**, then label it *Expensive*.

- If `NetRetailPrice` has **any other price**, then label it *Very Expensive*.

```SQL
USE my_online_store;

SELECT ProductName, WholesalePrice, NetRetailPrice,
	'Price Point' AS 'PricePoint', 
	UnitKGWeight
	
FROM products
ORDER BY ProductName;
```

![Alt text](Images/CASE%20Statements.png)

The query returns `ProductName`, `WholesalePrice`, `NetRetailPrice`, and `UnitKGWeight` columns from the `Products` table, plus a new column `PricePoint`. 

In the above query the `PricePoint` column shows *Price Point* for each row, we can use the `CASE` statement to change it according to the `NetRetailPrice` of each product as follows :

```SQL
USE my_online_store;

SELECT ProductName, WholesalePrice, NetRetailPrice,

	CASE
		WHEN NetRetailPrice <= 24.99 THEN 'Cheap'
		
		WHEN NetRetailPrice > 24.99 AND 
			NetRetailPrice <= 79.99 THEN 'Mid-price'
			
		WHEN NetRetailPrice > 79.99 AND 
			NetRetailPrice <= 2499.99 THEN 'Expensive'
			
		ELSE 'Very Expensive'
		
	END AS 'Price Point',
	
	UnitKGWeight
	
FROM products

ORDER BY ProductName;
```

![Alt text](Images/applying%20the%20case%20statmenet'.png)

The `CASE` statement starts with the `CASE` keyword and ends with `END`. It has one or more conditions for each rule that we want to test. 

Each condition starts with the `WHEN` keyword, then the condition, followed by the `THEN` keyword, and finally the result for that specific condition. 

As soon as the `CASE` statement finds a true condition, it will execute that condition and then leave the `CASE` statement. The `ELSE` keyword serves as a default if no conditions are true.


# Using Case Statements

Our online store calculates the shipping price based on `NetRetailPrice * UnitKGWeight`. 

Using a `CASE` statement we can label each product shipping price using the following rules :

- If `NetRetailPrice * UnitKGWeight` is less than or equal to 1.0, then label it *Cheap*.

- If `NetRetailPrice * UnitKGWeight` is more than 1.0 and less than or equal to 35.00,
then label it *Mid-price*.

- If `NetRetailPrice * UnitKGWeight` is more than 35.00 and less than or equal to
100.00, then label it *Expensive*.

- If `NetRetailPrice * UnitKGWeight` has any other price, then label it *Very Expensive*.

```SQL
USE my_online_store;

SELECT ProductName, NetRetailPrice, UnitKGWeight,
	CASE
		WHEN (NetRetailPrice * UnitKGWeight) <= 1.0 
		THEN 'Cheap'
		
		WHEN (NetRetailPrice * UnitKGWeight) > 1.0 AND
			(NetRetailPrice * UnitKGWeight) <= 35.00 
			THEN 'Mid-price'
			
		WHEN (NetRetailPrice * UnitKGWeight) > 35.00 AND
		(NetRetailPrice * UnitKGWeight) <= 100.00 
		THEN 'Expensive'
		
		ELSE 'Very Expensive'
		
	END AS 'Shipping Cost'
	
FROM products;
```

![Alt text](Images/CASE%20Statements-1.png)