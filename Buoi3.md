# Buổi 3: MySQL Cơ Bản

## A. Các thao tác cơ bản

### I. Lệnh SELECT (truy vấn dữ liệu)

#### 1. Khái niệm

Lệnh **SELECT** được dùng để _lấy dữ liệu_ (retrieve data) từ một hoặc nhiều bảng (_tables_) trong cơ sở dữ liệu (_database_). Đây là câu lệnh được sử dụng nhiều nhất trong SQL.

Cú pháp cơ bản:

```sql
SELECT column1, column2, ...
FROM table_name;
```

- **SELECT**: từ khóa để chọn cột cần lấy.
- **FROM**: xác định bảng (_table_) chứa dữ liệu.

Ví dụ:

```sql
SELECT first_name, last_name
FROM employees;
```

→ Lấy hai cột `first_name` và `last_name` từ bảng `employees`.

#### 2. SELECT \* (lấy tất cả các cột)

Khi bạn muốn xem toàn bộ dữ liệu trong bảng:

```sql
SELECT * FROM employees;
```

Ký tự `*` có nghĩa là “chọn tất cả các cột”.

#### 3. SELECT với điều kiện WHERE

Dùng **WHERE** để lọc (_filter_) các hàng (_rows_) theo điều kiện:

```sql
SELECT *
FROM employees
WHERE department_id = 5;
```

→ Lấy tất cả nhân viên thuộc phòng ban có `department_id = 5`.

#### 4. SELECT với sắp xếp ORDER BY

Dùng **ORDER BY** để sắp xếp kết quả:

```sql
SELECT first_name, salary
FROM employees
ORDER BY salary DESC;
```

→ Sắp xếp nhân viên theo lương giảm dần (_DESC – descending_). Mặc định là tăng dần (_ASC - ascending_)

Nếu sắp xếp theo **nhiều cột**, có thể kết hợp như sau:

```sql
SELECT first_name, department_id, salary
FROM employees
ORDER BY department_id ASC, salary DESC;
```

→ Nghĩa là: Sắp xếp theo phòng ban **tăng dần** và trong mỗi phòng ban (`department_id`), nhân viên được sắp theo lương **giảm dần**.

#### 5. SELECT với giới hạn LIMIT

Dùng **LIMIT** để giới hạn số dòng kết quả (đặc biệt hữu ích khi test):

```sql
SELECT * FROM employees
LIMIT 5;
```

→ Hiển thị 5 dòng đầu tiên.

### II. Lệnh INSERT (thêm dữ liệu)

#### 1. Khái niệm

**INSERT** được dùng để thêm (_insert_) một hoặc nhiều bản ghi (_records/rows_) vào bảng.

Cú pháp:

