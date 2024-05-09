# MySQL Advanced

## Creating Tables with Constraints

Constraints are rules enforced on data columns in a table. They ensure data integrity and prevent invalid data from being inserted into the database. Here are some common constraints:

- `PRIMARY KEY`: Uniquely identifies each row in a table.
- `FOREIGN KEY`: Links data across tables, ensuring referential integrity.
- `UNIQUE`: Ensures that all values in a column are unique.
- `NOT NULL`: Ensures that a column cannot have a NULL value.
- `CHECK`: Allows you to specify a condition that each row must satisfy.

Example:

CREATE TABLE users (
id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(50) NOT NULL,
email VARCHAR(50) UNIQUE,
age INT CHECK (age >= 18)
);

## Optimizing Queries with Indexes

Indexes are data structures that store a subset of data from a table to speed up data retrieval. They are crucial for improving query performance, especially on large tables.

Example:

CREATE INDEX idx_users_email ON users (email);

## Stored Procedures and Functions

Stored procedures and functions are code objects that are stored and executed within the database server. They can accept parameters, perform complex logic, and return results.

Stored Procedure Example:

DELIMITER $$
CREATE PROCEDURE get_user_by_email(IN email_param VARCHAR(50))
BEGIN
    SELECT * FROM users WHERE email = email_param;
END$$
DELIMITER ;

Function Example:

DELIMITER $$
CREATE FUNCTION calculate_age(birth_date DATE)
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN YEAR(CURDATE()) - YEAR(birth_date);
END$$
DELIMITER ;

## Views

Views are virtual tables that represent a subset of data from one or more tables. They can simplify complex queries and provide an additional layer of security by restricting access to sensitive data.

Example:

CREATE VIEW active_users AS
SELECT id, name, email
FROM users
WHERE last_login >= DATE_SUB(NOW(), INTERVAL 30 DAY);

## Triggers

Triggers are special types of stored procedures that are automatically executed when specific events occur, such as inserting, updating, or deleting data in a table. They can be used to enforce complex business rules, maintain data integrity, and automate data manipulation tasks.

Example:

DELIMITER $$
CREATE TRIGGER update_last_login
BEFORE UPDATE ON users
FOR EACH ROW
BEGIN
    SET NEW.last_login = NOW();
END$$
DELIMITER ;
