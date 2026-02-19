1. 4 데이터의 중복을 최소화하기 위해 비정규화(Denormalization) 원칙을 따른다.
2. 2  DML (Data Manipulation Language)
3. 4 CREATE
4. 1 GRANT, REVOKE 
5. 2 FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY
6. 3 SELECT COUNT(DISTINCT country) FROM users; 
7. 2 SELECT ROUND(AVG(price), 2) FROM products; 
8. 2 SELECT * FROM users WHERE created_at LIKE '2011-01%'; 
9. 2 WHERE price BETWEEN 10 AND 30
10. 1  INNER JOIN 
11. 1 RIGHT JOIN 
12. 4 모두 사용할 수 없다.
13. 1 SELECT 절보다 나중에 실행되어 집계 함수 결과에 조건을 건다. 
14. 2 UNION
15. 2 Scalar Subquery
16. 1 Inline View
17. 2 ALTER TABLE products ADD COLUMN category VARCHAR(50);
18. 2 UPDATE products SET price = 20 WHERE id = 1; 
19. 3 DELETE FROM users WHERE city = 'Seoul'; 
20. 4  COMMIT
21. 3 SELECT COUNT(*) FROM users WHERE country = 'Korea' AND is_marketing_agree = 1; 
22. 1 SELECT * FROM orders WHERE order_date LIKE '2015-12%'; 
23. 2 SELECT COUNT(*) FROM users WHERE country IN ('UK', 'USA');
24. 3 SELECT name FROM products WHERE price = (SELECT MAX(price) FROM products);
25. 3 SELECT city, COUNT(id) FROM users GROUP BY city; 
26. 2 LEFT JOIN과 WHERE 절에 od.order_id IS NULL 
27. 4 CROSS JOIN
28. 4 (SELECT COUNT(*) FROM orders WHERE order_date LIKE '2016-01%') UNION (SELECT COUNT(*) FROM orders WHERE order_date LIKE '2015-08%');
29. 3 HAVING
30. 4 특정 행의 데이터를 삭제하는 것



Part 1: DDL - 데이터베이스 및 테이블 정의 (15문제, 50점)
 