```sql
# nếu không có column1, column2,... thì sẽ tự động INSERT theo thứ tự các cột trong bảng
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

Ví dụ:

```sql
INSERT INTO employees (first_name, last_name, department_id, salary)
VALUES ('John', 'Doe', 5, 8000);
```

→ Thêm một nhân viên mới vào bảng `employees`.

#### 2. Thêm nhiều bản ghi cùng lúc

```sql
INSERT INTO employees (first_name, last_name, department_id, salary)
VALUES
('Anna', 'Smith', 4, 7200),
('Mark', 'Lee', 4, 7600);
```

### III. Lệnh UPDATE (cập nhật dữ liệu)

#### 1. Khái niệm

**UPDATE** dùng để thay đổi (_update/modify_) dữ liệu trong bảng.

Cú pháp:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

#### 2. Ví dụ

```sql
UPDATE employees
SET salary = 9000
WHERE first_name = 'John';
```

→ Cập nhật mức lương của nhân viên tên John lên 9000.

**Lưu ý:**
Nếu **không có WHERE**, _tất cả các hàng trong bảng sẽ bị cập nhật!_
Ví dụ:

```sql
UPDATE employees SET salary = 9000;
```

→ Tất cả nhân viên đều có lương 9000.

### IV. Lệnh DELETE (xóa dữ liệu)

#### 1. Khái niệm

**DELETE** được dùng để xóa (_delete_) các bản ghi khỏi bảng.

Cú pháp:

```sql
DELETE FROM table_name
WHERE condition;
```

#### 2. Ví dụ

```sql
DELETE FROM employees
WHERE employee_id = 101;
```

→ Xóa nhân viên có ID = 101.

**Lưu ý:**
Nếu bỏ **WHERE**, toàn bộ dữ liệu trong bảng sẽ bị xóa:

```sql
DELETE FROM employees;
```

→ Bảng trống nhưng vẫn tồn tại trong database.

### V. Từ khóa AS (đặt bí danh – alias)

#### 1. Khái niệm

**AS** được dùng để đặt tên tạm thời (_alias_) cho cột hoặc bảng, giúp câu truy vấn ngắn gọn và dễ đọc hơn.

Cú pháp:

```sql
SELECT column_name AS alias_name
FROM table_name AS alias_name;
```

#### 2. Ví dụ đặt bí danh cho cột

```sql
SELECT first_name AS 'Tên', salary AS 'Lương'
FROM employees;
```

→ Kết quả hiển thị với tiêu đề cột “Tên” và “Lương”.

### VI. Từ khóa DISTINCT (loại bỏ trùng lặp)

#### 1. Khái niệm

**DISTINCT** dùng để loại bỏ (_remove duplicates_) các giá trị trùng trong kết quả truy vấn.

Cú pháp:

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

#### 2. Ví dụ

```sql
SELECT DISTINCT department_id
FROM employees;
```

→ Trả về danh sách các phòng ban mà có nhân viên (mỗi `department_id` chỉ xuất hiện một lần).

#### 3. Kết hợp DISTINCT với nhiều cột

```sql
SELECT DISTINCT department_id, job_id
FROM employees;
```

→ Mỗi cặp `department_id` + `job_id` chỉ xuất hiện một lần duy nhất.

Tuyệt vời, Seamus. Đây là phần **B. Lọc dữ liệu**, bao gồm hai mệnh đề cực kỳ quan trọng trong MySQL: **WHERE** (lọc từng dòng trước khi nhóm dữ liệu) và **HAVING** (lọc sau khi dữ liệu đã được nhóm).
Tôi sẽ trình bày chi tiết, có ví dụ rõ ràng theo đúng format bạn yêu cầu (A, I, 1, a...).

---

## B. Lọc dữ liệu

### I. Mệnh đề WHERE (lọc hàng – row filtering)

#### 1. Khái niệm

**WHERE** được dùng để _lọc các bản ghi (rows)_ theo điều kiện nhất định **trước khi** dữ liệu được nhóm hoặc tính toán.
Nói cách khác, **WHERE** quyết định _dòng nào sẽ được đưa vào kết quả truy vấn_.

Cú pháp:

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

#### 2. Ví dụ

```sql
SELECT first_name, salary
FROM employees
WHERE salary > 8000000;
```

→ Lấy danh sách nhân viên có lương lớn hơn 8000000.

#### 3. WHERE với nhiều điều kiện (AND, OR, NOT)

**AND** – Tất cả điều kiện phải đúng
**OR** – Chỉ cần một điều kiện đúng
**NOT** – Phủ định điều kiện

Ví dụ:

```sql
SELECT first_name, department_id, salary
FROM employees
WHERE department_id = 5 AND salary > 8000000;
```

→ Nhân viên ở phòng ban 5 **và** có lương lớn hơn 8000000.

```sql
SELECT first_name, department_id
FROM employees
WHERE department_id = 5 OR department_id = 6;
```

→ Nhân viên thuộc phòng 5 **hoặc** 6.

```sql
SELECT first_name
FROM employees
WHERE NOT department_id = 5;
```

→ Nhân viên **không thuộc** phòng ban 5.

#### 4. WHERE với toán tử so sánh

Các toán tử thường dùng:

| Toán tử    | Ý nghĩa           | Ví dụ             |
| ---------- | ----------------- | ----------------- |
| =          | Bằng              | `salary = 9000`   |
| <> hoặc != | Khác              | `salary <> 9000`  |
| >          | Lớn hơn           | `salary > 8000`   |
| <          | Nhỏ hơn           | `salary < 8000`   |
| >=         | Lớn hơn hoặc bằng | `salary >= 10000` |
| <=         | Nhỏ hơn hoặc bằng | `salary <= 5000`  |

#### 5. WHERE với BETWEEN (trong khoảng)

```sql
SELECT first_name, salary
FROM employees
WHERE salary BETWEEN 6000000 AND 9000000;
```

→ Lấy nhân viên có lương trong đoạn từ 6000000 đến 9000000 (bao gồm cả 2 giá trị đầu–cuối).

#### 6. WHERE với IN (nằm trong tập hợp)

```sql
SELECT first_name, department_id
FROM employees
WHERE department_id IN (1, 3, 5);
```

→ Chỉ lấy nhân viên thuộc phòng ban 1, 3 hoặc 5.

#### 7. WHERE với LIKE (tìm chuỗi tương tự)

**LIKE** dùng để so khớp mẫu (_pattern matching_) với ký tự đại diện:

| Ký tự | Ý nghĩa                               |
| ----- | ------------------------------------- |
| `%`   | Đại diện cho _bất kỳ chuỗi ký tự nào_ |
| `_`   | Đại diện cho _một ký tự bất kỳ_       |

Ví dụ:

```sql
SELECT first_name
FROM employees
WHERE first_name LIKE 'Ch%';
```

→ Tên bắt đầu bằng chữ “Ch”.

```sql
SELECT first_name
FROM employees
WHERE first_name LIKE '_h%';
```

→ Ký tự thứ 2 trong tên là “h”.

#### 8. WHERE với IS NULL / IS NOT NULL

```sql
SELECT first_name
FROM employees
WHERE commission_pct IS NULL;
```

→ Nhân viên **chưa có** phần trăm hoa hồng.

### II. Mệnh đề HAVING (lọc nhóm – group filtering)

#### 1. Khái niệm

**HAVING** được dùng để _lọc kết quả sau khi dữ liệu đã được nhóm (GROUP BY)_.
Trong khi **WHERE** hoạt động _trước khi nhóm_, thì **HAVING** hoạt động _sau khi nhóm_.

Cú pháp:

```sql
SELECT column1, aggregate_function(column2)
FROM table_name
GROUP BY column1
HAVING condition;
```

#### 2. Ví dụ cơ bản

```sql
SELECT department_id, AVG(salary) AS average_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 8000000;
```

→ Chỉ hiển thị những phòng ban có mức lương trung bình lớn hơn 8000.

#### 3. So sánh WHERE và HAVING

| Đặc điểm                                   | WHERE                      | HAVING                         |
| ------------------------------------------ | -------------------------- | ------------------------------ |
| Áp dụng cho                                | Từng dòng dữ liệu (_rows_) | Nhóm dữ liệu (_groups_)        |
| Thời điểm thực thi                         | Trước khi nhóm (GROUP BY)  | Sau khi nhóm (GROUP BY)        |
| Dùng với hàm tổng hợp (aggregate function) | Không                      | Có thể                         |
| Ví dụ                                      | `WHERE salary > 8000000`   | `HAVING AVG(salary) > 8000000` |

#### 4. Kết hợp WHERE + HAVING

Bạn có thể dùng cả hai trong cùng một truy vấn:

```sql
SELECT department_id, COUNT(*) AS total_employees
FROM employees
WHERE salary > 5000
GROUP BY department_id
HAVING COUNT(*) >= 5;
```

→ Lọc nhân viên có lương > 5000, sau đó chỉ hiển thị những phòng ban có ít nhất 5 nhân viên như vậy.

### III. Một số lưu ý khi lọc dữ liệu

#### 1. Thứ tự thực thi của câu lệnh SELECT

Đây là thứ tự thực thi logic trong MySQL (dù khi viết khác thứ tự này):

| Thứ tự thực thi | Mệnh đề  | Chức năng                  |
| --------------- | -------- | -------------------------- |
| 1               | FROM     | Xác định bảng cần truy vấn |
| 2               | WHERE    | Lọc hàng theo điều kiện    |
| 3               | GROUP BY | Nhóm dữ liệu               |
| 4               | HAVING   | Lọc nhóm sau khi nhóm      |
| 5               | SELECT   | Chọn cột hiển thị          |
| 6               | ORDER BY | Sắp xếp kết quả            |
| 7               | LIMIT    | Giới hạn số dòng           |

---

## C. Kết hợp bảng và kết quả

### I. JOIN (kết hợp bảng – table join)

#### 1. Khái niệm

**JOIN** được dùng để _kết hợp dữ liệu từ nhiều bảng_ dựa trên một hoặc nhiều cột có liên quan (thường là khóa chính – _primary key_ và khóa ngoại – _foreign key_).

JOIN là phần cốt lõi của SQL, giúp “nối” logic giữa các bảng, ví dụ:

- Bảng `employees` chứa thông tin nhân viên.
- Bảng `departments` chứa thông tin phòng ban.
  → Dùng JOIN để hiển thị nhân viên cùng tên phòng ban.

Cú pháp tổng quát:

```sql
SELECT columns
FROM table1
JOIN table2
ON table1.column_name = table2.column_name;
```

#### 2. Các loại JOIN trong MySQL

##### a. INNER JOIN (kết hợp giao nhau)

**INNER JOIN** chỉ lấy _những bản ghi có giá trị khớp_ ở cả hai bảng.

```sql
SELECT e.first_name, d.department_name
FROM employees AS e
INNER JOIN departments AS d
ON e.department_id = d.department_id;
```

##### b. LEFT JOIN (hoặc LEFT OUTER JOIN)

**LEFT JOIN** trả về _tất cả bản ghi ở bảng bên trái (table1)_ và chỉ những bản ghi khớp từ bảng bên phải (table2).
Nếu không khớp, MySQL sẽ trả về **NULL** cho cột của bảng bên phải.

```sql
SELECT e.first_name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
ON e.department_id = d.department_id;
```

→ Tất cả nhân viên đều hiển thị, kể cả những người chưa thuộc phòng ban nào (sẽ có `department_name = NULL`).

##### c. RIGHT JOIN (hoặc RIGHT OUTER JOIN)

**RIGHT JOIN** là ngược lại của LEFT JOIN:
Trả về _tất cả bản ghi từ bảng bên phải (table2)_, và chỉ những bản ghi khớp ở bảng bên trái (table1).

```sql
SELECT e.first_name, d.department_name
FROM employees AS e
RIGHT JOIN departments AS d
ON e.department_id = d.department_id;
```

→ Hiển thị tất cả phòng ban, kể cả những phòng chưa có nhân viên (với cột nhân viên = NULL).

##### d. FULL JOIN (hoặc FULL OUTER JOIN)

MySQL **không hỗ trợ trực tiếp** FULL JOIN, nhưng có thể mô phỏng bằng **UNION** giữa LEFT JOIN và RIGHT JOIN:

```sql
SELECT e.first_name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
ON e.department_id = d.department_id

