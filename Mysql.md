# 1、认识[MySQL](https://www.mysql.com/ )

## 1.1、数据库是什么？

数据库（DB，DataBase）

概念：数据仓库，软件，安装在操作系统上（window，Linux，mac、...）。SQL，可以存储大量的数据。500万！（超过再做索引优化等）

作用：存储数据，管理数据。

## 1.2、数据库分类

**关系型数据库：**（SQL）

- Mysql、Oracle、Sql Server、DB2、SQLite
- 通过表和表之间，行和列之间的关系进行数据的存储



**非关系型数据库：**（NoSql ）Not Only Sql

- Redis、MongoDB
- 非关系型数据库，对象存储，通过对象的自身的属性来决定



DBMS（数据库管理系统）





## 1.3、安装Mysql

安装建议：

1. 尽量不用使用exe，走注册表（卸载时遇到卸载不干净）
2. 尽可能使用压缩包安装

## 1.4、安装SQLyog

# 2、问题解决

## 2.1、MySql修改root密码

 **MySQL 5.7** 的版本，因为在user表中没有password字段，一直使用下边的方式来修改root密码 

```sql
use mysql; 
update user set authentication_string = password('root') where user = 'root' and Host = 'localhost';
```



 在mysql 5.7.9以后废弃了password字段和password()函数；authentication_string:字段表示用户密码，而authentication_string字段下只能是mysql加密后的41位字符串密码。  

**MySql 从8.0**开始修改密码有了变化，在user表加了字段authentication_string，修改密码前先检查authentication_string是否为空 

1. 如果不为空

   ```sql
   use mysql; 
   update user set authentication_string='' where user='root';-- 将字段置为空
   ALTER user 'root'@'localhost' IDENTIFIED BY 'root';--修改密码为root
   ```

2. 如果为空，直接修改

   ```sql
   ALTER user 'root'@'localhost' IDENTIFIED BY 'root';-- 修改密码为root
   ```

   ```sql
   flush privileges;-- 刷新权限
   ```

   

## 2.2、MySql8.0版本问题

MySQL 8.0以上版本（MySQL连接驱动和版本都是8.0以上）的时候出现的问题错误：

1. 第一个异常

   Loading class 'com.mysql.jdbc.Driver'. This is deprecated. The new driver class is 'com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.

   意思是 mysqljdbc . driver被弃用了新的驱动类是“com.mysql.cjdbc.driver”。驱动程序通过SPI自动注册，而手动加载类通常是不必要的。

   **解决方案如下：**

   把com.mysql.jdbc.Driver 改为com.mysql.cj.jdbc.Driver 即可

   

2. 第二个异常

   java.sql.SQLException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time 

   是时区的错误，因此只你需要设置为你当前系统时区即可。

   **解决方案如下：**

   加上?serverTimezone=GMT%2B8" 即可          

   GMT%2B8代表： 东八区

   static final String DB_URL="jdbc:mysql://localhost:3306/db1"+"?serverTimezone=GMT%2B8";
   
   

## 2.3、SQLyog连接MySql时出现 2058 错误

配置新连接报错：错误号码 2058，分析是 mysql 密码加密方法变了。

解决方法：windows 下cmd 登录 mysql -u root -p 登录你的 mysql 数据库，然后执行这条SQL：

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
-- 注意分号,'password' 是你自己设置的root密码；
```





# 3、基本的命令操作

```sql
mysql -uroot -p123456 -- 连接数据库
-- 所有的语句都使用；结尾
show databases; -- 查看所有的数据库


mysql> use db1 -- 切换数据库 user 数据库名
Database changed

show tables; -- 查看数据库中所有的表
describe user; -- 显示数据库中所有的表的信息

create database db1; -- 创建一个数据库db1

exit； -- 退出连接

