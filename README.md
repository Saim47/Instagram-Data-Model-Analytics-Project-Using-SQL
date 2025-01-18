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
| Column      | Type         | Constraints                                                 |
| ----------- | ------------ | ----------------------------------------------------------- |
| post\_id    | SERIAL       | PRIMARY KEY                                                 |
| user\_id    | INTEGER      | NOT NULL, FOREIGN KEY (user\_id) REFERENCES Users(user\_id) |
| caption     | TEXT         |                                                             |
| image\_url  | VARCHAR(200) |                                                             |
| created\_at | TIMESTAMP    | DEFAULT CURRENT\_TIMESTAMP                                  |

### Comments Table

| Column        | Type      | Constraints                                                 |
| ------------- | --------- | ----------------------------------------------------------- |
| comment\_id   | SERIAL    | PRIMARY KEY                                                 |
| post\_id      | INTEGER   | NOT NULL, FOREIGN KEY (post\_id) REFERENCES Posts(post\_id) |
| user\_id      | INTEGER   | NOT NULL, FOREIGN KEY (user\_id) REFERENCES Users(user\_id) |
| comment\_text | TEXT      | NOT NULL                                                    |
| created\_at   | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP                                  |

### Likes Table

| Column      | Type      | Constraints                                                 |
| ----------- | --------- | ----------------------------------------------------------- |
| like\_id    | SERIAL    | PRIMARY KEY                                                 |
| post\_id    | INTEGER   | NOT NULL, FOREIGN KEY (post\_id) REFERENCES Posts(post\_id) |
| user\_id    | INTEGER   | NOT NULL, FOREIGN KEY (user\_id) REFERENCES Users(user\_id) |
| created\_at | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP                                  |

### Followers Table

| Column             | Type      | Constraints                                                           |
| ------------------ | --------- | --------------------------------------------------------------------- |
| follower\_id       | INTEGER    | PRIMARY KEY                                                           |
| user\_id           | INTEGER   | NOT NULL, FOREIGN KEY (user\_id) REFERENCES Users(user\_id)           |
| follower\_user\_id | INTEGER   | NOT NULL, FOREIGN KEY (follower\_user\_id) REFERENCES Users(user\_id) |
| created\_at        | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP                                            |
#
