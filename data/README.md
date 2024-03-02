# SQL CRUD

```sql

create table restaurants (
    id INTEGER PRIMARY KEY,
    name TEXT,
    category TEXT,
    price tier TEXT,
    opening hour INTEGER,
    closing hour INTEGER,
    average rating INTEGER,
    good for kids TEXT   
   );
```

```sql
.mode csv
.separator ","
.import /Users/zoucory/Desktop/NYU/Senior_Spring/Database_Design_and_Implement/4-sql-crud-Coryzyq/data/restaurants.csv
```