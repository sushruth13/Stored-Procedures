Stored Procedures are statements written in SQL and stored in the database which allows it to be reused over multiple programs. Stored procedures are written in SQL and stored in the cache once it has complied and run.
    Advantages of Stored Procedures

1) Reduce network traffic Stored procedures help reduce the network traffic between applications and MySQL Server. Because instead of sending multiple lengthy SQL statements, applications have to send only the name and parameters of stored procedures.
2) Centralize business logic in the database You can use the stored procedures to implement business logic that is reusable by multiple applications. The stored procedures help reduce the efforts of duplicating the same logic in many applications and make your database more consistent.
3) Make database more secure the database administrator can grant appropriate privileges to applications that only access specific stored procedures without giving any privileges on the underlying tables.
Disadvantages of Stored Procedures
1)	If you use many stored procedures, the memory usage of every connection will increase substantially.
2)	It’s difficult to debug stored procedures. Unfortunately, MySQL does not provide any facilities to debug stored procedures like other enterprise database products such as Oracle and SQL Server.
3)	Developing and maintaining stored procedures often requires a specialized skill set that not all application developers possess. This may lead to problems in both application development and maintenance.
Note: The queries are based on the sql file in the repo
Examples 
Normal SQL queries
SELECT customerName, city, state, postalCode, country FROM customers
ORDER BY customerName;

 
CREATE PROCEDURE GetCustomers()
BEGIN
    SELECT 
        customerName, 
        city, 
        state, 
        postalCode, 
        country
    FROM
        customers
    ORDER BY customerName;    
END$$
DELIMITER ;

The above procedure can be executed by
CALL GetCustomers();

Defining parameters in stored procedure
CREATE PROCEDURE findShips(IN product varchar(255))
BEGIN
select productName from products where productLine=product;
END
To call the above proc
CALL findShips(<<variable>>)

DELIMITER $$
CREATE PROCEDURE GetOrderCountByStatus (
    IN  orderStatus VARCHAR(25),
    OUT total INT
)
BEGIN
    SELECT COUNT(orderNumber)
    INTO total
    FROM orders
    WHERE status = orderStatus;
END$$
 
DELIMITER ;

To call the above proc
CALL getOrderCountByStatus('Shipped',@total);
SELECT @total;

Running 2 queries
CREATE DEFINER=`root`@`localhost` PROCEDURE `twoprocs`(IN product varchar(255),IN market VARCHAR(255))
BEGIN
select productName from products where productLine=product;
select * from customers where country=market;
END
To call the above proc
CALL twoprocs(<<variable>>,<<variable>>)
