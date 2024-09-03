# 数据库设计范式：

数据库设计范式是指导数据库设计的一系列规则，旨在减少数据冗余并确保数据的一致性和完整性。常见的数据库设计范式包括第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）等。以下是对这些范式的简要介绍：

### 第一范式（1NF）：

确保每个字段都是原子的，即每个字段不能再分解成更小的数据单元。
每个字段都包含单一值，不允许有重复的组或嵌套的记录。
### 第二范式（2NF）：

在满足1NF的基础上，确保每个非主键字段完全依赖于主键，而不是主键的一部分。
消除部分函数依赖。
### 第三范式（3NF）：

在满足2NF的基础上，确保每个非主键字段不依赖于其他非主键字段。
消除传递函数依赖。
### 巴斯-科德范式（BCNF）：

在满足3NF的基础上，确保每个决定因素都是候选键。
消除任何字段对候选键的非平凡且非函数依赖的关系。
这些范式逐级递进，每一种范式都以前一种范式为基础，进一步减少数据冗余和提高数据一致性。在实际应用中，通常会根据具体需求选择合适的范式，有时为了性能或其他原因，可能会牺牲一些范式要求。

# SQl（ Structured Query Language，结构化查询语言 ）
主要分为以下两种语言：

### 数据定义语言 DDL （ Data Definition Language ）
DDL用于定义数据库的结构，包括创建、修改和删除数据库对象（如表、索引、视图等）。
* 常见的DDL命令包括：
* CREATE：创建数据库对象。
* ALTER：修改数据库对象。
* DROP：删除数据库对象。
* TRUNCATE：清空表中的数据，但保留表结构。
### 数据操作语言（DML，Data Manipulation Language）
DML用于操作数据库中的数据，包括查询、插入、更新和删除数据。
* 常见的DML命令包括：
* SELECT：查询数据。
* INSERT：插入数据。
* UPDATE：更新数据。
* DELETE：删除数据。 

      DDL 和 DML 在日常开发中使用最多的 SQL，
* 此外，还有一些其他的SQL子语言，例如：
  * 数据控制语言（DCL，Data Control Language）：
  DCL用于控制数据库的访问权限和安全性，包括授予和撤销权限。
  常见的DCL命令包括：
  GRANT：授予用户或角色权限。
  REVOKE：撤销用户或角色的权限。
  * 事务控制语言（TCL，Transaction Control Language）：
  TCL用于管理数据库的事务，包括提交和回滚事务。
  常见的TCL命令包括：
  COMMIT：提交事务，使其对数据库的修改永久生效。
  ROLLBACK：回滚事务，撤销对数据库的修改。
  SAVEPOINT：在事务中设置保存点，以便在需要时回滚到该点。

        事物控制语言在日常开发中也会用到，事务控制（TCL）通常
        是由Spring框架的事务管理机制来控制的，主要通过
        @Transactional注解或编程方式来实现。注解方式是最常
        见和简便的方式，而编程方式则提供了更灵活和复杂的事务控制能力。

# CREATE 
CREATE 是数据定义语言（DDL）中的一个关键命令，用于创建数据库对象，如表、视图、索引、存储过程、函数等。以下是 CREATE 命令的详细讲解，包括其语法和常见用法。
### 创建表（CREATE TABLE）
创建表是 CREATE 命令最常见的用法之一。表是数据库中最基本的存储单元，用于存储结构化数据。
* 基础语法

      CREATE TABLE table_name (
      column1 datatype [column_constraint],
      column2 datatype [column_constraint],
      ...
      [table_constraint]
      );

  * 示例

        CREATE TABLE employees (
        id INT PRIMARY KEY,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100) UNIQUE,
        hire_date DATE,
        salary DECIMAL(10, 2)
        );
* 关键点
  * 列定义：每个列需要指定名称和数据类型，还可以添加列级约束（如 PRIMARY KEY, UNIQUE, NOT NULL 等）。
  * 表级约束：可以在表的末尾添加表级约束，如 PRIMARY KEY, FOREIGN KEY, CHECK 等。
  
### 创建索引（CREATE INDEX）
索引用于提高数据库查询的性能。通过在表的列上创建索引，可以加快数据检索速度。
* 基础语法

      CREATE INDEX index_name
      ON table_name (column1, column2, ...);
  * 示例

        CREATE INDEX idx_last_name
        ON employees (last_name);
