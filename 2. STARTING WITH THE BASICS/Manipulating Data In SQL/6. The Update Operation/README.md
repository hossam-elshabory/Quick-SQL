# The `UPDATE` Operation 

`UPDATE` modifies data in one or more column in a table, But just like the `INSERT` operation it can only be applied  on a single table at a time.

Most of the time you will have to filter out the records you need to update. This can be achieved using the `WHERE` clause (The one we also used to filter records in the `DELETE` Operation).

Suppose Ava-May changed her name to Ava-May Rodgers, and you had to update the table. 
You would use the following SQL statement :

```SQL
UPDATE employees
SET
	Email = 'Ava-May.Rodgers@awesomenes.com'
WHERE empno = 3
```

In the above example we can conclude that the `UPDATE` statement consists of the the `UPDATE` keyword, followed by the table name, followed by the `SET` keyword, followed by the column names and values. If you would like to modify specific values, you would use a `WHERE` clause with the condition.

# A Basic UPDATE Statement

We have a table called `department` in the database. Let's say that you want to modify the the data for each of the departments to the current data (Today's  date). We do this using the following statement :

```SQL
UPDATE department SET departmentEstDate = now();
```

If you query all the records within the table and see what `departmentEstDate` looks like for each record :

```SQL
SELECT * FROM department;
```

The `departmentEstDate` should be updated :

![Alt text](Images/update%20departmentdate.png)

# Conditional Update of Records

Let's say that everyone in the sales team got  20% raise  to their salary, assuming the salary of the sales team is '8000', the new salary after the raise will be (8000 * 1.2) = 9,600.

If you want to update the new salary of the sales department employees, you can do it with the following SQL statement

```SQL
UPDATE Employee
	SET 
		Salary = 9600
	WHERE department = 'Sales'
FROM Employee
```

# UPDATE Using Computed Values

Consider the scenario where we are providing 10% off the net retail price of all the products in `my_online_store` :

This is how the `NetRetailPrice` looks before the 10% off discount

![Alt text](Images/products%20before%20discount.png)

To compute and add the discount we will do the following :

1. Type the following query :

```SQL
UPDATE products
	SET
		NetRetailPrice = NetRetailPrice * 0.90;
```

2. Check out the table `products` :

![Alt text](Images/products%20after%20discount.png)