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
