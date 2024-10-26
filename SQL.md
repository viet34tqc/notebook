# SQL

## `JOIN` and `INTERCEP`, `UNION`, `EXCEPT`

- `JOIN`: to connect two table
- `INTERCEPT`, `UNION`, `EXCEPT`: connect two queries. the queries result must have the same column structure
	- `INTERCEPT`: the result must match both queries, like `AND` condition
	- `UNION`: the result matches either two queries, like `OR` condition. The result is the combination of both queries
	- `EXCEPT`: the result matches first query, but not the second query.

## `group by`

Bản chất câu lệnh `group by`: nếu trong data trả về có các giá trị trùng lặp thì mình có thể nhóm dữ liệu theo giá trị đó (group by), ví dụ như trùng id.

Đối tượng thứ 2 trong câu lệnh `select` sử dụng `group by` bắt buộc phải phải sử dụng aggregate function (`sum`, `count`). Hình dung câu lệnh group by như sau

`select id, value from table`

trả về

<table>
	<tr>
		<td>1</td>
		<td>1000</td>
	</tr>
	<tr>
		<td>1</td>
		<td>2000</td>
	</tr>
	<tr>
		<td>1</td>
		<td>3000</td>
	</tr>
</table>

`id` = 1 xuất hiện nhiều lần trong kết quả trả về, giờ mình sẽ gom lại theo id bằng câu lệnh: `select id, value from table group by id`

Câu lệnh trên sẽ không chạy, nhưng mình tưởng tưởng nó sẽ thế này:

<table>
	<tr>
		<td>1</td>
		<td>1000, 1000, 1000</td>
	</tr>
</table>

Giá trị `value` trong bảng `table` sẽ được gom lại theo id vào chung 1 ô. Kết quả trả về không thể hiện thị theo dạng `1000, 1000, 1000`  được nên mình phải dùng 1 aggregate function như `sum`. Câu lệnh đúng sẽ là: `select id, sum(value) from table group by id`

Mình cũng có thể đếm số lần xuất hiện của id bằng câu lệnh: `select id, count(id) from table group by id`

## `having` và `where`

Trong câu lệnh sql dùng với aggregate function và `group by`, `where` dùng để lọc data truyền vào aggregate function còn `having` lọc data đầu ra của aggregate function

## VIEW

This is like a table that stores the result of a query, then from where we can make another query

3 types of VIEW

- permanant view: stored in DB

```sql
CREATE VIEW 'avg_book_ratings' AS (
    SELECT "book_id", "title", "year", ROUND(AVG(rating), 2) AS "rating"
    FROM ratings
    JOIN "books" ON ratings.book_id = books.id
    GROUP BY "book_id"
)
SELECT "year", "rating" from "avg_book_ratings" GROUP BY year
```

- temporary view: only exists when we connect to DB

```sql
CREATE TEMPORARY VIEW 'avg_book_ratings' AS (
    SELECT "book_id", "title", "year", ROUND(AVG(rating), 2) AS "rating"
    FROM ratings
    JOIN "books" ON ratings.book_id = books.id
    GROUP BY "book_id"
)
SELECT "year", "rating" from "avg_book_ratings" GROUP BY year
```


- CTE: stands for common table expression, only exists for each query

```sql
WITH 'avg_book_ratings' AS (
    SELECT "book_id", "title", "year", ROUND(AVG(rating), 2) AS "rating"
    FROM ratings
    JOIN "books" ON ratings.book_id = books.id
    GROUP BY "book_id"
)
SELECT "year", "rating" from "avg_book_ratings" GROUP BY year
```

## Trigger

Trigger a query statement when another query is implemented.

In this example, when we run "delete" on 'current_collections', it runs the query in `BEGIN` and `END` blocks, instead of running delete.

`OLD` is the table that is being applied `DELETE`

```sql
CREATE TRIGGER "delete"
INSTEAD OF DELETE ON "current_collections"
FOR EACH ROW
BEGIN
    UPDATE "collections" SET "deleted" = 1
    WHERE "id" = OLD.id;
END;
```

In this example, we add additional condition to run the trigger. `NEW` is the table that is being implemented `INSERT`

```sql
CREATE TRIGGER "insert_when_exists"
INSTEAD OF INSERT ON "current_collections"
FOR EACH ROW WHEN NEW.accession_number IN (SELECT accession_number from collections)
BEGIN
    UPDATE "collections" SET "deleted" = 0
    WHERE accession_number = NEW.accession_number;
END;
```

## INDEX

By default, when we select a piece of data using `where` condition, sql needs to perform a full table scan, row by row to match that query. With `index`, the query is much faster. Here are the reasons:

- The database can go directly to the relevant rows, bypassing the need to scan the entire table.
- Basically, when we index a column, db makes a copy of that column and stores column data in a sorted structure (like a B-tree or hash table). This makes searching for specific values much faster, as it uses algorithms like binary search, reducing the number of comparisons required.

In short, indexes reduce the amount of data the database has to read, sort, and compare, which greatly improves query performance. 

**Trade-off**:

However, it's worth noting that indexes do come with a trade-off in terms of storage space and slower insert/update/delete operations, as the index must be maintained.

```sql
CREATE INDEX idx_name ON table_name (column_name);
```

We can create indexes on multiple tables when we want to speed up the query that has subqueries to others tables

```sql
CREATE INDEX 'person_index' ON "stars" ("person_id");
CREATE INDEX 'name_index' ON "people" ("name")

SELECT "title" FROM "movies" WHERE "id" IN (
    SELECT "movie_id" FROM "stars" WHERE "person_id" = (
        SELECT "id" from people WHERE "name" = "Tom Hanks"
    )
)
```

### Covering index

A covering index is a type of database index that contains all the columns needed to satisfy a particular query, meaning that the database can retrieve all the required data directly from the index without having to look up the actual table rows

Suppose you have a table users with the following columns:

```sql
users (id, first_name, last_name, email, created_at)
```

Now, consider this query:

```sql
SELECT first_name, last_name FROM users WHERE email = 'user@example.com';
```

If you have an index only on the email column, like:

```sql
CREATE INDEX idx_email ON users(email);
```

The database will use the index to quickly find rows matching the email, but then it has to go back to the full table to retrieve the first_name and last_name values.

However, if you create a covering index like this:

```sql
CREATE INDEX idx_covering ON users(email, first_name, last_name);
```

Now, the index contains both the email, first_name, and last_name columns. The database can find everything it needs from the index alone, without having to access the main users table, making the query faster.

## Postgres

### New types

- `TIMESTAMP`: `"datetime" TIMESTAMP DEFAULT now()`

### Custom Type

You can create custom type based on another type

```sql
CREATE `swipe_type` AS ENUM("enter", "exit", "checkout")
 ```