UNION

SELECT e.first_name, d.department_name
FROM employees AS e
RIGHT JOIN departments AS d
ON e.department_id = d.department_id;
```

→ Kết quả bao gồm tất cả nhân viên và tất cả phòng ban (kể cả không khớp).

##### e. CROSS JOIN (tích Descartes)

**CROSS JOIN** kết hợp _mọi bản ghi của bảng 1 với mọi bản ghi của bảng 2_.
Cực kỳ hiếm khi dùng trong thực tế, trừ khi bạn cần sinh ra tất cả các tổ hợp có thể.

```sql
SELECT e.first_name, d.department_name
FROM employees AS e
CROSS JOIN departments AS d;
```

→ Nếu `employees` có 5 dòng và `departments` có 3 dòng, kết quả sẽ có `5 × 3 = 15` dòng.

##### f. SELF JOIN (tự kết nối bảng)

**SELF JOIN** là khi một bảng _tự nối chính nó_ (thường dùng để so sánh dữ liệu trong cùng một bảng, ví dụ quan hệ cấp trên – cấp dưới).

```sql
SELECT e1.first_name AS employee, e2.first_name AS manager
FROM employees AS e1
JOIN employees AS e2
ON e1.manager_id = e2.employee_id;
```

→ Lấy tên nhân viên cùng tên người quản lý của họ trong cùng bảng `employees`.

### II. UNION (kết hợp kết quả – combine result sets)

#### 1. Khái niệm

**UNION** được dùng để _kết hợp kết quả từ nhiều câu SELECT_ thành **một tập kết quả duy nhất**.
Các bảng hoặc truy vấn được gộp phải **có cùng số lượng cột và kiểu dữ liệu tương ứng** hay còn gọi là **khả hợp**.

Cú pháp:

```sql
SELECT column1, column2, ...
FROM table1

