# Instagram-Data-Model-Analytics-Project-Using-SQL
This project designs an Instagram data model in MYSQL, covering table creation, SQL statements, data insertion, and updates. It includes analytics using aggregation, subqueries, window functions, CTEs and case statements.


# Data Model Design #


We created the following tables to model Instagram data:

### Users Table

| Column        | Type         | Constraints     |
| ------------- | ------------ | --------------- |
| user\_id      | Int       | PRIMARY KEY     |
| name          | VARCHAR(50)  | NOT NULL        |
| email         | VARCHAR(100) | UNIQUE NOT NULL |
| phone\_number | VARCHAR(20)  | UNIQUE          |

### Posts Table