* 关键点
  * 索引名称：为索引指定一个名称。
  * 表名和列名：指定要在哪个表的哪些列上创建索引

### 创建视图（CREATE VIEW）
视图是基于一个或多个表的虚拟表，其内容由查询定义。视图可以简化复杂查询，并提供数据的安全性和抽象性。
* 基础语法
      
      CREATE VIEW view_name AS
      SELECT column1, column2, ...
      FROM table_name
      WHERE condition;
  * 示例
  
        CREATE VIEW high_salary_employees AS
        SELECT id, first_name, last_name, salary
        FROM employees
        WHERE salary > 5000;
* 关键点
  * 视图名称：为视图指定一个名称。
  * SELECT 语句：定义视图内容的查询语句。


#### 其他 创建存储过程（CREATE PROCEDURE）； 创建函数（CREATE FUNCTION）
存储过程是一组预编译的SQL语句，可以接受参数并返回结果。存储过程可以提高性能并减少网络流量；
函数与存储过程类似，但函数通常返回一个值，并且可以在SQL语句中使用。
这两个东西一般使用较少，一种将数据的处理逻辑保存在数据库的编程模式，我只在银行的封控系统遇到过使用sql编程的模式；

# ALTER
ALTER 是数据定义语言（DDL）中的一个关键命令，用于修改数据库对象的结构。通过 ALTER 命令，可以对已存在的表、索引、
视图等进行修改，如添加、删除或修改列，添加或删除约束等。以下是 ALTER 命令的详细讲解，包括其语法和常见用法。

### 修改表（ALTER TABLE）
修改表是 ALTER 命令最常见的用法之一。可以对表的结构进行多种修改，如添加列、删除列、修改列定义、添加约束等。

* 基础语法

      ALTER TABLE table_name action;

  * 示例
    * 添加列：
  
          ALTER TABLE employees ADD COLUMN phone_number VARCHAR(20);
    这将在 employees 表中添加一个名为 phone_number 的新列，数据类型为 VARCHAR(20)。
    * 删除列
    
          ALTER TABLE employees DROP COLUMN phone_number;
    这将从 employees 表中删除名为 phone_number 的列
    * 修改列定义：
    
          ALTER TABLE employees MODIFY COLUMN first_name VARCHAR(100);
    这将修改 employees 表中 first_name 列的数据类型为 VARCHAR(100)。
    * 添加约束
    
          ALTER TABLE employees ADD CONSTRAINT uc_email UNIQUE (email);
    这将为 employees 表中的 email 列添加一个唯一约束。
    * 删除约束
    
          ALTER TABLE employees DROP CONSTRAINT uc_email;
    这将删除 employees 表中名为 uc_email 的唯一约束。
### 修改视图（ALTER VIEW）
视图是基于一个或多个表的虚拟表，其内容由查询定义。可以通过 ALTER VIEW 命令修改视图的定义。
* 基础语法

      ALTER VIEW view_name AS
      SELECT column1, column2, ...
      FROM table_name
      WHERE condition;
  * 示例

        ALTER VIEW high_salary_employees AS
        SELECT id, first_name, last_name, salary
        FROM employees
        WHERE salary > 6000;
  这将修改 high_salary_employees 视图，使其只包含薪水大于 6000 的员工。
### 修改索引（ALTER INDEX）
虽然 ALTER INDEX 不是所有数据库系统都支持的标准命令，但某些数据库系统（如 Oracle）提供了类似的命令来修改索引。
* 重命名索引

      ALTER INDEX idx_last_name RENAME TO idx_employee_last_name;
这将把名为 idx_last_name 的索引重命名为 idx_employee_last_name。
* MySQL 不直接支持 ALTER INDEX 命令，但可以通过 ALTER TABLE 命令来修改索引。
  * 重命名索引：

        ALTER TABLE employees RENAME INDEX idx_last_name TO idx_employee_last_name;
  * 删除索引：

        ALTER TABLE employees DROP INDEX idx_last_name;

#### ALTER 命令是 DDL 中用于修改数据库对象结构的关键命令。通过 ALTER TABLE, ALTER INDEX, ALTER VIEW 等命令，可以对表、索引、视图等进行多种修改，如添加、删除或修改列，添加或删除约束等。根据实际需求选择合适的命令和参数，以确保数据库结构的正确性和一致性。