UNION

SELECT column1, column2, ...
FROM table2;
```

#### 2. UNION và UNION ALL

| Từ khóa     | Đặc điểm                             | Loại bỏ trùng lặp |
| ----------- | ------------------------------------ | ----------------- |
| `UNION`     | Gộp và **loại bỏ bản ghi trùng lặp** | Có                |
| `UNION ALL` | Gộp và **giữ lại bản ghi trùng lặp** | Không             |

Ví dụ:

```sql
SELECT city FROM customers
UNION
SELECT city FROM suppliers;
```

→ Trả về danh sách các thành phố (city) từ cả hai bảng nhưng **loại bỏ trùng lặp**.

Nếu bạn muốn giữ lại tất cả:

```sql
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;
```

#### 3. Quy tắc khi dùng UNION

- Số lượng cột trong các câu SELECT phải **bằng nhau**.
- Kiểu dữ liệu của từng cột tương ứng phải **tương thích** (ví dụ: INT với INT, VARCHAR với VARCHAR).
- Thứ tự cột trong hai truy vấn phải **giống nhau**.

Ví dụ sai:

```sql
-- Sai: bảng đầu có 2 cột, bảng sau có 3 cột
SELECT id, name FROM employees
UNION
SELECT id, name, salary FROM managers;
```

---

#### 4. Ví dụ thực tế

Giả sử bạn có hai bảng:

```sql
SELECT name, email FROM employees
UNION
SELECT name, email FROM interns;
```

→ Gộp danh sách nhân viên chính thức và thực tập sinh thành một danh sách duy nhất (loại bỏ trùng lặp nếu có).

### III. Kết hợp JOIN và UNION

Có thể kết hợp cả hai để tạo truy vấn phức tạp hơn.
Ví dụ: lấy tất cả nhân viên trong phòng ban **Sales** hoặc **IT**, bao gồm cả thực tập sinh:

```sql
SELECT e.first_name, d.department_name
FROM employees AS e
JOIN departments AS d
ON e.department_id = d.department_id
WHERE d.department_name IN ('Sales', 'IT')

UNION

