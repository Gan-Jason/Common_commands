# 常见的MySQL命令

---

## 一.用户验证  
### 1.连接MySQL  

* 语法  
    <table><tr><td bgcolor=pink>    
    mysql -h xx.xx.xx(主机ip,或为localhost) -u xxx(用户名) -p xxx(密码)
    </td></tr></table>

***注：用户名和主机IP前有无空格均可，但是密码前不能前不能有空格***

---
### 2.修改密码
* 语法
    <table><tr><td bgcolor=pink>
    mysqladmin -u用户名 -p旧密码 password 新密码
    </td></tr></table>
* 给用户(root)加个密码
    <table><tr><td bgcolor=black>
    mysqladmin -u用户名 -p旧密码 password 新密码
    </td></tr></table>
***注：因为开始时root没有密码，所以-p旧密码一项就可以省略了。 如果进入了mysql后想修改密码，参照下条*** 

* 初始化密码
   <table><tr><td bgcolor=pink>
     set PASSWORD=PASSWORD("123");注意mysql语句以分号结束
   </td></tr></table>
***注：以上均是对MySQL服务的操作，不属于SQL语句，句尾不需要分号，和以下区分***

---
## 二.数据库操作
### 3.添加新用户
* 语法
   <table><tr><td bgcolor=pink>
     grant select on 数据库.* to 用户名@登录主机 identified by “密码”
   </td></tr></table>
* 增加一个用户test1密码为abc，让他可以在任何主机上登录，并对所有数据库有查询、插入、修改、删除的权限。首先用root用户连入MYSQL，然后键入以下命令：

* 但增加的用户是十分危险的，你想如某个人知道test1的密码，那么他就可以在internet上的任何一台电脑上登录你的mysql数据库并对你的数据可以为所欲为了，解决办法见👇。
   <table><tr><td bgcolor=pink>
    grant select,insert,update,delete on mydb.* to [email=test2@localhost]test2@localhost[/email] identified by “”;
   </td></tr></table>
   
* 增加一个用户test2密码为abc,让他只可以在localhost上登录，并可以对数据库mydb进行查询、插入、修改、删除的操作(localhost指本地主机，即MYSQL数据库所在的那台主机),这样用户即使用知道test2的密码，他也无法从internet上直接访问数据库，只能通过MYSQL主机上的web页来访问了。

### 4.数据库操作
* 创建数据库
   <table><tr><td bgcolor=pink>
    create database database_name;
   </td></tr></table>
* 显示所有数据库
    <table><tr><td bgcolor=pink>
     show databases;
    </td></tr></table> 
* 删除数据库
   <table><tr><td bgcolor=pink>
    drop database <数据库名>;
   </td></tr></table>
* 选择数据库
   <table><tr><td bgcolor=pink>
    use <数据库名>;
   </td></tr></table>
### 5.数据表操作
* 显示所有数据表
   <table><tr><td bgcolor=pink>
    show tables;
   </td></tr></table>
* 显示table_name表的字段信息
   <table><tr><td bgcolor=pink>
    desc <数据表名>;
   </td></tr></table>

* 创建表
   <table><tr><td bgcolor=pink>
    create table  <数据表名> (name char(100), path char(100), count int(10), firstName char(100), firstMD5 char(100), secondName char(100), secondMD5 char(100), thirdName char(100), thirdMD5 char(100))engine=innodb,charset='utf8';
   </td></tr></table>
* 列出创建表时的创建语句，包含注释
   <table><tr><td bgcolor=pink>
    show create table <表名> \G
   </td></tr></table>
    
***注：创建表的结构、默认值、索引、递增递减自己选***
* 修改表名
   <table><tr><td bgcolor=pink>
    rename table <表1> to <表2>;
   </td></tr></table>
* 删除表
   <table><tr><td bgcolor=pink>
    drop table <表名>;   
   </td></tr></table>
* 插入数据
   <table><tr><td bgcolor=pink>
    insert into <表名> (name, path, count, firstName, firstMD5, secondName, secondMD5, thirdName, thirdMD5) VALUES ('test', 'test', 1, 'name1', 'md1', 'name2', 'md2', 'name3', 'md3');   
   </td></tr></table>
* 查询数据库中的重复数据 
   <table><tr><td bgcolor=pink>
    select firstMD5, count(*) as count from MIFit_Image group by firstMD5 having count > 1;  
   </td></tr></table
* 内联接查询数据
   <table><tr><td bgcolor=pink>
    select <table1>.id,<table1>.name,<table2>.id,<table2>.name from <table1> inner join <table2> on <table1>.id=<table2>.id; --组合两个表中的记录，返回关联字段相符的记录，也就是返回两个表的交集
   </td></tr></table>
* 左联接查询数据
   <table><tr><td bgcolor=pink>
    select <table1>.id,<table1>.name,<table2>.id,<table2>.name from <table1> left join <table2> on <table1>.id=<table2>.id; --left join 是left outer join的简写，它的全称是左外连接，是外连接中的一种。左(外)连接，左表(a_table)的记录将会全部表示出来，而右表(b_table)只会显示符合搜索条件的记录。右表记录不足的地方均为NULL。也就是以左表数据为准，把左表数据全部列出来，右表没有相关数据的补NULL；
   </td></tr></table>
* 更新数据
   <table><tr><td bgcolor=pink>
    update <表名> set folderName ='Mary' where id=1;
   </td></tr></table>
* 删除数据
   <table><tr><td bgcolor=pink>
    delete from <表名> where folderName = 'test';
   </td></tr></table>
    
### 6.数据表段操作
* 语法
* 添加字段
   <table><tr><td bgcolor=pink>
    alter table <表名> add id int auto_increment not null primary key;
   </td></tr></table>
* 修改字段的顺序 (id 放置最前)
   <table><tr><td bgcolor=pink>
    ALTER TABLE <表名> MODIFY 字段名1 数据类型 FIRST ｜ AFTER 字段名2;
    其中：

    字段名1：表示需要修改位置的字段的名称。
    数据类型：表示“字段名1”的数据类型。
    FIRST：指定位置为表的第一个位置。
    AFTER 字段名2：指定“字段名1”插入在“字段名2”之后。

    alter table <表名> modify id int first;

    alter table <表名> modify id int after name;
   </td></tr></table>
* 移除id主键标志
   <table><tr><td bgcolor=pink>
    alter table <表名> modify id int, drop primary key;
   </td></tr></table>
* 修改字段名name为folderName
    <table><tr><td bgcolor=pink>
    alter table <表名> change name folderName char(100);
    </td></tr></table>
* 删除字段
    <table><tr><td bgcolor=pink>
    alter table <表名> drop folderName;
    </td></tr></table>
* 加索引
    <table><tr><td bgcolor=pink>
    alter table <表名> add index indexName (folderName);
    </td></tr></table>
* 删除索引
    <table><tr><td bgcolor=pink>
    alter table <表名> drop index indexName;
    </td></tr></table>
    
* 修改字段的默认值(例如修改时间字段默认值为当前时间戳)
    <table><tr><td bgcolor=pink>
    alter table <表名> modify <columnName> timestamp not null default current_timestamp;
    </td></tr></table>
