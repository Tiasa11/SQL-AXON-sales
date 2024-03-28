![Axon Sales Dashboard- 2](https://github.com/Tiasa11/SQL-AXON-sales/assets/164670315/a3822ecf-6f8f-49a4-91cd-ec810eb204d3)
![Axom Sales Dashboard 1](https://github.com/Tiasa11/SQL-AXON-sales/assets/164670315/2f775303-4c67-4342-aeeb-a5df30a2834c)

The Axon Sales Dashboard, crafted through the integration of SQL and PowerBI, offers a comprehensive overview of sales performance metrics and trends. Tailored for the Automobile Sales business, this dashboard empowers data-driven decision-making and strategic planning by providing invaluable insights into revenue generation, sales patterns over different time frames and geographical regions, as well as product performance analysis.

✨ Key Business Insights:- 

🙎‍♂️ Customer Base:
 Engagement with a diverse customer base consisting of 98 distinct entities.

📦 Order Fulfillment:
Successful completion of 326 orders.

💲 Financial Summary:
Total Sales: $9.60 million, demonstrating consistent growth.
Total Profit: $3.83 million, showing a linear relationship with sales.

📈 Peak Performance Month:
November 2004 identified as the peak month due to significant upticks in orders, sales, and profit attributed to Black Friday and Holiday sales.

🚗 Top Product Lines:
Classic Cars & Vintage Cars account for 60% of total sales.

🚘 Top Profitable Products:
1992 Ferrari 360 Spider red
1952 Alpine Renault 1300
2001 Ferrari Enzo

👨‍💼 Top Employees by Profit:
Gerard Hernandez
Leslie Jennings
Pamela Castillo

🌍 Global Sales Presence:
Maximum sales recorded in the USA, Spain, and France.










AXON Sales SQL Queries. 

Total Profit:- 

    SELECT
    SUM((od.quantityOrdered * od.priceEach) - (od.quantityOrdered * p.buyPrice)) AS totalProfits
    FROM
    OrderDetails od
    JOIN
    Products p ON od.productCode = p.productCode;
 

Total Sale:- 

    SELECT SUM(quantityOrdered * priceEach) AS totalSales FROM OrderDetails;

 

Total Customers:- 

    SELECT 
    COUNT(DISTINCT c.customerNumber) AS totalCustomers
    FROM Customers c
    JOIN Orders o ON c.customerNumber = o.customerNumber;

 

Total Orders:- 
           
    SELECT COUNT(*) AS TotalOrders FROM Orders;
 

Total Sales by Country:- 
    
    
    SELECT 
    c.country,
    YEAR(o.orderDate) AS orderYear,
    SUM(od.quantityOrdered * od.priceEach) AS totalSales
    FROM 
    Customers c
    JOIN 
    Orders o ON c.customerNumber = o.customerNumber
    JOIN 
    OrderDetails od ON o.orderNumber = od.orderNumber
    JOIN 
    Products p ON od.productCode = p.productCode
    GROUP BY 
    c.country, orderYear
    ORDER BY 
    orderYear DESC, totalSales DESC;
 

Top Sales Person:- 

    SELECT 
    e.employeeNumber,
    e.firstName,
    e.lastName,
    YEAR(o.orderDate) AS orderYear,
    SUM(od.quantityOrdered * od.priceEach) AS totalSales
    FROM 
    Employees e
    JOIN 
    Customers c ON e.employeeNumber = c.salesRepEmployeeNumber
    JOIN 
    Orders o ON c.customerNumber = o.customerNumber
    JOIN 
    OrderDetails od ON o.orderNumber = od.orderNumber
    GROUP BY 
    e.employeeNumber, e.firstName, e.lastName, orderYear
    ORDER BY 
    orderYear DESC, totalSales DESC;

 

Total Sales by Product Line:- 

    SELECT
    pl.productLine,
    SUM(od.quantityOrdered * od.priceEach) AS totalSales
    FROM
    ProductLines pl
    JOIN
    Products p ON pl.productLine = p.productLine
    JOIN
    OrderDetails od ON p.productCode = od.productCode
    JOIN
    Orders o ON od.orderNumber = o.orderNumber
    GROUP BY
    pl.productLine
    ORDER BY
    totalSales DESC;

 

Monthly Trends for total profits :-

    SELECT
    YEAR(o.orderDate) AS orderYear,
    MONTHNAME(o.orderDate) AS orderMonth,
    SUM(od.quantityOrdered * od.priceEach - od.quantityOrdered * p.buyPrice) AS totalProfit
    FROM
    Orders o
    JOIN
    OrderDetails od ON o.orderNumber = od.orderNumber
    JOIN
    Products p ON od.productCode = p.productCode
    JOIN
    Payments pay ON o.customerNumber = pay.customerNumber
    GROUP BY
    orderYear, orderMonth
    ORDER BY
    orderYear, MONTH(o.orderDate); 


 


Monthly Trends for total sales :-


    SELECT
    YEAR(o.orderDate) AS orderYear,
    MONTHNAME(o.orderDate) AS orderMonth,
    SUM(od.quantityOrdered * od.priceEach) AS totalSales
     FROM
    Orders o
    JOIN
    OrderDetails od ON o.orderNumber = od.orderNumber
    GROUP BY
    orderYear, orderMonth
    ORDER BY
    orderYear, MONTH(o.orderDate);


 



Top Selling products and Quantity Ordered :-

    
    SELECT
    YEAR(o.orderDate) AS orderYear,
    p.productCode,
    p.productName,
    SUM(od.quantityOrdered) AS totalQuantitySold,
    SUM(od.quantityOrdered * od.priceEach) AS totalSales
    FROM
    Products p
    JOIN
    OrderDetails od ON p.productCode = od.productCode
    JOIN
    Orders o ON od.orderNumber = o.orderNumber
    GROUP BY
    orderYear, p.productCode, p.productName
    ORDER BY
    orderYear, totalQuantitySold DESC;

 