1. CREATE DATABASE sql_finals;
2. USE sql_finals;
3. 
CREATE TABLE users (
	id INT AUTO_INCREMENT PRIMARY KEY,
	created_at VARCHAR(100),
	username VARCHAR(100) NOT NULL,
    phone VARCHAR(100),
    country VARCHAR(50)
);
4. ALTER TABLE users ADD is_marketing_agree INT;
5. ALTER TABLE users RENAME COLUMN phone TO user_phone;
6. ALTER TABLE users DROP COLUMN created_at;
7. 
CREATE TABLE products (
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(255) NOT NULL,
	price DECIMAL(10, 2) NOT NULL,
    discount_price DECIMAL(10, 2) NOT NULL
);
8. ALTER TABLE products MODIFY name VARCHAR(500);
9. 
CREATE TABLE staff (
	id INT AUTO_INCREMENT PRIMARY KEY,
	user_id INT NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	first_name VARCHAR(50) NOT NULL,
	birth_date DATE
);
10. 
CREATE TABLE orders (
	id INT AUTO_INCREMENT PRIMARY KEY,
	user_id INT,
	staff_id INT,
	order_date DATE,
    FOREIGN KEY (user_id) REFERENCES students(id),
    FOREIGN KEY (staff_id) REFERENCES staff(id)
);
11. 
CREATE TABLE orderdetails (
	id INT AUTO_INCREMENT PRIMARY KEY,
	order_id INT,
	product_id INT,
	quantity INT NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
12. ALTER TABLE users DROP COLUMN is_marketing_agree;
13. ALTER TABLE orderdetails RENAME COLUMN quantity TO amount;
14. DROP TABLE orderdetails;
15. DROP DATABASE sql_finals;

Part 2: DML - 데이터 삽입, 수정, 삭제 (15문제, 50점)
 

1. 
ALTER TABLE users ADD city VARCHAR(50);

INSERT INTO users (username,user_phone,city,country)
	VALUES('joejoe@joecompany.com', '010-1234-5678', 'Seoul', 'Korea'),('phk4938@never.com', '010-9876-5432', 'Buenos Aires', 'Argentina');
 
2. INSERT INTO users (username,user_phone,city,country)
	VALUES('inr01@never.com', '010-1111-2222', 'New York', 'USA'),('fuxp76@never.com', '010-3333-4444', 'Seoul', 'Korea');

3. 
INSERT INTO products (name, price, discount_price) VALUES ('Terrarossa Coffee Back', 18.00, 15.00);

4. 
INSERT INTO products (name, price, discount_price) VALUES ('Mini Cheese Ball', 19.00, 19.00),('Stabucks Drip Coffee', 22.00, 22.00);

5. 
INSERT INTO staff (user_id, last_name, first_name,birth_date) VALUES (2, 'Carter','Joe','1990-10-20'),(10,'Jang','Ken','1991-02-19');


6. 
INSERT INTO orders (user_id, staff_id, order_date) VALUES (2, 1, '2025-09-18');


7. 
INSERT INTO orderdetails (order_id, product_id, quantity) VALUES (1, 1, 2),(1, 2, 3);

8. 
UPDATE users
SET user_phone = '010-5555-6666'
WHERE username= 'inr01@never.com';

9. 
UPDATE products
SET price = 20.00
WHERE name = 'Mini Cheese Ball';

10. 
UPDATE staff
SET first_name = 'Minjun'
WHERE user_id = '10';

11. 
DELETE FROM orderdetails WHERE order_id = 1;
DELETE FROM orders WHERE user_id = 2;
DELETE FROM users WHERE username = 'phk4938@never.com';

12. DELETE FROM product WHERE id = 3;

13. DELETE FROM orderdetails WHERE order_id = 1;


14. 
SET FOREIGN_KEY_CHECKS = 0;
TRUNCATE TABLE users;

15.  DELETE FROM orders WHERE order_date = '2025-09-18';




Part 3: DML - 데이터 조회 (20문제, 100점)
 
1. 
SELECT id, username, phone FROM users;

2. 
SELECT COUNT(*) FROM orders WHERE order_date LIKE '2015%';

3. SELECT name FROM products WHERE price = (SELECT MAX(price) FROM products);

4. SELECT id, username FROM users WHERE country = 'Korea'; 

5. 
SELECT last_name, first_name FROM staff WHERE birth_date < '1960-01-01';

6. 
SELECT id, order_date FROM orders WHERE user_id = 20;

7. 
SELECT SUM(quantity) FROM orderdetails WHERE product_id = 17;

8. 
SELECT ROUND(AVG(price), 2) FROM products;

9. 
SELECT country FROM users GROUP BY country HAVING COUNT(country) > 4;


10. 
SELECT staff_id, COUNT(*) AS 주문건수 FROM orders GROUP BY staff_id ORDER BY 주문건수 DESC;

11. 
SELECT u.username, COUNT(o.user_id) AS 주문건수
FROM users u JOIN orders o ON u.id = o.user_id
GROUP BY o.user_id
HAVING 주문건수 > 1;

12. 
SELECT u.id, u.username, s.last_name, s.first_name
FROM users u JOIN staff s ON u.id = s.user_id;

13. 
SELECT od.order_id, SUM(od.quantity) AS 총수량
FROM orders o JOIN orderdetails od ON o.id = od.order_id
GROUP BY od.order_id
HAVING SUM(od.quantity) > 100; 

14. 
SELECT u.country, COUNT(o.user_id)
FROM orders o JOIN users u ON u.id = o.user_id
WHERE u.country = 'USA';

15. 
SELECT COUNT(*)
FROM orders
WHERE staff_id = 3 AND order_date LIKE '2015-12%'; 

16. 
SELECT name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products); 

17. 
SELECT COUNT(id)
FROM users
WHERE city = 'Seoul';

18. 
SELECT order_id, (price * quantity) AS 총금액
FROM orders o JOIN orderdetails od ON o.id = od.order_id JOIN products p ON p.id = od.product_id
WHERE (price * quantity) >= 2000;

19. 
SELECT name, price
FROM products
WHERE name LIKE '%Coffee%';


20. 
SELECT last_name, first_name
FROM staff
WHERE birth_date < '1960-01-01'
UNION
SELECT last_name, first_name
FROM staff
WHERE birth_date > '1990%';

