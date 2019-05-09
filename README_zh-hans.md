# SQL 速查表 

一个所有 SQL 语句用法的速查表。

这个仓库被社区不断添加和更新，欢迎提交 PR 👏。 

# 内容 
1. [ 查找数据的查询 ](#find)
2. [ 修改数据的查询 ](#modify)
3. [ 聚合查询 ](#report)
4. [ 连接查询 ](#joins)
5. [ 视图查询 ](#view)
6. [ 修改表的查询 ](#alter)

<a name="find"></a>
# 1. 查找数据的查询

### **SELECT**: 用于从数据库中选择数据
* `SELECT` * `FROM` table_name;

### **DISTINCT**: 用于过滤掉重复的值并返回指定列的行
* `SELECT DISTINCT` column_name;

### **WHERE**: 用于过滤记录/行
* `SELECT` column1, column2 `FROM` table_name `WHERE` condition;
* `SELECT` * `FROM` table_name `WHERE` condition1 `AND` condition2;
* `SELECT` * `FROM` table_name `WHERE` condition1 `OR` condition2;
* `SELECT` * `FROM` table_name `WHERE NOT` condition;
* `SELECT` * `FROM` table_name `WHERE` condition1 `AND` (condition2 `OR` condition3);
* `SELECT` * `FROM` table_name `WHERE EXISTS` (`SELECT` column_name `FROM` table_name `WHERE` condition);

### **ORDER BY**: 用于结果集的排序，升序（ASC）或者降序（DESC）
* `SELECT` * `FROM` table_name `ORDER BY` column;
* `SELECT` * `FROM` table_name `ORDER BY` column `DESC`;
* `SELECT` * `FROM` table_name `ORDER BY` column1 `ASC`, column2 `DESC`;

### **SELECT TOP**: 用于指定从表顶部返回的记录数
* `SELECT TOP` number columns_names `FROM` table_name `WHERE` condition;
* `SELECT TOP` percent columns_names `FROM` table_name `WHERE` condition;
* 并非所有数据库系统都支持`SELECT TOP`。 MySQL 中是`LIMIT`子句
* `SELECT` column_names `FROM` table_name `LIMIT` offset, count;

### **LIKE**: 用于搜索列中的特定模式，WHERE 子句中使用的运算符
* % (percent sign) 是一个表示零个，一个或多个字符的通配符
* _ (underscore) 是一个表示单个字符通配符
* `SELECT` column_names `FROM` table_name `WHERE` column_name `LIKE` pattern;
* `LIKE` ‘a%’    （查找任何以“a”开头的值）
* `LIKE` ‘%a’    （查找任何以“a”结尾的值）
* `LIKE` ‘%or%’  （查找任何包含“or”的值）
* `LIKE` ‘_r%’   （查找任何第二位是“r”的值）
* `LIKE` ‘a_%_%’ （查找任何以“a”开头且长度至少为3的值）
* `LIKE` ‘[a-c]%’（查找任何以“a”或“b”或“c”开头的值）

### **IN**: 用于在 WHERE 子句中指定多个值的运算符
* 本质上，IN运算符是多个OR条件的简写
* `SELECT` column_names `FROM` table_name `WHERE` column_name `IN` (value1, value2, …);
* `SELECT` column_names `FROM` table_name `WHERE` column_name `IN` (`SELECT STATEMENT`);

### **BETWEEN**: 用于过滤给定范围的值的运算符
* `SELECT` column_names `FROM` table_name `WHERE` column_name `BETWEEN` value1 `AND` value2;
* `SELECT` * `FROM` Products `WHERE` (column_name `BETWEEN` value1 `AND` value2) `AND NOT` column_name2 `IN` (value3, value4);
* `SELECT` * `FROM` Products `WHERE` column_name `BETWEEN` #01/07/1999# AND #03/12/1999#;

### **NULL**: 代表一个字段没有值
* `SELECT` * `FROM` table_name `WHERE` column_name `IS NULL`;
* `SELECT` * `FROM` table_name `WHERE` column_name `IS NOT NULL`;

### **AS**: 用于给表或者列分配别名
* `SELECT` column_name `AS` alias_name `FROM` table_name;
* `SELECT` column_name `FROM` table_name `AS` alias_name;
* `SELECT` column_name `AS` alias_name1, column_name2 `AS` alias_name2;
* `SELECT` column_name1, column_name2 + ‘, ‘ + column_name3 `AS` alias_name;

### **UNION**: 用于组合两个或者多个 SELECT 语句的结果集的运算符
* 每个 SELECT 语句必须拥有相同的列数
* 列必须拥有相似的数据类型
* 每个 SELECT 语句中的列也必须具有相同的顺序
* `SELECT` columns_names `FROM` table1 `UNION SELECT` column_name `FROM` table2;
* `UNION` 仅允许选择不同的值, `UNION ALL` 允许重复

### **ANY|ALL**: 用于检查 WHERE 或 HAVING 子句中使用的子查询条件的运算符
* `ANY` 如果任何子查询值满足条件，则返回 true。
* `ALL` 如果所有子查询值都满足条件，则返回 true。
* `SELECT` columns_names `FROM` table1 `WHERE` column_name operator (`ANY`|`ALL`) (`SELECT` column_name `FROM` table_name `WHERE` condition);

### **GROUP BY**: 通常与聚合函数（COUNT，MAX，MIN，SUM，AVG）一起使用，用于将结果集分组为一列或多列
* `SELECT` column_name1, COUNT(column_name2) `FROM` table_name `WHERE` condition `GROUP BY` column_name1 `ORDER BY` COUNT(column_name2) DESC;

### **HAVING**: HAVING 子句指定 SELECT 语句应仅返回聚合值满足指定条件的行。它被添加到 SQL 语言中，因为WHERE关键字不能与聚合函数一起使用。
* `SELECT` `COUNT`(column_name1), column_name2 `FROM` table `GROUP BY` column_name2 `HAVING` `COUNT(`column_name1`)` > 5;


<a name="modify"></a>
# 2. 修改数据的查询

### **INSERT INTO**: 用于在表中插入新记录/行
* `INSERT INTO` table_name (column1, column2) `VALUES` (value1, value2);
* `INSERT INTO` table_name `VALUES` (value1, value2 …);

### **UPDATE**: 用于修改表中的现有记录/行
* `UPDATE` table_name `SET` column1 = value1, column2 = value2 `WHERE` condition;
* `UPDATE` table_name `SET` column_name = value;

### **DELETE**: 用于删除表中的现有记录/行
* `DELETE FROM` table_name `WHERE` condition;
* `DELETE` * `FROM` table_name;

<a name="report"></a>
# 3. 聚合查询

### **COUNT**: 返回出现次数
* `SELECT COUNT (DISTINCT` column_name`)`;

### **MIN() and MAX()**: 返回所选列的最小/最大值
* `SELECT MIN (`column_names`) FROM` table_name `WHERE` condition;
* `SELECT MAX (`column_names`) FROM` table_name `WHERE` condition;

### **AVG()**: 返回数字列的平均值
* `SELECT AVG (`column_name`) FROM` table_name `WHERE` condition;

### **SUM()**: 返回数值列的总和
* `SELECT SUM (`column_name`) FROM` table_name `WHERE` condition;

<a name="joins"></a>
# 4. 连接查询

###  **INNER JOIN**: 内连接，返回在两张表中具有匹配值的记录
* `SELECT` column_names `FROM` table1 `INNER JOIN` table2 `ON` table1.column_name=table2.column_name;
* `SELECT` table1.column_name1, table2.column_name2, table3.column_name3 `FROM` ((table1 `INNER JOIN` table2 `ON` relationship) `INNER JOIN` table3 `ON` relationship);

### **LEFT (OUTER) JOIN**: 左外连接，返回左表（table1）中的所有记录，以及右表中的匹配记录（table2）
* `SELECT` column_names `FROM` table1 `LEFT JOIN` table2 `ON` table1.column_name=table2.column_name;

### **RIGHT (OUTER) JOIN**: 右外连接，返回右表（table2）中的所有记录，以及左表（table1）中匹配的记录
* `SELECT` column_names `FROM` table1 `RIGHT JOIN` table2 `ON` table1.column_name=table2.column_name;

### **FULL (OUTER) JOIN**: 全外连接，全连接是左右外连接的并集. 连接表包含被连接的表的所有记录, 如果缺少匹配的记录, 以 NULL 填充。
* `SELECT` column_names `FROM` table1 ``FULL OUTER JOIN`` table2 `ON` table1.column_name=table2.column_name;

### **Self JOIN**: 自连接，表自身连接
* `SELECT` column_names `FROM` table1 T1, table1 T2 `WHERE` condition;

<a name="view"></a>
# 5. 视图查询

### **CREATE**: 创建视图
* `CREATE VIEW` view_name `AS SELECT` column1, column2 `FROM` table_name `WHERE` condition;

### **SELECT**: 检索视图
* `SELECT` * `FROM` view_name;

### **DROP**: 删除视图
* `DROP VIEW` view_name;

<a name="alter"></a>
# 6. 修改表的查询

### **ADD**: 添加字段
* `ALTER TABLE` table_name `ADD` column_name column_definition;

### **MODIFY**: 修改字段数据类型
* `ALTER TABLE` table_name `MODIFY` column_name column_type;

### **DROP**: 删除字段
* `ALTER TABLE` table_name `DROP COLUMN` column_name;






