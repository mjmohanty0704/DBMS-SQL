# DBMS-SQL
SQL from scratch

# SQL DDL COMMANDS
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
