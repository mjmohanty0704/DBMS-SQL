# SQL- Structure Query Language
## DDL COMMANDS
## CREATE, DROP, TRUNCATE, ALTER

```SQL
-- the keywords are case insensitive but the user defined names should be lowercase
CREATE DATABASE practice_from_scratch;
CREATE TABLE users(
    user_id INTEGER,
    name VARCHAR(255),
    email VARCHAR(255),
    password VARCHAR(255)
);
INSERT INTO users (user_id, name, email, password) VALUES (3, 'DRM', 'DRM@ggg.com', '112233'), (4, 'ALM', 'ALM@hhh.com', '132435');
TRUNCATE TABLE users;
DROP TABLE IF EXISTS users;
```

# DATA INTEGRITY
Data integrity in databases refers to the accuracy, completeness and consistency of the data stored in a database. It is a measure of the reliability and trustworthiness of the data and ensures that the data in a database is protected from errors, corruption or unauthorized changes.

There are various methods used to ensure data integrity, including: 

**1. Constraints**
    
- Constraints in a database are rules or conditions that must be met for data to be inserted, updated or deleted in a database table. They are used to enforce the integrity of the data stored in a database and to prevent data from becoming inconsistent or corrupted.
- NOT NULL, UNIQUE(COMBO) another way of creating a constraint, PRIMARY KEY, AUTO INCREMENT, CHECK, DEFAULT, FOREIGN KEY


```SQL
  create table customer(
    cid INTEGER PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(50) NOT NULL UNIQUE
);

create table orders(
    oid INTEGER PRIMARY KEY AUTO_INCREMENT,
    cid INTEGER NOT NULL,
    order_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    
    CONSTRAINT orders_fk FOREIGN KEY (cid) REFERENCES customer(cid)
    ON DELETE CASCADE
    ON UPDATE CASCADE
    -- ON DELETE SET NULL
);
-- ADD COLUMNS
alter table customers add column password VARCHAR(50) NOT NULL;

alter table customers add column surname VARCHAR(50) NOT NULL after name;

alter table customers
add column pan_num VARCHAR(10) AFTER surname,
add column joining_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP;

-- DELETE COLUMNS
alter table customers drop column pan_number;

alter table customers 
drop column password,
drop column joining_date;

-- MODIFY COLUMN
alter table customers modify column surname INTEGER;

alter table customers add column age INTEGER NOT NULL;
-- constraints cant be modified; they should be deleted and then added again with the desired modification
alter table customers add constraint customer_age_check CHECK (age > 13);
alter table customers drop constraint customer_age_check
alter table customers add constraint customer_age_check CHECK (age > 6)
```

**2. Transactions**

- It is a sequence of database operations that are treated as a single unit of work.

**3. Normalization**

- is a design technique tjat miniizes data redundancy and ensures data consistency by organizing data into separate tables.

---

## DML COMMANDS
### INSERT, SELECT, UPDATE, DELETE
---

```sql
CREATE DATABASE CX_DBMS;
use CX_DBMS;
CREATE TABLE users(
	uid INTEGER primary key auto_increment,
    name varchar(255) not null,
    email varchar(255) not null unique,
    password varchar(255) not null
);

insert into users
values
(NULL,'manisha','manisha@abc.com','2345'),
(NULL,'dharma','dharma@abc.com','12344'),
(NULL,'asha','asha@abc.com','12354');

select * from users;
```
Given a dataset of smartphones, work on it using DML
```sql
select * from cx_dbms.smartphones where 1;
select model, battery_capacity as mAh, os as operating_system from smartphones;

select model, sqrt(resolution_width * resolution_width + resolution_height* resolution_height)/screen_size as 'ppi' from smartphones;

select model, rating/10 as ratings from smartphones;

select model, 'smartphone' as type from smartphones;

select distinct(brand_name) as all_brands
from smartphones;

select distinct processor_brand as 'all processors'
from smartphones;

select distinct brand_name,processor_brand 
from smartphones;

select * from smartphones
where brand_name = 'apple';

select * from smartphones
where price > 100000;

select * from smartphones
where price > 10000 and price < 20000;

select * from smartphones
where price between 10000 and 20000;

select * from smartphones
where rating > 80 and price < 25000;

select * from smartphones
where brand_name = 'samsung' and ram_capacity > 8;

select * from smartphones
where brand_name = 'samsung' and processor_brand = 'snapdragon';

SELECT distinct brand_name from smartphones
where price > 150000;

select * from smartphones
where processor_brand = 'snapdragon' or processor_brand = 'exynos' or processor_brand = 'bionic';

select * from smartphones
where processor_brand IN ('snapdragon','bionic','exynos','dimensity');

update smartphones
set processor_brand = 'samsung'
where processor_brand = 'exynos';

select * from smartphones
where price > 200000;

delete from smartphones
where primary_camera_rear > 100 and brand_name = 'samsung';
```
### Order Of Execution of SQL Queries

