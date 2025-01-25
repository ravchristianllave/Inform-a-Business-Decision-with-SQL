## Introduction

In this Coursera Project, SQL is used to explore the Northwind Database. Bonuses will be awarded to those employees who are responsible for the five highest order amounts.

# Objective 1: Explore the Northwind database.

**Database Overview:**

![undefined](https://cdn.mavenanalytics.io/public/profile/a891b370-5021-704e-8440-4d3d6fbf7625/projects/cccc.png)

# Objective 2: Examine the contents of database tables to identify data tables and fields required to inform a business decision.

**Business Question:**

*“As a method of increasing future sales, the company has decided to give employee bonuses for exemplary performance in sales. Bonuses will be awarded to those employees who are responsible for the five highest order amounts. How can we identify those employees?”*

**Proposed Solution:**

A list of employees who have orders with five highest sales amounts.

**Tables used for answering the business question:**

●      Employees:           First and last names of employees

●      Orders:                    Orders with the highest sales value

●      OrderDetails:      Quantity to compute the sales value

●      Products:               Unit Price to compute the sales value

**Sales Value per Order**

Sales amount per item = Quantity multiplied by Unit Price

![undefined](https://cdn.mavenanalytics.io/public/profile/a891b370-5021-704e-8440-4d3d6fbf7625/projects/dfsgsdfgdsg.png)

# Objective 3: Retrieve the data needed to solve a business question by joining multiple tables in SQL.

Tables and fields used for answering the business question:

●      Employees:           LastName and FirstName

●      Orders:                    OrderID

●      OrderDetails:      ProductID and Quantity

●      Products:               Price

**Common key between the related tables:**

![undefined](https://cdn.mavenanalytics.io/public/profile/a891b370-5021-704e-8440-4d3d6fbf7625/projects/asdfsdffsa.png)

**Joining the 4 tables with their related keys**

**Note: W3Schools SQL system is slightly different with other SQL programs, that is why there is a parenthesis in FROM statement in order to execute the query.*

```
SELECT LastName, FirstName, Orders.OrderID, Products.ProductID, Quantity, Price
FROM ((Employees
INNER JOIN Orders
ON Employees.EmployeeID = Orders.EmployeeID)
INNER JOIN OrderDetails
ON Orders.OrderID = OrderDetails.OrderID)
INNER JOIN Products
ON OrderDetails.ProductID = Products.ProductID
ORDER BY LastName, FirstName;
```

![undefined](https://cdn.mavenanalytics.io/public/profile/a891b370-5021-704e-8440-4d3d6fbf7625/projects/123123123.png)

# Objective 4: Write SQL code to calculate and aggregate data.

**Adding a new column of sales value per order**

```
SELECT LastName, FirstName, Orders.OrderID, Products.ProductID, Quantity, Price, Quantity * Price AS SalesAmt
FROM ((Employees
INNER JOIN Orders
ON Employees.EmployeeID = Orders.EmployeeID)
INNER JOIN OrderDetails
ON Orders.OrderID = OrderDetails.OrderID)
INNER JOIN Products
ON OrderDetails.ProductID = Products.ProductID
ORDER BY LastName, FirstName;
```

![undefined](https://cdn.mavenanalytics.io/public/profile/a891b370-5021-704e-8440-4d3d6fbf7625/projects/dfsdfshdsfgh.JPG)

**Taking the sum of the SalesAmt column**

```
SELECT LastName, FirstName, Orders.OrderID, SUM(Quantity * Price) AS SalesAmt
FROM ((Employees
INNER JOIN Orders
ON Employees.EmployeeID = Orders.EmployeeID)
INNER JOIN OrderDetails
ON Orders.OrderID = OrderDetails.OrderID)
INNER JOIN Products
ON OrderDetails.ProductID = Products.ProductID
GROUP BY LastName, FirstName, Orders.OrderID
ORDER BY Orders.OrderID;
```

![undefined](https://cdn.mavenanalytics.io/public/profile/a891b370-5021-704e-8440-4d3d6fbf7625/projects/hsdfghfgds.JPG)

# Objective 5: Display data in a format that can be used to inform a business decision

```
Displaying by SalesAmt by descending to determine the top 5 employees with most sales per order
SELECT TOP 7 LastName, FirstName, Orders.OrderID, SUM(Quantity * Price) AS SalesAmt
FROM ((Employees
INNER JOIN Orders
ON Employees.EmployeeID = Orders.EmployeeID)
INNER JOIN OrderDetails
ON Orders.OrderID = OrderDetails.OrderID)
INNER JOIN Products
ON OrderDetails.ProductID = Products.ProductID
GROUP BY LastName, FirstName, Orders.OrderID
ORDER BY SUM(Quantity * Price) DESC;
```

![undefined](https://cdn.mavenanalytics.io/public/profile/a891b370-5021-704e-8440-4d3d6fbf7625/projects/231gfg.JPG)

If we check their order numbers, the unique top 5 customers have OrderID = 10372, 10424, 10417, 10324 and 10351

**Filtering the OrderIDs**

```
SELECT TOP 7 LastName, FirstName, Orders.OrderID, SUM(Quantity * Price) AS SalesAmt
FROM ((Employees
INNER JOIN Orders
ON Employees.EmployeeID = Orders.EmployeeID)
INNER JOIN OrderDetails
ON Orders.OrderID = OrderDetails.OrderID)
INNER JOIN Products
ON OrderDetails.ProductID = Products.ProductID
GROUP BY LastName, FirstName, Orders.OrderID
HAVING Orders.OrderID IN (10372, 10424, 10417, 10324, 10351)
ORDER BY SUM(Quantity * Price) DESC;
```

![undefined](https://cdn.mavenanalytics.io/public/profile/a891b370-5021-704e-8440-4d3d6fbf7625/projects/dfgsdfg.png)

# Conclusion:

“The top 5 unique customers with the most sales per order ID are the following:

1.     Steven Buchanan

2.     Robert King

3.     Margaret Peacock

4.     Anne Dodsworth

5.     Nancy Davolio