# SELECT
SELECT 是数据操作语言（DML）中的一个关键命令，用于从数据库中查询数据。SELECT 语句是 SQL 中最常用和最强大的命令之一，可以用于检索、过滤和组织数据。以下是 SELECT 语句的详细讲解，包括其语法和常见用法。

#### 基础语法

    SELECT column1, column2, ...
    FROM table_name
    WHERE condition;
* 示例：

      SELECT first_name, last_name, email
      FROM employees
      WHERE salary > 5000;
#### 关键点
* 列选择：指定要查询的列，可以使用 * 表示所有列。
* 表选择：指定要查询的表。
* 条件过滤：使用 WHERE 子句来过滤数据。
### 常用子句和关键字

#### 1. SELECT DISTINCT
用于返回唯一的结果，去除重复行。
        
    SELECT DISTINCT department_id
    FROM employees;
#### 2. WHERE 子句
用于过滤数据，只返回满足条件的数据。

    SELECT first_name, last_name
    FROM employees
    WHERE department_id = 10;
#### 3. ORDER BY 子句
用于对结果进行排序，可以按升序（ASC）或降序（DESC）排序。

    SELECT first_name, last_name, salary
    FROM employees
    ORDER BY salary DESC;
#### 4. GROUP BY 子句
用于对结果进行分组，常与聚合函数一起使用。

    SELECT department_id, AVG(salary)
    FROM employees
    GROUP BY department_id;

#### 5. HAVING 子句
用于对分组后的结果进行过滤。

    SELECT department_id, AVG(salary)
    FROM employees
    GROUP BY department_id
    HAVING AVG(salary) > 5000;
#### 6. JOIN 子句
用于连接多个表，基于某些条件合并数据。

    SELECT e.first_name, e.last_name, d.department_name
    FROM employees e
    JOIN departments d ON e.department_id = d.department_id;
#### 7. UNION 和 UNION ALL
用于合并多个 SELECT 语句的结果，UNION 去除重复行，UNION ALL 保留重复行。

    SELECT first_name, last_name
    FROM employees
    UNION
    SELECT first_name, last_name
    FROM contractors;
#### 8. LIMIT 和 OFFSET
用于限制返回的行数，常用于分页查询。

    SELECT first_name, last_name
    FROM employees
    LIMIT 10 OFFSET 20;
### 聚合函数
聚合函数用于对一组值进行计算，并返回单个值。常见的聚合函数包括：
* COUNT()：计算行数。
* SUM()：计算总和。
* AVG()：计算平均值。
* MAX()：计算最大值。
* MIN()：计算最小值。

      SELECT COUNT(*), AVG(salary), MAX(salary), MIN(salary)
      FROM employees;
### 子查询
子查询是嵌套在另一个查询中的查询，可以用于复杂的数据检索和过滤。

    SELECT first_name, last_name
    FROM employees
    WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'Sales');
# INSERT
INSERT 是数据操作语言（DML）中的一个关键命令，用于向数据库表中插入新数据。INSERT 语句可以插入单行数据，也可以插入多行数据。以下是 INSERT 语句的详细讲解，包括其语法和常见用法
### 基本语法

#### 插入单行数据

    INSERT INTO table_name (column1, column2, column3, ...)
    VALUES (value1, value2, value3, ...);
#### 插入多行数据

    INSERT INTO table_name (column1, column2, column3, ...)
    VALUES (value1, value2, value3, ...),
    (value4, value5, value6, ...),
    (value7, value8, value9, ...);
* 示例
  * 插入单行数据

        INSERT INTO employees (id, first_name, last_name, email, hire_date, salary)
        VALUES (1, 'John', 'Doe', 'john.doe@example.com', '2020-01-01', 5000);
  * 插入多行数据

        INSERT INTO employees (id, first_name, last_name, email, hire_date, salary)
        VALUES (2, 'Jane', 'Smith', 'jane.smith@example.com', '2020-02-01', 6000),
        (3, 'Alice', 'Johnson', 'alice.johnson@example.com', '2020-03-01', 5500),
        (4, 'Bob', 'Brown', 'bob.brown@example.com', '2020-04-01', 7000);
* 关键点
  * 表名：指定要插入数据的表。
  * 列名：指定要插入数据的列，可以省略，但需要确保值的顺序与表定义的列顺序一致。
  * 值：指定要插入的数据，必须与列的数量和数据类型匹配。
