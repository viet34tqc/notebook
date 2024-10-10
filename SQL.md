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