SELECT i.first_name, d.department_name
FROM interns AS i
JOIN departments AS d
ON i.department_id = d.department_id
WHERE d.department_name IN ('Sales', 'IT');
```

---

## D. Tổng hợp và nhóm dữ liệu

### I. Hàm tổng hợp (Aggregate Functions)

#### 1. Khái niệm

**Hàm tổng hợp (aggregate function)** là các hàm trong SQL dùng để *tính toán giá trị thống kê* dựa trên một nhóm dữ liệu hoặc toàn bộ bảng.
Các hàm này thường đi kèm với **GROUP BY** để tính toán theo nhóm cụ thể (ví dụ: tổng lương theo phòng ban, trung bình điểm theo lớp...).

MySQL hỗ trợ nhiều hàm tổng hợp, phổ biến nhất gồm:

* **COUNT()** – đếm số lượng bản ghi
* **SUM()** – tính tổng
* **AVG()** – tính trung bình
* **MIN()** – giá trị nhỏ nhất
* **MAX()** – giá trị lớn nhất

#### 2. COUNT() – đếm số lượng bản ghi

##### a. Cú pháp

```sql
SELECT COUNT(column_name)
FROM table_name
WHERE condition;
```

##### b. Ví dụ

```sql
SELECT COUNT(*) AS total_employees
FROM employees;
```

→ Trả về tổng số nhân viên trong bảng `employees`.

##### c. COUNT(column_name) vs COUNT(*)

| Cú pháp              | Ý nghĩa                                            | Ghi chú                                   |
| -------------------- | -------------------------------------------------- | ----------------------------------------- |
| `COUNT(*)`           | Đếm **tất cả các dòng**, kể cả có giá trị NULL     | Nhanh hơn trong đa số trường hợp          |
| `COUNT(column_name)` | Chỉ đếm những dòng mà `column_name` **không NULL** | Dùng khi chỉ muốn tính dữ liệu hợp lệ |

Ví dụ:

```sql
SELECT COUNT(commission_pct) AS valid_bonus_count
FROM employees;
```

→ Đếm số nhân viên **có hoa hồng (commission_pct)** khác NULL.

#### 3. SUM() – tính tổng giá trị

##### a. Cú pháp

```sql
SELECT SUM(column_name)
FROM table_name;
```

##### b. Ví dụ

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

→ Tổng tất cả tiền lương của nhân viên.

##### c. SUM kết hợp WHERE

```sql
SELECT SUM(salary) AS total_sales_salary
FROM employees
WHERE department_id = 5;
```

→ Tổng lương của nhân viên trong phòng ban số 5.

#### 4. AVG() – tính trung bình

##### a. Cú pháp

```sql
SELECT AVG(column_name)
FROM table_name;
```

##### b. Ví dụ

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

→ Tính mức lương trung bình của tất cả nhân viên.

##### c. AVG kết hợp điều kiện

```sql
SELECT AVG(salary) AS avg_hr
FROM employees
WHERE department_id = 2;
```

→ Lương trung bình của nhân viên phòng ban 2.

#### 5. MIN() và MAX()

##### a. Ví dụ cơ bản

```sql
SELECT MIN(salary) AS lowest_salary, MAX(salary) AS highest_salary
FROM employees;
```

→ Trả về mức lương nhỏ nhất và lớn nhất trong toàn bộ nhân viên.

##### b. Kết hợp điều kiện

```sql
SELECT MIN(salary) AS min_3, MAX(salary) AS max_3
FROM employees
WHERE department_id = 3;
```

→ Mức lương nhỏ nhất và lớn nhất trong phòng ban id 3.

### II. Nhóm dữ liệu bằng GROUP BY

#### 1. Khái niệm

**GROUP BY** được dùng để *gom nhóm các bản ghi có giá trị giống nhau* trong một hoặc nhiều cột, thường kết hợp với hàm tổng hợp.

Cú pháp:

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

#### 2. Ví dụ cơ bản

```sql
SELECT department_id, COUNT(*) AS total_employees
FROM employees
GROUP BY department_id;
```

→ Đếm số lượng nhân viên trong từng phòng ban.

Kết quả minh họa:

| department_id | total_employees |
| ------------- | --------------- |
| 1             | 4               |
| 2             | 3               |
| 3             | 5               |

#### 3. Kết hợp nhiều hàm tổng hợp

```sql
SELECT department_id,
       COUNT(*) AS total_employees,
       SUM(salary) AS total_salary,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id;
```

→ Mỗi dòng thể hiện thống kê cho từng phòng ban: tổng số nhân viên, tổng lương và lương trung bình.

#### 4. GROUP BY nhiều cột

```sql
SELECT department_id, job_id, COUNT(*) AS total
FROM employees
GROUP BY department_id, job_id;
```

→ Gom nhóm theo *phòng ban và vị trí công việc*, cho biết mỗi nhóm có bao nhiêu người.

#### 5. Kết hợp GROUP BY với HAVING

Như đã học ở phần B

### III. Kết hợp GROUP BY với các mệnh đề khác

#### 1. WHERE + GROUP BY

```sql
SELECT department_id, COUNT(*) AS total
FROM employees
WHERE salary > 5000
GROUP BY department_id;
```

→ Chỉ đếm nhân viên có lương > 5000 trong mỗi phòng ban.

---

#### 2. ORDER BY + GROUP BY

```sql
SELECT department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id
ORDER BY total_salary DESC;
```

→ Sắp xếp các phòng ban theo tổng lương giảm dần.

---

#### 3. WHERE + GROUP BY + HAVING

```sql
SELECT department_id, COUNT(*) AS emp_count, AVG(salary) AS avg_sal
FROM employees
WHERE salary > 5000
GROUP BY department_id
HAVING emp_count > 3;
```

→ Lấy các phòng ban có:

* Nhân viên lương > 5000
* Ít nhất 3 nhân viên như vậy

---

## E. Truy vấn con (Subquery)

### I. Khái niệm

#### 1. Định nghĩa

**Subquery** (còn gọi là *inner query* hoặc *nested query*) là **một truy vấn SQL nằm bên trong một truy vấn khác** (*outer query*).
MySQL sẽ *chạy truy vấn con trước*, sau đó *dùng kết quả của nó* để thực hiện truy vấn chính.

Ví dụ tổng quát:

```sql
SELECT column1
FROM table1
WHERE column2 > (SELECT AVG(column2) FROM table1);
```

→ Ở đây, `(SELECT AVG(column2) FROM table1)` là **truy vấn con**, trả về giá trị trung bình để truy vấn chính so sánh.

#### 2. Phân loại subquery

Dựa theo **số dòng kết quả mà subquery trả về**, có 3 loại chính:

| Loại                | Số kết quả trả về     | Dùng với                          | Ví dụ                               |
| ------------------- | --------------------- | --------------------------------- | ----------------------------------- |
| **Scalar subquery** | 1 giá trị duy nhất    | Toán tử so sánh (=, >, <, …)      | `salary > (SELECT AVG(salary) ...)` |
| **Row subquery**    | 1 dòng (nhiều cột)    | Toán tử tuple                     | `(col1, col2) IN (SELECT ...)`      |
| **Table subquery**  | Nhiều dòng, nhiều cột | Dùng trong `IN`, `EXISTS`, `FROM` | `WHERE dept_id IN (SELECT ...)`     |

### II. Subquery trong mệnh đề WHERE

#### 1. Dùng để lọc dữ liệu theo điều kiện

```sql
SELECT first_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