#### 插入默认值
如果某些列有默认值，可以使用 DEFAULT 关键字插入默认值。

    INSERT INTO employees (id, first_name, last_name, email, hire_date, salary)
    VALUES (5, 'Charlie', 'Davis', 'charlie.davis@example.com', DEFAULT, 5200);
#### 从其他表插入数据
可以使用 INSERT INTO ... SELECT 语句从其他表中选择数据并插入到目标表中。

    INSERT INTO employees (id, first_name, last_name, email, hire_date, salary)
    SELECT id, first_name, last_name, email, hire_date, salary
    FROM temp_employees
    WHERE hire_date > '2020-01-01';
# UPDATE
UPDATE 是数据操作语言（DML）中的一个关键命令，用于修改数据库表中已有的数据。UPDATE 语句可以根据指定的条件更新一个或多个列的值。以下是 UPDATE 语句的详细讲解，包括其语法和常见用法。

### 基本语法

    UPDATE table_name
    SET column1 = value1, column2 = value2, ...
    WHERE condition;
#### 示例
##### 更新单个列

    UPDATE employees
    SET salary = 5500
    WHERE id = 1;
这会将 employees 表中 id 为 1 的员工的薪水更新为 5500。
##### 更新多个列

    UPDATE employees
    SET first_name = 'John', last_name = 'Smith'
    WHERE id = 1;
这会将 employees 表中 id 为 1 的员工的名字和姓氏分别更新为 'John' 和 'Smith'。
#### 关键点
* 表名：指定要更新数据的表。
* SET 子句：指定要更新的列及其新值。
* WHERE 子句：指定更新数据的条件，如果不使用 WHERE 子句，则表中所有行的指定列都会被更新。
### 使用子查询更新数据
可以使用子查询来获取要更新的值。

    UPDATE employees
    SET salary = (SELECT salary FROM temp_employees WHERE temp_employees.id = employees.id)
    WHERE EXISTS (SELECT 1 FROM temp_employees WHERE temp_employees.id = employees.id);
这会将 employees 表中员工的薪水更新为 temp_employees 表中相同 id 的员工的薪水。
### 更新时使用计算表达式
可以在 SET 子句中使用计算表达式来更新数据。

    UPDATE employees
    SET salary = salary * 1.1
    WHERE department_id = 10;
这会将 employees 表中 department_id 为 10 的员工的薪水增加 10%。
### 更新时使用默认值
可以使用 DEFAULT 关键字将列的值更新为默认值。

    UPDATE employees
    SET hire_date = DEFAULT
    WHERE id = 1;
这会将 employees 表中 id 为 1 的员工的雇佣日期更新为默认值（如果有定义默认值）。
### 批量更新
如果不使用 WHERE 子句，则表中所有行的指定列都会被更新。

    UPDATE employees
    SET salary = 5000;
这会将 employees 表中所有员工的薪水更新为 5000。

# DELETE
DELETE 是数据操作语言（DML）中的一个关键命令，用于从数据库表中删除数据。DELETE 语句可以根据指定的条件删除一个或多个行。以下是 DELETE 语句的详细讲解，包括其语法和常见用法。
### 基本语法

    DELETE FROM table_name
    WHERE condition;
#### 示例
##### 删除单行数据

    DELETE FROM employees
    WHERE id = 1;
这会从 employees 表中删除 id 为 1 的员工记录。
##### 删除多行数据
    DELETE FROM employees
    WHERE department_id = 10;
这会从 employees 表中删除 department_id 为 10 的所有员工记录
#### 关键点
* 表名：指定要删除数据的表。
* WHERE 子句：指定删除数据的条件，如果不使用 WHERE 子句，则表中所有行都会被删除
### 使用子查询删除数据
可以使用子查询来指定要删除的行。

    DELETE FROM employees
    WHERE id IN (SELECT id FROM temp_employees);
这会从 employees 表中删除 id 在 temp_employees 表中存在的所有员工记录。
### 删除所有数据
如果不使用 WHERE 子句，则表中所有行都会被删除。

    DELETE FROM employees;
### 删除时使用 LIMIT
某些数据库系统（如 MySQL）支持在 DELETE 语句中使用 LIMIT 子句来限制删除的行数。

    DELETE FROM employees
    WHERE department_id = 10
    LIMIT 10;
