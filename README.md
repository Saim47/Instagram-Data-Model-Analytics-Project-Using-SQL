# Instagram-Data-Model-Analytics-Project-Using-SQL
This project designs an Instagram data model in MYSQL, covering table creation, SQL statements, data insertion, and updates. It includes analytics using aggregation, subqueries, window functions, CTEs and case statements.


# Data Model Design #


We created the following tables to model Instagram data:

### Users Table

| Column        | Type         | Constraints     |
| ------------- | ------------ | --------------- |
| user\_id      | INT      | PRIMARY KEY     |
| name          | VARCHAR(50)  | NOT NULL        |
| email         | VARCHAR(100) | UNIQUE NOT NULL |
| phone\_number | VARCHAR(20)  | UNIQUE          |

### Posts Table
| Column      | Type         | Constraints                                                 |
| ----------- | ------------ | ----------------------------------------------------------- |
| post\_id    | INT      | PRIMARY KEY                                                 |
| user\_id    | INTEGER      | NOT NULL, FOREIGN KEY (user\_id) REFERENCES Users(user\_id) |
| caption     | TEXT         |                                                             |
| image\_url  | VARCHAR(200) |                                                             |
| created\_at | TIMESTAMP    | DEFAULT CURRENT\_TIMESTAMP                                  |

### Comments Table

| Column        | Type      | Constraints                                                 |
| ------------- | --------- | ----------------------------------------------------------- |
| comment\_id   | INT    | PRIMARY KEY                                                 |
| post\_id      | INTEGER   | NOT NULL, FOREIGN KEY (post\_id) REFERENCES Posts(post\_id) |
| user\_id      | INTEGER   | NOT NULL, FOREIGN KEY (user\_id) REFERENCES Users(user\_id) |
| comment\_text | TEXT      | NOT NULL                                                    |
| created\_at   | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP                                  |

### Likes Table

| Column      | Type      | Constraints                                                 |
| ----------- | --------- | ----------------------------------------------------------- |
| like\_id    | INT    | PRIMARY KEY                                                 |
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
## SQL Queries

### Creating Tables

Below is the SQL code to create the tables:

```sql
CREATE TABLE Users (
    user_id INT auto_increment PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(20) UNIQUE
);

CREATE TABLE Posts (
    post_id INT auto_increment PRIMARY KEY,
    user_id INTEGER NOT NULL,
    caption TEXT,
    image_url VARCHAR(200),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

CREATE TABLE Comments (
    comment_id INT auto_increment KEY,
    post_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    comment_text TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES Posts(post_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

CREATE TABLE Likes (
    like_id INT auto_increment KEY,
    post_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES Posts(post_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

CREATE TABLE Followers (
    follower_id INT auto_increment KEY,
    user_id INTEGER NOT NULL,
    follower_user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (follower_user_id) REFERENCES Users(user_id)
);
```


### Inserting Data

```sql
-- Inserting into Users table
INSERT INTO Users (name, email, phone_number)
VALUES
    ('John Smith', 'johnsmith@gmail.com', '1234567890'),
    ('Jane Doe', 'janedoe@yahoo.com', '0987654321'),
    ('Bob Johnson', 'bjohnson@gmail.com', '1112223333'),
    ('Alice Brown', 'abrown@yahoo.com', NULL),
    ('Mike Davis', 'mdavis@gmail.com', '5556667777');

-- Inserting into Posts table
INSERT INTO Posts (user_id, caption, image_url)
VALUES
    (1, 'Beautiful sunset', '<https://www.example.com/sunset.jpg>'),
    (2, 'My new puppy', '<https://www.example.com/puppy.jpg>'),
    (3, 'Delicious pizza', '<https://www.example.com/pizza.jpg>'),
    (4, 'Throwback to my vacation', '<https://www.example.com/vacation.jpg>'),
    (5, 'Amazing concert', '<https://www.example.com/concert.jpg>');
```


### Updating Data

```sql
-- Updating the caption of post_id 3
UPDATE Posts
SET caption = 'Best pizza ever'
WHERE post_id = 3;
```
#
## Analytics Examples

### WHERE Condition

```sql
-- Selecting all the posts where user_id is 1
SELECT *
FROM Posts
WHERE user_id = 1;
```

### ORDER BY

```sql
-- Selecting all the posts and ordering them by created_at in descending order
SELECT *
FROM Posts
ORDER BY created_at DESC;
```

### GROUP BY and HAVING

```sql
-- Counting the number of likes for each post and showing only the posts with more than 2 likes
SELECT Posts.post_id, COUNT(Likes.like_id) AS num_likes
FROM Posts
LEFT JOIN Likes ON Posts.post_id = Likes.post_id
GROUP BY Posts.post_id
HAVING COUNT(Likes.like_id) > 2;
```

### Aggregation Functions

```sql
-- Finding the total number of likes for all posts
SELECT SUM(num_likes) AS total_likes
FROM (
    SELECT COUNT(Likes.like_id) AS num_likes
    FROM Posts
    LEFT JOIN Likes ON Posts.post_id = Likes.post_id
    GROUP BY Posts.post_id
) AS likes_by_post;
```

### Subqueries

```sql
-- Finding all the users who have commented on post_id 1
SELECT name
FROM Users
WHERE user_id IN (
    SELECT user_id
    FROM Comments
    WHERE post_id = 1
);
```

### Window Functions