***From -> Join -> Where -> Group By -> Having -> Select -> Distinct -> Order By***
- FRANK JOHN'S WICKED GRAVE HAUNTS SEVERAL DULL OWLS

## TYPES OF FUNCTIONS
### Built-in functions
**1. Scalar functions** - are the functions which gives a value for each selected row.
 
**2. Aggregate functions** - are the functions which gives only an individual value for the whole selected column.

```sql
select min(price) as min_price from smartphones;
select max(price) as max_price from smartphones
where brand_name = 'samsung';

select avg(price) from smartphones
where brand_name = 'xiaomi';

select sum(price) from smartphones
where brand_name = 'samsung';

select count(*) from smartphones
where brand_name = 'xiaomi';

select count(distinct(brand_name)) as num_of_brands from smartphones;

select std(screen_size) from smartphones;

select variance(screen_size) from smartphones;

-- Scalar functions
select abs(price - 100000) as temp from smartphones;

select model, round(sqrt(resolution_width * resolution_width + resolution_height* resolution_height)/screen_size,3) as 'ppi' from smartphones;

select distinct ceil(screen_size) from smartphones;
select distinct floor(screen_size) from smartphones;

select avg(battery_capacity), avg(primary_camera_rear) from smartphones
where price >= 100000;

select model from smartphones
where has_5g = 'True';

select avg(internal_memory) from smartphones
where refresh_rate >= 120 and primary_camera_front >= 20;
```

## GROUPING AND SORTING
```SQL
select model,screen_size from smartphones
where brand_name = 'samsung'
order by screen_size desc
limit 12 ;

SELECT ROW_NUMBER() OVER (ORDER BY screen_size DESC) AS id, model, screen_size
FROM smartphones
WHERE brand_name = 'samsung'
LIMIT 12;

select model, num_front_cameras + num_rear_cameras as total_cameras from smartphones
order by total_cameras desc;

select model, round(sqrt(resolution_width*resolution_width + resolution_height*resolution_height)/screen_size) as ppi from smartphones
order by ppi desc;

select model, battery_capacity 
from smartphones
order by battery_capacity
limit 1,1;

select model,rating from smartphones
where brand_name = 'apple'
order by rating
limit 1;

select model,price from smartphones
order by brand_name asc, price asc;


-- grouping
select brand_name, count(*) as num_phones,
round(avg(price),2) as 'avg price',
max(rating) as 'max rating',
round(avg(screen_size),2) as 'avg screen size',
avg(battery_capacity) as 'avg battery capacity'
from smartphones
group by brand_name
order by num_phones desc
limit 15;

select has_5g,avg(price) as avg_price, avg(rating) as rating
from smartphones
group by has_5g;

select brand_name, processor_brand, count(*) as num_phns, avg(primary_camera_rear) as avg_cam_res 
from smartphones
group by brand_name,processor_brand;

select brand_name, round(avg(price)) as avg_price 
from smartphones
group by brand_name
order by avg_price desc
limit 5;

select brand_name, round(avg(screen_size)) as avg_screen_size
from smartphones
group by brand_name
order by avg_screen_size asc
limit 1;

select brand_name, count(*) as count 
from smartphones
where  has_nfc = 'True' and has_ir_blaster = 'True'
group by brand_name
order by count desc limit 1;

select has_nfc, avg(price) as avg_price
from smartphones
where brand_name = 'samsung'
group by has_nfc;

select model, price
from smartphones
order by price desc limit 1;

-- having
select brand_name, count(*) as 'count', avg(price) as avg_price
from smartphones
group by brand_name
having count > 20
order by avg_price desc;

select brand_name, count(*) as 'count', avg(rating) as avg_rating
from smartphones
group by brand_name
having count > 20
order by avg_rating desc;

select brand_name, avg(ram_capacity) as avg_ram from smartphones
where refresh_rate > 90 and fast_charging_available = 1
group by brand_name
having count(*) > 10
order by avg_ram desc
limit 3;

select brand_name, avg(price) as avg_price
from smartphones
where has_5g = 'True'
group by brand_name
having count(*) > 10 and avg(rating) > 70;

-- IPL DATASET
select batter, sum(batsman_run) as runs
from ipl_ball_by_ball_2008_2022
group by batter
order by runs desc limit 5;

select batter, count(*) as num_sixes
from ipl_ball_by_ball_2008_2022
where batsman_run = 6
group by batter
order by num_sixes desc limit 1,1;

select batter, sum(batsman_run) as score from ipl_ball_by_ball_2008_2022
where batter = 'V Kohli'
group by batter
having score >= 100;

select batter from ipl_ball_by_ball_2008_2022
where ;

select batter, sum(batsman_run) as runs_scored 
from ipl_ball_by_ball_2008_2022
group by batter
having runs_scored >= 100
order by runs_scored desc;


select batter,sum(batsman_run) as runs_scored,count(batsman_run) as balls_faced, 
round((sum(batsman_run)/count(batsman_run) ) * 100,2) as strike_rate
from ipl_ball_by_ball_2008_2022
group by batter
having balls_faced > 1000
order by strike_rate desc limit 5;

```