这会从 employees 表中删除 department_id 为 10 的前 10 条员工记录。
### 删除时使用 ORDER BY
某些数据库系统（如 MySQL）支持在 DELETE 语句中使用 ORDER BY 子句来指定删除行的顺序。

    DELETE FROM employees
    WHERE department_id = 10
    ORDER BY hire_date
    LIMIT 10;
这会从 employees 表中删除 department_id 为 10 的按雇佣日期排序的前 10 条员工记录。
# 函数篇
在 SQL 中，除了聚合函数（如 COUNT, SUM, AVG, MAX, MIN 等），还有许多其他类型的函数，用于执行各种数据操作和转换。以下是一些常见的 SQL 函数类别及其示例：
### 1. 字符串函数
字符串函数用于处理和操作字符串数据。
* CONCAT(str1, str2, ...)：连接两个或多个字符串。

      SELECT CONCAT('Hello', ' ', 'World') AS greeting;
* SUBSTRING(str, start, length)：从字符串中提取子字符串。

      SELECT SUBSTRING('Hello World', 7, 5) AS result;
* LENGTH(str)：返回字符串的长度。

      SELECT LENGTH('Hello World') AS length;
* UPPER(str)：将字符串转换为大写。

      SELECT UPPER('Hello World') AS upper_case;
* LOWER(str)：将字符串转换为小写。

      SELECT LOWER('Hello World') AS lower_case;
### 2. 数值函数
数值函数用于处理和操作数值数据。
* ABS(num)：返回数值的绝对值。

      SELECT ABS(-10) AS absolute_value;
* ROUND(num, decimals)：将数值四舍五入到指定的小数位数。

      SELECT ROUND(3.14159, 2) AS rounded_value;
* CEIL(num)：返回大于或等于指定数值的最小整数。

      SELECT CEIL(3.14) AS ceiling_value;
* FLOOR(num)：返回小于或等于指定数值的最大整数。

      SELECT FLOOR(3.14) AS floor_value;
### 3. 日期和时间函数
日期和时间函数用于处理和操作日期和时间数据。
* NOW()：返回当前日期和时间。

      SELECT NOW() AS current_datetime;
* DATE(datetime)：从日期时间值中提取日期部分。

      SELECT DATE('2023-10-01 12:34:56') AS date_part;
* YEAR(date)：从日期值中提取年份。

      SELECT YEAR('2023-10-01') AS year_part;
* MONTH(date)：从日期值中提取月份。

      SELECT MONTH('2023-10-01') AS month_part;
* DAY(date)：从日期值中提取日。

      SELECT DAY('2023-10-01') AS day_part;
### 4. 转换函数
转换函数用于在不同数据类型之间进行转换
* CAST(value AS type)：将值转换为指定的数据类型。

      SELECT CAST('123' AS INTEGER) AS integer_value;
* CONVERT(value, type)：将值转换为指定的数据类型（某些数据库系统支持）。

      SELECT CONVERT('123', INTEGER) AS integer_value;
### 5. 条件函数
条件函数用于在 SQL 语句中实现条件逻辑。
* CASE：根据条件返回不同的值。

      SELECT 
          CASE
            WHEN salary > 5000 THEN 'High'
            WHEN salary > 3000 THEN 'Medium'
            ELSE 'Low'
          END AS salary_level
      FROM employees;
* IFNULL(value, default)：如果值为 NULL，则返回默认值。

      SELECT IFNULL(commission_pct, 0) AS commission
      FROM employees;
### 6.系统函数
系统函数用于获取数据库系统的信息。
* USER()：返回当前数据库用户。

      SELECT USER() AS current_user;
* DATABASE()：返回当前数据库名称。

      SELECT DATABASE() AS current_database;
### 7.其他函数
还有一些其他类型的函数，具体取决于数据库系统的支持。
* COALESCE(value1, value2, ...)：返回第一个非 NULL 的值。

      SELECT COALESCE(phone_number, 'N/A') AS phone
      FROM employees;
* NULLIF(value1, value2)：如果两个值相等，则返回 NULL，否则返回第一个值。

      SELECT NULLIF(department_id, 10) AS dept_id
      FROM employees;
### 8.特别说明，时间格式化函数
在SQL中，用于时间格式化的函数可以根据具体数据库系统有所不同。以下是一些常用数据库中的时间格式化函数及其用法：
#### MySQL
在MySQL中，DATE_FORMAT函数用于格式化日期和时间。

      SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s') AS formatted_datetime;