-- 单行注释（sql的本来的注释）
```



**数据库xxx语言** CRUD增删改查！

DDL	定义

DML	操作

DQL	查询

DCL	控制





# 4、操作数据库

## 4.1、操作数据库

1. 创建数据库

   ```sql
   CREATE DATABASE IF NOT EXISTS db2; -- IF NOT EXISTS 判断不存在这个数据库就创建数据库，可以不写
   ```

2. 删除数据库

   ```sql
   DROP DATABASE IF EXISTS db2;
   ```

3. 使用数据库

   ```sql
   -- 如果你的表面或则字段名是一个特殊字符，就需要带 ``   符号在键盘tab键的上面。
   USE `db2`;
   ```

4. 查看数据库

   ```sql
   SHOW DATABASES --查看所有的数据库
   ```



- **sqlyog可视化历史记录查看sql**





## 4.2、数据库的列类型

>数据

tinyint		十分小的数据	1个字节

smallint	  较小的数据	   2个字节                         				

mediumint	中等大小的数据 	3个字节

**int	标准的整数	4个字节	常用的**

bigint	较大的数据	8个字节

float	浮点数	4个字节

double	浮点数	8个字节

decimal	字符串形式的浮点数	金融计算的时候，一般用decimal



>字符串

char	字符串固定大小的	0~255

**varchar	可变字符串	0~65535	常用的	String**

tinyint	微型文本	2^8-1

text	文本串	2^16-1	保存大文本



>时间日期

java.util.Date



date	YYYY-MM-DD	日期格式

time	HH:mm:ss	时间格式

**datetime	YYYY-MM-DD HH:mm:ss	最常用的时间格式**

**timestamp	时间戳	1970.1.1 到现在的毫秒数	也较为常用**

year	年份表示



>null

没有值，未知

注意，不要使用NULL进行运算，结果为NULL



## 4.2、数据库的字段属性（重点）

Unsigned：

- 无符号的整数
- 声明了该列不能声明为负数



zerofill：

- 0填充的
- 不足的位数，使用0来填充， 比如：int(3)     5   ----  005



自增：

- 自动在上一条记录的基础上+1（默认）
- 通常用来设计唯一的主键，必须是整数类型
- 可以自定义设计主键自增的起始值和步长



非空：not null

- 假设设置为not null，如果不给它赋值，就会报错
- null，如果不填写值，默认就是null



默认：

- 设置默认的值
- sex，默认值为男，如果不知道该列的值，则会有默认的值！



拓展：

```
每一个表，都必须存在一下五个自段，表示一个记存在的意义！

id	主键
`version`乐观锁
is_delete	伪删除
gmt_create	创建时间
gmt_update	修改时间
```





命令建表：

```sql
-- 注意点，使用英文 () ,表的名称 和 字段 尽量使用 `` 括起来
-- AUTO_INCREAMENT 自增
-- 字符串使用单引号括起来
-- 所有的语句后面加 ,  (英文的)，最后一句不用加
-- PRIMARY KEY 主键，一般一个表只有一个唯一的主键


