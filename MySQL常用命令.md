# MySQL常用命令(大小写不敏感)(非Linux命令)

## 管理数据库

- 添加用户: `CREATE USER '<用户名>'@'<主机>' IDENTIFIED BY '<密码>';`
  
  - 主机可为具体IP, 域名, 或本机(localhost), 或任意非本机地址(%).
  
- 添加所有权限: `GRANT ALL ON <作用范围(一般为数据库或表格的正则表达式)> TO '<用户名>'@'<登陆主机>';`

- 更新权限: `FLUSH PRIVILEDGES;`
  
  > (改变权限没有自动更新时使用)
  
- 显示权限: `SHOW GRANTS FOR <用户名>;`

- SSL状态: `SHOW VARIABLES LIKE '%ssl%';` 

- 以root用户进入MySQL命令行客户端(Linux命令): `sudo mysql -u root -p`

- 显示当前用户`SELECT user();` 或 `SELECT current_user();`

- 列表方式查看用户的所有权限, 依次执行:

  ` USE mysql; DESC user; SELECT * FROM user \G;`  

- 检查字符集设定: ` SHOW VARIABLES LIKE '%char%'; `
## 操作数据
- 建立数据库: ` CREAT DATABASE <name of database>;`
	- 显示所有(当前用户可见的)数据库: ` SHOW DATABASE;`
	- 切换操作的数据库: ` USE <name of database>;`
	- 如果命名与系统关键字重名, 要用反引号来避免系统误会``.
- 建立表: 
```  
CREATE TABLE <name of table>(
<field> <data type> <PRIMARY KEY(可选)>,
<field> <data type>,
......);
```
  > 数据类型常见的有INT, VARCHAR(长度), DATE, DECIMAL等. field之间用逗号隔开.  

- 显示表的参数: ` DESCRIBE <name of table>; `
- 删除表: ` DROP TABLE <name of table>;`
- 给表增加一列: ` ALTER TABLE <name of table> ADD <field> <data type>;`
	- 删除一列: ` ALTER TABLE <name of table> DROP COLUMN <field>;`
- 增加一条数据到表(entry), 两种方法:
	1. ` INSERT INTO <name of table> VALUES (<data>, <data>, <curdate()>, <number>, <'string'> );`
	> 此方法需要按建立表格时数据的排列顺序依次填入, 暂时不能填的要写NULL.
	2. ` INSERT INTO <name of table> (<data1>,<data2>,<data3>,<data4>) VALUES (<data>,<'string'>,<number>,<function()>);`
	> 此方法可以指定需要输入的数据, 也可以指定输入的顺序, 没有输入的部分会自动置NULL.
- 修改数据: ` UPDATE <name of table> SET <field> = <value> WHERE <condition>; `(i.e. id=3)
  
  > 多个条件也可使用 OR 和 AND 连接, 修改的内容也可以有多个, 用逗号分开.
- 删除数据: ` DELETE FROM <name of table> WHERE <condition>;`
  
  > 条件可以是数值判断, 例如 number > 60. 如果不写条件就会删除除了表头外所有数据.
- 选取数据(返回,显示数据): ` SELECT <field>, <field> FROM <name of table>;`
  > 显示指定的列(field), 使用*就显示所有的列.
  > 给结果排序, 加上 ` ORDER BY <field>;`
    - 排序结果默认是增序, 需要降序再加上DESC, 增序为ASC.
    - 可以使用多个排序依据: ` ORDER BY <field>, <field>;`
  - 结果显示最前面的几笔, 加上: ` LIMIT <number>;`
  - 选取结果也可加条件判断, 方法同上(WHERE).
- 不等号的写法: <>
- 多个判断条件的简化写法, 如果有三个判断条件: ` WHERE <field>=1 OR <field>=2 OR <field>=3;` 简化的写法是 ` WHERE <field> IN (1,2,3);`
- MySQL的通配符: % 匹配任意长度, _ 匹配单个字符.
- 筛选数据: ` SELECT * FROM <name of table> WHERE <field> LIKE '<%365%>';`
  
  > 以上代码意思为筛选出field这一列数据中含有365的所有行.
- 合并数据查询的结果(垂直合并): ` SELECT <field> FROM <table> UNION SELECT <field> FROM <table>;`
  
  > 可以合并多个查询结果, 被合并的结果需要有一样的列数和一样的数据类型.
- 合并表格(水平合并): ` SELECT * FROM <table> JOIN <table2> ON <condition>;`
  > 合并的条件一般是foreign key的对应.
  > 如果key name含有空格就要用反引号包裹, 所以命名时最好避免含有空格.
  > key name存在于多个表格中的, 加上表格的名字避免误会. 可以写成table.field.(例如class.id).
  JOIN 如果写成LEFT JOIN, 那么即使判断条件不成立也会返回左边(第一个)的表格数据. RIGHT JOIN就相反, 先返回右边(第二个)表格的数据,左边的表格数据只有满足判断条件才会被返回.
- 对于field查询结果, 可以设置临时的别名方便观看, 使用关键词 AS.
  例: `SELECT name AS 'student name' FROM <table>;`
  
  > 以上例子将表中的name显示成student name 展示给用户. 
- 删除条目: ` DELETE FROM <table> WHERE id=207;`
  
  > 以上将会删除表格id等于207的一行.
- 删除foreign key的时候, 一般对应的表格会被置NULL, 或者对应的条目也会被删除, 取决于建立foreign key时候的设定.

- subquery (嵌套查询)(以查询的结果去另一个表中查询):

  - 例1:
> select address  
> from name_list  
> where name=  
> (select name  
> from id_list  
> where id='001');    

>以上代码的意思是先在id_list中寻找 id 为 001的条目, 然后返回其名字, 再以这个名字在name_list中搜寻,返回此名字对应的地址.

  - 例2, 在销售金额表中找到销售额大于50000的人并返回id, 在员工列表中根据id找到人名:  

  >  SELECT name 
  >
  >  FROM employee_list 
  >  WHERE employee_id IN (
  >  SELECT id FROM sales 
  >  WHERE sales_value>50000);

######  