常见的日期格式化符号：
* %Y：四位数的年份。
* %m：月份（01 到 12）。
* %d：月份中的天数（01 到 31）。
* %H：小时（00 到 23）。
* %i：分钟（00 到 59）。
* %s：秒（00 到 59）。
#### PostgreSQL
在PostgreSQL中，TO_CHAR函数用于格式化日期和时间。

      SELECT TO_CHAR(NOW(), 'YYYY-MM-DD HH24:MI:SS') AS formatted_datetime;
常见的日期格式化符号：
* YYYY：四位数的年份。
* MM：月份（01 到 12）。
* DD：月份中的天数（01 到 31）。
* HH24：24小时制的小时（00 到 23）。
* MI：分钟（00 到 59）。
* SS：秒（00 到 59）。
#### SQL Server
在SQL Server中，可以使用 FORMAT 函数来格式化日期和时间。

      SELECT FORMAT(GETDATE(), 'yyyy-MM-dd HH:mm:ss') AS formatted_datetime;
常见的日期格式化符号：
* yyyy：四位数的年份。
* MM：月份（01 到 12）。
* dd：月份中的天数（01 到 31）。
* HH：小时（00 到 23）。
* mm：分钟（00 到 59）。
* ss：秒（00 到 59）。
#### Oracle
在Oracle中，TO_CHAR函数用于格式化日期和时间。

      SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') AS formatted_datetime FROM dual;
常见的日期格式化符号：
* YYYY：四位数的年份。
* MM：月份（01 到 12）。
* DD：月份中的天数（01 到 31）。
* HH24：24小时制的小时（00 到 23）。
* MI：分钟（00 到 59）。
* SS：秒（00 到 59）。
### 注意事项
* 格式化符号在不同的数据库系统中可能略有不同，使用时请参考具体数据库系统的文档。
* 格式化输出通常是字符串类型，因此在进行日期运算时应考虑转换回日期时间类型。

## 自增主键
在 SQL 中，自增主键（Auto-Increment Primary Key）是一种常见的机制，用于在插入新记录时自动生成唯一的标识符。自增主键通常用于确保每条记录都有一个唯一的主键值，从而简化数据管理和查询。
### 自增主键的实现
不同的数据库系统实现自增主键的方式略有不同，以下是一些常见数据库系统的实现方法：
#### MySQL
在 MySQL 中，可以使用 AUTO_INCREMENT 关键字来定义自增主键。

      CREATE TABLE employees (
        id INT AUTO_INCREMENT PRIMARY KEY,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100)
      );
插入数据时，不需要为 id 列指定值，MySQL 会自动生成并插入一个唯一的整数值。

      INSERT INTO employees (first_name, last_name, email)
      VALUES ('John', 'Doe', 'john.doe@example.com');
#### PostgreSQL
在 PostgreSQL 中，可以使用 SERIAL 数据类型来定义自增主键。

      CREATE TABLE employees (
        id SERIAL PRIMARY KEY,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100)
      );
插入数据时，不需要为 id 列指定值，PostgreSQL 会自动生成并插入一个唯一的整数值。

      INSERT INTO employees (first_name, last_name, email)
      VALUES ('John', 'Doe', 'john.doe@example.com');
#### SQL Server
在 SQL Server 中，可以使用 IDENTITY 关键字来定义自增主键。

      CREATE TABLE employees (
        id INT IDENTITY(1,1) PRIMARY KEY,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100)
      );
插入数据时，不需要为 id 列指定值，SQL Server 会自动生成并插入一个唯一的整数值。

      INSERT INTO employees (first_name, last_name, email)
      VALUES ('John', 'Doe', 'john.doe@example.com');
#### Oracle
在 Oracle 中，可以使用序列（Sequence）和触发器（Trigger）来实现自增主键。
##### 首先，创建一个序列：
      
      CREATE SEQUENCE employees_seq START WITH 1 INCREMENT BY 1;
##### 然后，创建一个触发器：

      CREATE OR REPLACE TRIGGER employees_bir 
      BEFORE INSERT ON employees
      FOR EACH ROW
      BEGIN
        SELECT employees_seq.NEXTVAL INTO :NEW.id FROM DUAL;
      END;