CREATE TABLE IF NOT EXISTS `student2`(
	`id`	INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
	`sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别',
	`birthday` DATETIME  DEFAULT NULL COMMENT '出生日期',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (`id`)
	
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

格式

```sql
CREATE TABLE [IF NOT EXISTS] `表名`(
	`字段名` 列类型 [属性] [索引] [注释],
    `字段名` 列类型 [属性] [索引] [注释],
    `字段名` 列类型 [属性] [索引] [注释]
)[表类型] [字符集设置] [注释]
```



```sql
SHOW CREATE DATABASE db2; -- 查看创建数据库的语句

SHOW CREATE TABLE student2; -- 查看表的定义语句

DESC student2; -- 显示表的结果
```





## 4.3、数据表的类型

```
INNODB 默认使用		安全性高，事务的处理，多表多用户操作
MYISAM 早些年使用的		节约空间，速度较快
```



|              | MYISAM | INNODB        |
| ------------ | ------ | ------------- |
| 事务支持     | 不支持 | 支持          |
| 数据行锁定   | 不支持 | 支持          |
| 外键约束     | 不支持 | 支持          |
| 全文索引     | 支持   | 不支持        |
| 表空间的大小 | 较小   | 较大，约为2倍 |



## 4.4、修改删除表

>修改：

```SQL
-- 修改表面：alter table 旧表名 rename as 新表名
ALTER TABLE student2 RENAME AS student3;

-- 增加表的字段：alter table 表名 add 字段名 列属性
ALTER TABLE student3 ADD age INT(11);

-- 修改表的字段（重命名，修改约束）
-- alter table 表名 modify 字段名 列属性()
ALTER TABLE student3 MODIFY age VARCHAR(11); -- 修改约束
-- alter table 表名 change 旧字段名 新字段名 列属性()
ALTER TABLE student3 CHANGE age age1 INT(1); -- 字段重命名

-- 删除表的字段: alter table 表名 drop 字段
ALTER TABLE student3 DROP age1;
```

>删除：

```sql
-- 删除表（如果表存在再删除）
DROP TABLE IF EXISTS student;
```

==所有的创建和删除操作尽量加上判断，以免报错==





sql关键字大小写不敏感，建议小写





# 5、MySql数据管理

## 5.1、外键

创建表成功后，再添加外键约束

```sql
-- 创建年纪表
CREATE TABLE `grade`(
	`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年纪id',
	`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
	PRIMARY KEY (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8


-- 创建学生表
-- 学生表的 gradeid 字段，要去引用年级表的 gradeid
-- 给这个外键添加约束 （执行引用） references 引用
CREATE TABLE IF NOT EXISTS `student`(
	`id`	INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
	`sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别',
	`birthday` DATETIME  DEFAULT NULL COMMENT '出生日期',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	`gradeid` INT(10) NOT NULL COMMENT '学生的年纪',
	PRIMARY KEY (`id`)
	
)ENGINE=INNODB DEFAULT CHARSET=utf8

ALTER TABLE `student` 
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY(`gradeid`) REFERENCES `grade`(`gradeid`);
-- alter table 表 add constraint 约束名 foreign key(作为外键的列) references 引用的表名(引用的字段)
```

以上操作都是物理外键，数据库级别的外键，不建议使用（避免数据库过多造成困扰）

==最佳实践==

- 数据库就是单纯的表，是用来存数据，只有行（数据）和列（字段）
- 想使用多张表的数据，想使用外键（程序去实现）



## 5.2、DML语言（全部记住）

- insert
- update
- delete

## 5.3、添加

>insert

```sql
-- 插入语句（添加）
-- insert into 表名([字段1,字段2,字段3]) values('值1','值2','值3')
INSERT INTO `grade` (`gradename`) VALUES('大四');

-- 由于主键自增可以省略（如果不写表的字段，它就会一一匹配）
INSERT INTO `grade` VALUES('大二'); -- 报错

-- 一般写插入语句，一定要数据和字段一一对应
-- 插入多个字段
INSERT INTO `grade`(`gradename`) 
VALUES('大大'),('dayi');
```

注意事项：

1. 字段和字段之间使用英文逗号隔开
2. 字段是可以省略的，但是后面的值必须要一一对应，不能少
3. 可以同时插入多条数据，values后面的值，需要使用  ,   隔开即可

## 5.4、修改

>update

```sql
-- 修改学生名字
UPDATE `student` SET `name`='小明' WHERE id=1; -- 要有where条件，不然表里数据全改了

-- 修改多个属性，逗号隔开
UPDATE `student` SET `name`='小明',`email`='1111@qq.com' WHERE id=1;
```

条件：where子句，运算符 id等于某个值，大于某个值，在某个区间内修改...

操作符会返回布尔值

| 操作符            | 含义         | 范围        | 结果  |
| ----------------- | ------------ | ----------- | ----- |
| =                 | 等于         | 5=6         | false |
| <>或!=            | 不等于       | 5<>6        | true  |
| >                 |              |             |       |
| <                 |              |             |       |
| <=                |              |             |       |
| >=                |              |             |       |
| between... and... | 在莫格范围内 | [2,5]       |       |
| and               | 我和你&&     | 5>1 and 1>2 | false |
| or                | 我或你\|\|   | 5>1 or 1>2  | true  |

语法： update 表名 set column_name = value,[column_name = value,...] where [条件]

注意：

- column_name 是数据库的列，尽量带上``
- 条件，筛选的条件，如果没有指定，则会修改所有的列
- value,是一个具体的值，也可以是一个变量



## 5.5、删除

>delete

语法：delete from 表名 [where 条件]

```sql
-- 删除数据（避免这样写，会全部删除）
delete from `student`;

-- 删除指定数据
delete from `student` where id=1;
```



>truncate命令

作用：完全清空一个数据库表，表的结果和所有约束不会变

```sql
TRUNCATE TABLE `student`;
```



>delete和truncate区别

- 相同点：都能删除数据，都不会删除表结果
- 不同的：
  - truncate 重新设置自增列 计数器会归零
  - truncate 不会影响事务

了解：delete删除的问题，重启数据库后，出现的现象：

- InnoDB 自增列从1开始（存在内存当中的，断电即失）
- MyISM 继续从上一个自增量开始（存在文件中的，不会丢失）