→ Lấy nhân viên có lương **cao hơn mức trung bình** của toàn bộ nhân viên.

#### 2. Dùng với toán tử IN

```sql
SELECT first_name
FROM employees
WHERE department_id IN (SELECT department_id
                        FROM departments
                        WHERE location_id = 1700);
```

→ Lấy nhân viên làm việc trong **những phòng ban** có `location_id = 1700`.

#### 3. Dùng với toán tử ANY / ALL

| Toán tử | Ý nghĩa                                         | Ví dụ                                                                |
| ------- | ----------------------------------------------- | -------------------------------------------------------------------- |
| **ANY** | So sánh với *một giá trị bất kỳ* trong subquery | `salary > ANY(SELECT salary FROM employees WHERE department_id = 5)` |
| **ALL** | So sánh với *tất cả giá trị* trong subquery     | `salary > ALL(SELECT salary FROM employees WHERE department_id = 5)` |

Ví dụ:

```sql
SELECT first_name, salary
FROM employees
WHERE salary > ALL (SELECT salary
                    FROM employees
                    WHERE department_id = 5);
```

→ Lấy nhân viên có lương **cao hơn mọi người trong phòng ban 5**.

### III. Subquery trong mệnh đề FROM

#### 1. Dùng subquery như bảng tạm (derived table)

Bạn có thể viết subquery trong **FROM** để tạo ra bảng tạm thời (*derived table*) rồi tiếp tục xử lý trên đó.

Ví dụ:

```sql
SELECT department_id, average_salary
FROM (
    SELECT department_id, AVG(salary) AS average_salary
    FROM employees
    GROUP BY department_id
) AS department_summary
WHERE average_salary > 8000;
```

→ Subquery bên trong tạo bảng tạm `department_summary` (gồm phòng ban và lương trung bình).
Truy vấn bên ngoài lọc những phòng có lương trung bình > 8000.

#### 2. Tên bắt buộc của bảng tạm

Lưu ý: Khi dùng subquery trong **FROM**, bạn *bắt buộc phải đặt bí danh (alias)* cho nó:

```sql
... FROM (SELECT ...) AS alias_name
```

Nếu không, MySQL sẽ báo lỗi cú pháp.

### IV. Subquery trong mệnh đề SELECT

#### 1. Dùng để hiển thị giá trị tính toán phụ

```sql
SELECT first_name,
       salary,
       (SELECT AVG(salary) FROM employees) AS avg_salary
FROM employees;
```

→ Với mỗi nhân viên, MySQL hiển thị thêm cột `avg_salary` – giá trị trung bình chung toàn công ty.

**Lưu ý:**
Truy vấn này không hiệu quả nếu bảng lớn, vì subquery trong SELECT được chạy *cho từng dòng*.
→ Chỉ nên dùng trong trường hợp nhỏ hoặc cần minh họa logic.

### V. Subquery có liên kết (Correlated Subquery)

#### 1. Khái niệm

**Correlated subquery** là truy vấn con **phụ thuộc vào dữ liệu của truy vấn cha**.
Điều đó nghĩa là subquery sẽ chạy **lặp lại cho mỗi dòng** của truy vấn bên ngoài.

Ví dụ:

```sql
SELECT e1.first_name, e1.salary
FROM employees AS e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees AS e2
    WHERE e2.department_id = e1.department_id
);
```

→ Với **mỗi nhân viên**, subquery sẽ tính trung bình lương **trong cùng phòng ban**, sau đó so sánh lương cá nhân với mức trung bình đó.
→ Kết quả: nhân viên có lương cao hơn mức trung bình phòng mình.

#### 2. Đặc điểm

| Đặc điểm                            | Mô tả                                   |
| ----------------------------------- | --------------------------------------- |
| Phụ thuộc vào dòng của truy vấn cha | Có                                      |
| Thực thi nhiều lần                  | Có (mỗi dòng một lần)                   |
| Tốc độ                              | Chậm hơn subquery độc lập               |
| Dễ thay thế bằng                    | JOIN hoặc CTE (Common Table Expression) |

#### 3. Ví dụ thực tế

```sql
SELECT e1.first_name, e1.department_id, e1.salary
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id
);
```

→ Lấy danh sách nhân viên có lương cao hơn mức trung bình **trong chính phòng ban của họ**.

### VI. Subquery với EXISTS / NOT EXISTS

#### 1. Khái niệm

