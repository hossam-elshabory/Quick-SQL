# Ordering Results

Often, we want our query results to be sorted/ordered in a more user-friendly and readable way. We can do that be using the `ORDER BY` clause on our SQL statements which is designed to sort results in either ascending or descending order. By default the `ORDER BY` clause is used, it will sort the result in an ascending order unless specified otherwise.

The `ORDER BY` clause is placed at the end of the `SELECT` statement, and its general form is as follows :

```SQL
ORDER BY [COLUMN1_NAME] [ASC|DESC], [COLUMN2_NAME][ASC|DESC]
```

We can either specify the column name or it's alias.

# Ordering Rows According to a Particular Column

Let's select the `ProductCategoryName` and `ProductCategoryID` columns from the `ProductCategories` table and sort the results by `ProductCategoryName` in an ascending order :

```SQL
SELECT ProductCategoryName AS 'CATEGORY NAME', 
	   ProductCategoryID AS ID
FROM ProductCategories
ORDER BY ProductCategoryName ASC
```

The result is as follows :

![Alt text](Images/order%20by.png)

As you can see the rows are sorted alphabetically by **CATEGORY NAME** in ascending order. Let's try the same query again, but this time order the results in descending order.

```SQL
SELECT ProductCategoryName AS 'CATEGORY_NAME', 
	   ProductCategoryID AS ID
FROM ProductCategories
ORDER BY CATEGORY_NAME DESC;
```

The result is as follows :

![Alt text](Images/order%20descending.png)

# Ordering Rows According to Multiple Columns

If there are multiple records with the same value, the value in the second column is used for ordering. 
Let's take a look at this with an example from the `Customers` table and select the `FirstName` and `CustomerID` :

```SQL
SELECT FirstName, CustomerID
FROM Customers;
```
The result is as follows :

![Alt text](Images/null%20values.png)

There are multiple entries with NULL as their first name. So, when we order our results by `FirstName` in ascending order and the `CustomerID` in descending order, we will notice the difference. Let's implement this as follows :

```SQL
SELECT FirstName, CustomerID
FROM Customers
ORDER BY FirstName, CustomerID DESC;
```

The result is as follows :

![Alt text](Images/Ordering%20Results.png)

It's now sorted alphabetically by `FirstName` in ascending order, even though we didn't mention the ordering type in the statement (ASC being the default order). 

Where there are multiple entries in the name column that are the same (NULL), these are then sorted in descending order by ID.