```sql
-- Ranking the posts based on the number of likes
SELECT post_id, num_likes, RANK() OVER (ORDER BY num_likes DESC) AS rank
FROM (
    SELECT Posts.post_id, COUNT(Likes.like_id) AS num_likes
    FROM Posts
    LEFT JOIN Likes ON Posts.post_id = Likes.post_id
    GROUP BY Posts.post_id
) AS likes_by_post;
```

### Common Table Expressions (CTEs)

```sql
-- Finding all the posts and their comments using a Common Table Expression (CTE)
WITH post_comments AS (
    SELECT Posts.post_id, Posts.caption, Comments.comment_text
    FROM Posts
    LEFT JOIN Comments ON Posts.post_id = Comments.post_id
)
SELECT *
FROM post_comments;
```

### Case Statements

```sql
-- Categorizing the posts based on the number of likes
SELECT
    post_id,
    CASE
        WHEN num_likes = 0 THEN 'No likes'
        WHEN num_likes < 5 THEN 'Few likes'
        WHEN num_likes < 10 THEN 'Some likes'
        ELSE 'Lots of likes'
    END AS like_category
FROM (
    SELECT Posts.post_id, COUNT(Likes.like_id) AS num_likes
    FROM Posts
    LEFT JOIN Likes ON Posts.post_id = Likes.post_id
    GROUP BY Posts.post_id
) AS likes_by_post;
```
![1737012751131](https://github.com/user-attachments/assets/8ce517a3-8c0d-422a-9eaa-aa8b56249ab0)

![image](https://github.com/user-attachments/assets/153be17a-cdd8-4b3a-9431-2c0e54532fb3)

![image](https://github.com/user-attachments/assets/50ae9ebd-80c3-446d-ac40-32c58701cc89)

![image](https://github.com/user-attachments/assets/5c14356f-c1c7-437c-b440-b28b882e504f)

![image](https://github.com/user-attachments/assets/ce7b7d7a-fc1b-475d-b5d3-dd4217869436)

![image](https://github.com/user-attachments/assets/9c77a154-eb3c-4848-8e3d-3ea9a9183d27)

![image](https://github.com/user-attachments/assets/bebec83e-da72-4140-a4c1-407381259b34)

![image](https://github.com/user-attachments/assets/78ee4696-589d-4872-b377-e748b76d6d02)

![image](https://github.com/user-attachments/assets/2afe863d-c29b-4816-9b05-4864c50ef6b6)

![image](https://github.com/user-attachments/assets/10a57ade-3fcd-4bfa-9b1c-3256e7dd9381)

![image](https://github.com/user-attachments/assets/e0fcc8e8-1030-437a-9174-89b85cd834f2)

![image](https://github.com/user-attachments/assets/5d07612d-9647-4194-9ed3-925ead98a7ae)

![image](https://github.com/user-attachments/assets/ed843948-bd89-4cde-8813-b3e06699115b)

![image](https://github.com/user-attachments/assets/051813ef-7761-44b3-9a87-b8dce7274ea3)

![image](https://github.com/user-attachments/assets/f63d8b67-3ca8-4979-a97d-fd61b4903a79)

#
## Conclusion

In this project, we successfully designed and implemented a MYSQL data model for Instagram, covering key SQL concepts and techniques. We demonstrated the creation of various tables that represent Instagram data, including users, posts, comments, likes, and followers. Additionally, we executed several SQL operations such as creating tables, inserting and updating data, and performing complex analytics queries. Here are the key learning outcomes:

1. **Data Modeling with SQL**:  
   - Developed a relational database schema representing Instagram's core functionalities, including users, posts, comments, likes, and followers.
   - Designed tables with appropriate data types, constraints, and foreign key relationships to ensure data integrity.

2. **Advanced SQL Techniques**:  
   - Gained experience with advanced SQL concepts, including aggregation functions, subqueries, window functions, Common Table Expressions (CTEs), and case statements.
   - Learned to write complex queries to extract, manipulate, and analyze data efficiently.

3. **Data Insertion and Updates**:  
   - Learned how to insert large datasets into the database while adhering to data constraints (e.g., unique email addresses and phone numbers).
   - Explored how to update specific records in a table and track changes.

4. **Analytics and Reporting**:  
   - Practiced using SQL for data analysis, including grouping and aggregating data to generate meaningful insights (e.g., counting likes, ranking posts).
   - Applied the `WHERE`, `ORDER BY`, and `HAVING` clauses to filter and organize data for reporting.

5. **Optimizing Queries with Window Functions and CTEs**:  
   - Leveraged window functions to rank data dynamically and used CTEs for cleaner, more readable queries when working with complex joins.

6. **Problem-Solving and SQL Optimization**:  
   - Developed a deeper understanding of how to structure SQL queries for optimal performance and readability while handling large datasets.

Through this project, we have built a solid foundation in SQL and data modeling, which are crucial skills for working with relational databases and performing data analysis in real-world applications.


Gained proficiency in working with date and time functions, such as truncating timestamps and filtering posts based on date ranges (e.g., posts from the last month).
Optimizing Queries with Window Functions and CTEs:

Leveraged window functions to rank data dynamically and used CTEs for cleaner, more readable queries when working with complex joins.
Problem-Solving and SQL Optimization:

Developed a deeper understanding of how to structure SQL queries for optimal performance and readability while handling large datasets.