##### 最后，创建表：

      CREATE TABLE employees (
        id NUMBER PRIMARY KEY,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100)
      );
插入数据时，不需要为 id 列指定值，Oracle 会通过触发器自动生成并插入一个唯一的整数值。

      INSERT INTO employees (first_name, last_name, email)
      VALUES ('John', 'Doe', 'john.doe@example.com');
### 自增主键的优点
* 唯一性：确保每条记录都有一个唯一的主键值。
* 简化插入操作：插入数据时不需要手动指定主键值，简化了插入操作。
* 高效性：自动生成的整数主键通常比手动生成的字符串主键更高效。
### 自增主键的缺点
* 可预测性：自增主键的值是连续的整数，可能会泄露数据量信息。
* 分布式系统中的问题：在分布式系统中，自增主键可能会导致主键冲突问题。
# SQL 调优
SQL调优是一个复杂的过程，涉及到多个方面，包括查询重写、索引优化、数据库配置调整等。以下是一些常见的SQL调优技巧：
### 1. 查询重写
* **避免使用SELECT ***：只选择需要的列，减少不必要的数据传输。
* 使用JOIN代替子查询：子查询可能会导致性能问题，尽量使用JOIN来优化。
* 避免在WHERE子句中使用函数：这会导致索引失效，尽量在查询前处理数据
### 2. 索引优化
* 创建合适的索引：为经常查询的列创建索引，特别是WHERE、JOIN和ORDER BY子句中使用的列。
* 复合索引：对于多个列的查询，考虑创建复合索引。
* 避免过度索引：过多的索引会增加写操作的成本，定期检查并删除不必要的索引。
### 3. 数据库配置调整
* 调整缓冲区大小：增加数据库缓冲区大小，减少磁盘I/O。
* 调整连接数：根据实际需求调整数据库的最大连接数。
* 启用查询缓存：对于频繁执行的查询，启用查询缓存可以提高性能。
### 4. 分析和优化执行计划
* 使用EXPLAIN命令：分析查询的执行计划，找出性能瓶颈。
* 优化表结构：合理设计表结构，避免冗余数据和复杂的表关系。
### 5. 定期维护
* 定期更新统计信息：数据库统计信息对优化器选择执行计划至关重要。
* 定期清理无用数据：删除不再需要的数据，减少表的大小。
##### 示例
假设有一个查询如下：

      SELECT * FROM users WHERE age > 30;
可以进行以下优化：
* 创建索引：

      CREATE INDEX idx_age ON users(age);
* **避免使用SELECT ***：

      SELECT id, name, age FROM users WHERE age > 30;
通过这些方法，可以显著提高SQL查询的性能。具体的调优方法需要根据实际情况进行调整。
# EXPLAIN
EXPLAIN 是 SQL 数据库中用于分析查询执行计划的关键命令。通过 EXPLAIN，你可以了解数据库引擎是如何执行你的查询的，从而找出潜在的性能瓶颈并进行优化。以下是 EXPLAIN 的详细说明和使用方法：
### 基本语法

      EXPLAIN [FORMAT=JSON] SELECT ...;
* FORMAT=JSON：可选参数，以 JSON 格式输出执行计划，更详细和结构化。
### 输出字段解释
EXPLAIN 的输出字段可能因数据库系统而异，以下是 MySQL 的常见字段解释：
* id：查询的标识符，表示查询的顺序。
* select_type：查询的类型，例如 SIMPLE（简单查询）、PRIMARY（最外层查询）、SUBQUERY（子查询）等。
* table：涉及的表名。
* partitions：涉及的分区（如果启用了分区）。
* type：访问类型，例如 ALL（全表扫描）、index（索引扫描）、range（范围扫描）、ref（非唯一索引扫描）、eq_ref（唯一索引扫描）、const（常量查询）等。
* possible_keys：可能使用的索引。
* key：实际使用的索引。
* key_len：使用的索引长度。
* ref：与索引比较的列或常量。
* rows：估计要扫描的行数。
* filtered：按表条件过滤后剩余的行数的百分比。
* Extra：附加信息，例如 Using where（使用 WHERE 子句）、Using index（使用覆盖索引）、Using temporary（使用临时表）、Using filesort（使用文件排序）等。
##### 示例分析
假设有一个查询如下：

        EXPLAIN SELECT * FROM users WHERE age > 30;
