# SQL- Structure Query Language
# DDL COMMANDS
## CREATE, DROP, TRUNCATE

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
```

**2. Transactions**

- It is a sequence of database operations that are treated as a single unit of work.

**3. Normalization**

- is a design technique tjat miniizes data redundancy and ensures data consistency by organizing data into separate tables.