**EXISTS** kiểm tra xem subquery có trả về ít nhất *một dòng dữ liệu* hay không.
Nếu **subquery có dữ liệu**, biểu thức trả về **TRUE**.

Cú pháp:

```sql
SELECT column_name
FROM table_name
WHERE EXISTS (SELECT 1 FROM another_table WHERE condition);
```

#### 2. Ví dụ

```sql
SELECT d.department_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

→ Lấy danh sách phòng ban có **ít nhất một nhân viên**.

Tương tự, **NOT EXISTS** lọc ra những phòng ban **chưa có nhân viên**:

```sql
SELECT d.department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

### VII. Subquery lồng nhiều cấp

MySQL cho phép lồng nhiều tầng subquery (tối đa 255 tầng, nhưng thường không ai cần quá 3).

Ví dụ:

```sql
SELECT first_name, salary
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE location_id = (
        SELECT location_id
        FROM locations
        WHERE city = 'New York'
    )
);
```

→ Truy vấn 3 lớp:

1. Tìm `location_id` của thành phố New York.
2. Từ đó lấy `department_id` thuộc location đó.
3. Cuối cùng, lấy danh sách nhân viên trong các phòng đó.

---

Tuyệt đỉnh, Seamus — phần **F. Thứ tự thực thi logic của truy vấn (Logical Query Processing Order)** chính là “bản đồ bí mật” giúp bạn hiểu **tại sao MySQL xử lý câu lệnh SELECT theo cách đó**.
Nhiều người viết SQL giỏi mà vẫn gặp lỗi “Unknown column” hoặc “Invalid use of group function” — nguyên nhân chính là không hiểu rõ *thứ tự MySQL thật sự thực thi các mệnh đề*.

Dưới đây là phần **F**, được trình bày đầy đủ, chi tiết và bám sát thực tế MySQL Workbench theo format bạn yêu cầu.

---

## F. Thứ tự thực thi logic của truy vấn

### I. Khái niệm

#### 1. Định nghĩa

**Thứ tự thực thi logic (logical query processing order)** là *thứ tự MySQL thực sự xử lý dữ liệu bên trong* một câu truy vấn SQL — không phải thứ tự mà ta viết ra.

Khi bạn viết:

```sql
SELECT ...
FROM ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...
LIMIT ...
```

— MySQL **không thực thi từ trên xuống dưới**, mà có **trật tự logic riêng** để đảm bảo dữ liệu được lọc và xử lý đúng cách.

#### 2. Vì sao cần hiểu thứ tự này

* Giúp **hiểu rõ tại sao câu lệnh sai hoặc báo lỗi**.
* Giúp viết **truy vấn phức tạp chính xác hơn**, đặc biệt khi dùng `HAVING`, `GROUP BY`, `JOIN`, `ALIAS`.
* Giúp tối ưu hiệu năng — biết phần nào MySQL xử lý trước để lọc dữ liệu sớm.

### II. Thứ tự thực thi logic chi tiết

#### 1. Bảng tổng quan thứ tự

| Thứ tự | Mệnh đề      | Chức năng                              | Ghi chú                          |
| ------ | ------------ | -------------------------------------- | -------------------------------- |
| 1      | **FROM**     | Xác định bảng dữ liệu nguồn            | Bao gồm JOIN                     |
| 2      | **ON**       | Áp dụng điều kiện nối (JOIN condition) | Lọc dữ liệu giữa các bảng        |
| 3      | **JOIN**     | Thực hiện phép nối giữa các bảng       | Tạo bảng trung gian              |
| 4      | **WHERE**    | Lọc hàng (row filtering)               | Không dùng được với hàm tổng hợp |
| 5      | **GROUP BY** | Nhóm dữ liệu theo cột                  | Chuẩn bị cho hàm tổng hợp        |
| 6      | **HAVING**   | Lọc nhóm (group filtering)             | Dùng được với hàm tổng hợp       |
| 7      | **SELECT**   | Chọn cột hiển thị, tính toán biểu thức | Có thể dùng bí danh (AS)         |
| 8      | **DISTINCT** | Loại bỏ trùng lặp                      | Áp dụng sau SELECT               |
| 9      | **ORDER BY** | Sắp xếp kết quả                        | Có thể dùng alias hoặc hàm       |
| 10     | **LIMIT**    | Giới hạn số dòng trả về                | Xử lý sau cùng                   |

### III. Phân tích từng giai đoạn

#### 1. FROM – chọn nguồn dữ liệu

MySQL **bắt đầu** bằng cách xác định bảng (hoặc các bảng) từ mệnh đề **FROM**.

Ví dụ:

```sql
SELECT * FROM employees;
```

→ Lúc này, hệ thống chưa lọc, chưa nhóm, chưa sắp xếp — chỉ đơn giản “chuẩn bị bảng dữ liệu”.

#### 2. ON và JOIN – kết hợp bảng

Khi có nhiều bảng, MySQL xử lý **ON** để xác định mối liên hệ giữa chúng, rồi **JOIN** để tạo bảng tạm.