##### 输出示例：
`+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | users | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 1000 |    33.33 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
`
##### 分析：
* type=ALL：表示进行了全表扫描，性能较差。
* rows=1000：估计要扫描 1000 行。
* Extra=Using where：表示使用了 WHERE 子句进行过滤。
#### 优化建议
* 创建索引：为 age 列创建索引，避免全表扫描。

`CREATE INDEX idx_age ON users(age);
`
* 重新执行 EXPLAIN：

`EXPLAIN SELECT * FROM users WHERE age > 30;`
##### 优化后的输出示例：

`+----+-------------+-------+------------+-------+---------------+--------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key    | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+--------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | users | NULL       | range | idx_age       | idx_age| 4       | NULL | 333  |   100.00 | Using index condition |
+----+-------------+-------+------------+-------+---------------+--------+---------+------+------+----------+-------------+
`
##### 分析
* type=range：表示使用了范围扫描，性能较好。
* key=idx_age：表示使用了 idx_age 索引。
* rows=333：估计要扫描 333 行，减少了扫描的行数。

通过 EXPLAIN 分析查询执行计划，可以有效地找出查询的性能瓶颈并进行优化。具体的优化方法需要根据实际情况进行调整。
# 索引失效篇
在SQL查询中，索引是提高查询性能的重要工具，但某些情况下索引可能会失效，导致查询性能下降。以下是一些常见的导致索引失效的情况：
### 1. 使用函数或表达式
在WHERE子句中对索引列使用函数或表达式会导致索引失效。
#### 示例：

      SELECT * FROM users WHERE YEAR(birthdate) = 1990;
在这个查询中，YEAR(birthdate) 使用了函数，导致 birthdate 列上的索引无法使用。
### 2. 类型不匹配
查询条件中的数据类型与索引列的数据类型不匹配也会导致索引失效。
#### 示例：

      SELECT * FROM users WHERE age = '30';
在这个查询中，age 列是整数类型，而查询条件中的 '30' 是字符串类型，导致索引失效。
### 3. 使用LIKE进行前缀模糊查询
在LIKE查询中，如果通配符 % 或 _ 出现在字符串的前面，索引可能会失效。

#### 示例：

      SELECT * FROM users WHERE name LIKE '%john';
在这个查询中，% 出现在 name 列的前面，导致索引失效。
### 4. 使用OR条件
在WHERE子句中使用OR条件，且OR的各个条件不能都使用索引时，索引可能会失效。
#### 示例：

      SELECT * FROM users WHERE age = 30 OR city = 'New York';
在这个查询中，如果 age 和 city 列上都有索引，但OR条件可能导致索引失效。
### 5. 数据类型隐式转换
数据库在进行数据类型隐式转换时，可能会导致索引失效。
#### 示例：

      SELECT * FROM users WHERE age = '30';
在这个查询中，age 列是整数类型，而查询条件中的 '30' 是字符串类型，导致隐式转换，索引失效。
### 6. 使用NOT、<>、!= 等非等值操作符
在WHERE子句中使用NOT、<>、!= 等非等值操作符，可能会导致索引失效。
#### 示例：

      SELECT * FROM users WHERE age != 30;
在这个查询中，age 列上的索引可能无法使用。
### 7. 表数据量过小
对于非常小的表，数据库优化器可能会认为全表扫描比使用索引更快，因此选择不使用索引。
### 8. 索引列参与计算
在WHERE子句中对索引列进行计算，会导致索引失效。
#### 示例：

      SELECT * FROM users WHERE age + 1 = 31;
在这个查询中，age + 1 对索引列进行了计算，导致索引失效。
### 9. 使用NULL值
如果索引列允许NULL值，并且查询条件涉及NULL值，可能会导致索引失效。
#### 示例：

      SELECT * FROM users WHERE age IS NULL;
在这个查询中，age 列上的索引可能无法使用。
### 10. 复合索引顺序不匹配
在使用复合索引时，查询条件的顺序必须与索引列的顺序匹配，否则索引可能会失效。
#### 示例：

      CREATE INDEX idx_name_age ON users(name, age);
      SELECT * FROM users WHERE age = 30 AND name = 'John';
在这个查询中，虽然 name 和 age 列上都有索引，但查询条件的顺序与索引列的顺序不匹配，导致索引失效。

通过避免上述情况，可以有效地利用索引提高SQL查询的性能。具体的优化方法需要根据实际情况进行调整。
