# PART 1 RESTAURANT FINDER

## Creating restaurants and reviews tables
```sql
sqlite3 SQL.db
.mode csv
.separator ","

CREATE TABLE restaurants (
    id INTEGER PRIMARY KEY,
    name TEXT,
    category TEXT,
    price_tier TEXT,
    neighborhoods TEXT,
    opening_hour TEXT,
    closing_hour TEXT,
    average_rating INTEGER,
    good_for_kids TEXT   
   );

CREATE TABLE reviews (
    id INTEGER PRIMARY KEY,
    restaurant_id INTEGER,
    reviews TEXT,
    FOREIGN KEY (restaurant_id) REFERENCES restaurants(id)
);
```
## Importing the csv file into the restaurants table
```sql
.import /Users/zoucory/Desktop/NYU/Senior_Spring/Database_Design_and_Implement/4-sql-crud-Coryzyq/data/restaurants.csv restaurants --skip 1 
```
## Queries
### 1
```sql
SELECT * 
FROM restaurants 
WHERE price_tier = 'cheap' 
AND neighborhoods = 'down town';
```

### 2
```sql
SELECT * 
FROM restaurants 
WHERE category = 'Chinese' 
AND average_rating >= 3
ORDER BY average_rating DESC;
```

### 3
```sql
SELECT * 
FROM restaurants
WHERE opening_hour <= strftime('%H:%M', 'now', 'localtime')
AND closing_hour >= strftime('%H:%M', 'now', 'localtime');
```

### 4
```sql
INSERT INTO reviews (restaurant_id, reviews)
VALUES (746, 'Awful place!');
```

### 5
```sql
DELETE FROM restaurants
WHERE good_for_kids = 'false';
```

### 6
```sql
SELECT neighborhoods, COUNT(name)
FROM restaurants
GROUP BY neighborhoods;
```

# PART 2 SOCIAL MEDIA APP

## Creating Users and Posts tables
```sql
CREATE TABLE users (
    user_id INTEGER PRIMARY KEY,
    email TEXT,
    password TEXT,
    handle TEXT 
);

CREATE TABLE posts (
    posts_id INTEGER PRIMARY KEY,
    user_id INTEGER,
    posts_type TEXT CHECK (posts_type IN ('message', 'story')),
    content TEXT NOT NULL,
    receiver_id INTEGER ,
    time_created TEXT,
    viewed TEXT,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (receiver_id) REFERENCES users(user_id)
);
```
## Importing the csv file into the users and posts table
```sql
.import /Users/zoucory/Desktop/NYU/Senior_Spring/Database_Design_and_Implement/4-sql-crud-Coryzyq/data/users.csv users --skip 1 

.import /Users/zoucory/Desktop/NYU/Senior_Spring/Database_Design_and_Implement/4-sql-crud-Coryzyq/data/posts.csv posts --skip 1
```

## Queries
### 1
```sql
INSERT INTO users (
    email,
    password,
    handle
    )
    VALUES (
        'yz6350@nyu.edu',
        'cory1133',
        'CoryZ'
    );
```

### 2
```sql
INSERT INTO posts (
    user_id,
    posts_type,
    content,
    receiver_id,
    time_created,
    viewed
)
VALUES (
    473,
    'message',
    'hi',
    312,
    DATETIME('now'),
    FALSE
);
```

### 3
```sql
INSERT INTO posts (
    user_id,
    posts_type,
    content,
    receiver_id,
    time_created
)
VALUES (
    473,
    'story',
    'hello world',
    NULL,
    DATETIME('now')
);
```

### 4
```sql
SELECT *
FROM posts
WHERE (posts_type = 'story' AND (ROUND((JULIANDAY('now') - JULIANDAY(time_created)) * 24 > 24)))
OR (posts_type = 'message' AND viewed = FALSE)
ORDER BY datetime(time_created) DESC
LIMIT 10;
```

### 5
```sql
SELECT *
FROM posts
WHERE user_id = 613
AND receiver_id = 523
AND viewed = 'false'
ORDER BY datetime(time_created) DESC;
```

### 6
```sql
UPDATE posts
SET viewed = 'true'
WHERE posts_type = 'story'
AND (JULIANDAY('now') - JULIANDAY(time_created)) * 24 > 24;
```

### 7
```sql
SELECT *
FROM posts
WHERE viewed = 'true'
ORDER BY datetime(time_created) DESC;
```

### 8
```sql
SELECT user_id, COUNT(content)
FROM posts
GROUP BY user_id
ORDER BY COUNT(content) DESC;
```

### 9
```sql
SELECT p.content, u.email
FROM posts p
INNER JOIN users u
ON p.user_id = u. user_id
WHERE (JULIANDAY('now') - JULIANDAY(p.time_created)) * 24 <= 24;
```

### 10
```sql
SELECT u.email
FROM users u
LEFT JOIN posts p
ON u.user_id = p.user_id
WHERE p.posts_id IS NULL;
```

# LINK TO THE PRACTIVE CSV DATA FILES
restaurants.csv: /Users/zoucory/Desktop/NYU/Senior_Spring/Database_Design_and_Implement/4-sql-crud-Coryzyq/data/restaurants.csv

users.csv: /Users/zoucory/Desktop/NYU/Senior_Spring/Database_Design_and_Implement/4-sql-crud-Coryzyq/data/users.csv

posts.csv: /Users/zoucory/Desktop/NYU/Senior_Spring/Database_Design_and_Implement/4-sql-crud-Coryzyq/data/posts.csv