# JOINS
- In SQL (Structured Query Language), a join is a way to combine data
from two or more database tables based on a related column between
them.
- Joins are used when we want to query information that is distributed
across multiple tables in a database, and the information we need is
not contained in a single table. 
- By joining tables together, we can
create a virtual table that contains all of the information we need for
our query.

Problems
redundancy
update anomaly, delete anomaly etc
memory management
Solved using normalization

## Types of joins
left join, right join, inner join, full outer join, cross join, self join

- Cross Join

    - In SQL, a cross join (also known as a Cartesian product) is a type of join that returns the Cartesian product of the two tables being joined.
    - In other words, it returns all possible combinations of rows from the two tables.
    - Cross joins are not commonly used in practice, but they can be useful in certain scenarios, such as generating test data or exploring all possible combinations of items in a product catalogue.
    - However, it's important to be cautious when using cross joins with large tables, as they can generate a very large result set, which can be resource-intensive and slow to process. 
- Inner Join

    - In SQL, an inner join is a type of join operation that combines data from two or more tables based on a specified condition. 
    
    - The inner join returns only the rows from both tables that satisfy the specified condition, i.e., the matching rows.
    
    - When you perform an inner join on two tables, the result set will only contain rows where there is a match between the joining columns in both tables. If there is no match, then the row will not be included in the result set.

- Left Join

    - A left join, also known as a left outer join, is a type of SQL join operation that returns all the rows from the left table (also known as the "first" table) and matching rows from the right table (also known as the "second" table). 
    
    - If there are no matching rows in the right table, the result will contain NULL values in the columns that come from the right table.
    
    - In other words, a left join combines the rows from both tables based on a common column, but it also includes all the rows from the left table, even if there are no matches in the right table. 
    
    - This is useful when you want to include all the records from the first table, but only some records from the second table.

- Right Join

    - A right join, also known as a right outer join, is a type of join operation in SQL that returns all the rows from the right table and matching rows from the left table. 
    - If there are no matches in the left table, the result will still contain all the rows from the right table, with NULL values for the columns from the left table.

- Full Outer Join

    - A full outer join, sometimes called a full join, is a type of join operation in SQL that returns all matching rows from both the left and right tables, as well as any non-matching rows from either table. 
    - In other words, a full outer join returns all the rows from both tables and matches rows with common values in the specified columns, and fills in NULL values for columns where there is no match.

```sql
-- Tables (assuming these tables already exist in your database)

CREATE TABLE Products (
  ProductID INT PRIMARY KEY,
  Name VARCHAR(255),
  Category VARCHAR(255)
);

CREATE TABLE Orders (
  OrderID INT PRIMARY KEY,
  CustomerID INT,
  ProductID INT,
  FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);

-- Insert sample data (replace with your actual data if needed)

INSERT INTO Products (ProductID, Name, Category)
VALUES (1, 'Laptop', 'Electronics'),
       (2, 'Phone', 'Electronics'),
       (3, 'Headphones', 'Electronics'),
       (4, 'T-Shirt', 'Clothing'),
       (5, 'Jeans', 'Clothing');

INSERT INTO Orders (OrderID, CustomerID, ProductID)
VALUES (100, 1, 1),
       (200, 2, 2),
       (300, 1, 3);

-- Cross Join
SELECT 'Cross Join' AS JoinType, p.Name, o.OrderID
FROM Products p
CROSS JOIN Orders o;

-- Inner Join
SELECT 'Inner Join' AS JoinType, p.Name, o.OrderID
FROM Products p
INNER JOIN Orders o ON p.ProductID = o.ProductID;

-- Right Join
SELECT 'Right Join' AS JoinType, p.Name, o.OrderID
FROM Products p
RIGHT JOIN Orders o ON p.ProductID = o.ProductID;

-- Left Join
SELECT 'Left Join' AS JoinType, p.Name, o.OrderID
FROM Products p
LEFT JOIN Orders o ON p.ProductID = o.ProductID;

-- Full Outer Join
SELECT 'Full Outer Join' AS JoinType, p.Name, o.OrderID
FROM Products p
FULL OUTER JOIN Orders o ON p.ProductID = o.ProductID;

-- Self Join (Example: Find products in the same category)
SELECT 'Self Join' AS JoinType, p1.Name AS Product1, p2.Name AS Product2
FROM Products p1
INNER JOIN Products p2 ON p1.Category = p2.Category AND p1.ProductID <> p2.ProductID;


```