```sql
SELECT e.first_name, d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

→ MySQL tạo ra bảng tạm “e+d” chứa tất cả các cột của hai bảng khớp theo điều kiện ON.

#### 3. WHERE – lọc hàng

Sau khi FROM và JOIN hoàn tất, MySQL áp dụng **WHERE** để loại bỏ những dòng không thỏa điều kiện.

```sql
WHERE salary > 8000
```

→ Lúc này dữ liệu vẫn *chưa được nhóm hay tính toán tổng hợp*.

**Không thể** dùng hàm tổng hợp ở WHERE vì nhóm chưa được tạo.
Ví dụ sai:

```sql
WHERE AVG(salary) > 8000   -- lỗi
```

#### 4. GROUP BY – nhóm dữ liệu

Sau khi đã có dữ liệu được lọc, MySQL nhóm dữ liệu theo cột được chỉ định.

```sql
GROUP BY department_id
```

→ Mỗi nhóm giờ trở thành *một đơn vị logic mới*, chuẩn bị cho các hàm như `COUNT()`, `SUM()`, `AVG()`.

#### 5. HAVING – lọc nhóm

**HAVING** chạy sau GROUP BY, vì thế bạn có thể dùng hàm tổng hợp trong nó.

```sql
HAVING AVG(salary) > 8000
```

→ Lọc các nhóm (phòng ban) có lương trung bình lớn hơn 8000.

Khác biệt:

* WHERE lọc **trước khi nhóm**
* HAVING lọc **sau khi nhóm**

#### 6. SELECT – chọn dữ liệu hiển thị

Chỉ đến bước này, MySQL mới **chọn ra các cột để hiển thị** và tính toán các biểu thức hoặc bí danh (alias).

Ví dụ:

```sql
SELECT department_id, AVG(salary) AS avg_salary
```

→ Bây giờ mới xuất hiện cột “avg_salary”.

**Lưu ý:** Bạn không thể dùng alias trong WHERE vì SELECT chạy *sau* WHERE.
Ví dụ:

```sql
SELECT salary AS pay
FROM employees
WHERE pay > 8000;   -- sai vì pay chưa được tạo
```

→ Cần viết:

```sql
WHERE salary > 8000;   -- đúng
```

#### 7. DISTINCT – loại bỏ trùng lặp

Nếu có **DISTINCT**, MySQL sẽ loại bỏ các dòng bị trùng sau khi SELECT đã chạy.

```sql
SELECT DISTINCT department_id
FROM employees;
```

→ Kết quả chỉ chứa mỗi phòng ban một lần duy nhất.

#### 8. ORDER BY – sắp xếp

Sau khi dữ liệu đã hoàn thiện, MySQL sắp xếp theo cột hoặc biểu thức chỉ định.

```sql
ORDER BY avg_salary DESC;
```

→ Có thể dùng alias ở đây vì SELECT đã chạy trước.

#### 9. LIMIT – giới hạn số dòng kết quả

Cuối cùng, MySQL áp dụng **LIMIT** để giới hạn số dòng trả về cho người dùng.

```sql
LIMIT 5;
```

→ Chỉ hiển thị 5 kết quả đầu tiên sau khi sắp xếp.

---

### IV. Ví dụ minh họa tổng hợp

```sql
SELECT department_id, 
       AVG(salary) AS avg_salary
FROM employees
WHERE salary > 5000
GROUP BY department_id
HAVING AVG(salary) > 8000
ORDER BY avg_salary DESC
LIMIT 3;
```

**Thứ tự thực thi thực tế:**

1. **FROM employees** → lấy dữ liệu bảng `employees`
2. **WHERE salary > 5000** → lọc nhân viên lương > 5000
3. **GROUP BY department_id** → nhóm nhân viên theo phòng
4. **HAVING AVG(salary) > 8000** → giữ lại nhóm có trung bình lương > 8000
5. **SELECT ... AVG(salary)** → tính trung bình và tạo alias
6. **ORDER BY avg_salary DESC** → sắp xếp giảm dần
7. **LIMIT 3** → hiển thị 3 phòng ban đầu tiên

### V. Hình dung logic thực thi (diễn giải từng lớp)

```
┌──────────────────────────────┐
│ 7. LIMIT                     │
│ 6. ORDER BY                  │
│ 5. SELECT                    │
│ 4. HAVING                    │
│ 3. GROUP BY                  │
│ 2. WHERE                     │
│ 1. FROM (+JOIN +ON)          │
└──────────────────────────────┘
```

Nếu ví mỗi câu SELECT là một “dây chuyền xử lý dữ liệu”, thì MySQL **bắt đầu từ FROM ở đáy**, rồi “lọc – nhóm – chọn – sắp xếp – cắt bớt” dần lên trên.

### VI. Một số lỗi thường gặp do hiểu sai thứ tự

#### 1. Dùng alias trong WHERE (sai)

```sql
SELECT salary AS pay FROM employees WHERE pay > 5000;  -- lỗi
```

→ `pay` chưa tồn tại ở thời điểm WHERE chạy.

#### 2. Dùng hàm tổng hợp trong WHERE (sai)

```sql
SELECT department_id FROM employees WHERE AVG(salary) > 8000;  -- lỗi
```

→ `AVG()` chỉ tồn tại sau khi GROUP BY chạy.
