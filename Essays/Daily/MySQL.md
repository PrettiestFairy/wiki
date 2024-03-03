# MySQL

## Windwos下MySQL配置文件

```ini
[mysqld]
#shared-memory
#skip-grant-tables
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
#设置3306端口
port=3306
#设置mysql的安装目录
basedir="D:\mysql5.7.34"
#设置mysql数据库的数据的存放目录
datadir="D:\mysql5.7.34\Data"
#允许最大连接数
max_connections=10
#允许连接失败的次数.这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=5
#服务端使用的字符集默认为UTF8
character-set-server=utf8
#创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
#修改默认密码的加密方式
default_authentication_plugin=mysql_native_password
#指定一个请求的最大连接时间,对于4GB左右的内存服务器来说,可以将其设置为5-10
wait_timeout=10
#表缓存
table_open_cache=2560
#设置在网络传输中一次消息传输量的最大值.系统默认值为1MB,最大值是1GB,必须设置1024的倍数
max_allowed_packet=32M
#服务器有几个CPU就设置为几.建议用默认设置.一般为8
innodb_thread_concurrency=8
#服务器线程缓存,1GB内存配置为8,2GB配置为16,3GB配置为32,4GB或更高内存,可配置更大
thread_cache_size=8
#这对Innodb表来说非常重要.Innodb相比MyISAM表对缓冲更为敏感
innodb_buffer_pool_size=1024M
#如果将此参数设置为1，将在每次提交事务后将日志写入磁盘。为提供性能，可以设置为0或2，但要承担在发生故障时丢失数据的风险。设置为0表示事务日志写入日志文件，而日志文件每秒刷新到磁盘一次。设置为2表示事务日志将在提交时写入日志，但日志文件每次刷新到磁盘一次
innodb_flush_log_at_trx_commit=2
#此参数确定些日志文件所用的内存大小.以M为单位.缓冲区更大能提高性能.但意外的故障将会丢失数据.MySQL开发人员建议设置为1－8M之间
innodb_log_buffer_size=1M
#此参数确定数据日志文件的大小.以M为单位.更大的设置可以提高性能.但也会增加恢复故障数据库所需的时间
innodb_log_file_size=2M
#为提高性能.MySQL可以以循环方式将日志文件写到多个文件.推荐设置为3M
innodb_log_files_in_group=3M
#Buffer_Pool中Dirty_Page所占的数量.直接影响InnoDB的关闭时间
innodb_max_dirty_pages_pct=90
#InnoDB有其内置的死锁检测机制.能导致未完成的事务回滚.但是.如果结合InnoDB使用MyISAM的locktables语句或第三方事务引擎,则InnoDB无法识别死锁.为消除这种可能性.可以将innodb_lock_wait_timeout设置为一个整数值.指示MySQL在允许其他事务修改那些最终受事务回滚的数据之前要等待多长时间(秒数)
innodb_lock_wait_timeout=120
#独享表空间（关闭）
innodb_file_per_table=0
query_cache_limit=4M
[mysqldump]
max_allowed_packet=32M
[mysql]
#设置mysql客户端默认字符集
default-character-set=utf8
[client]
#设置mysql客户端连接服务端时默认使用的端口
port=3306

```

## 用户管理

```mysql
grant all on *.* to root@'%' identified by 'root';
grant all on *.* to discuss@'%' identified by '123';

```

## MySQL数据类型

### 数值类型

| 类型       | 大小 | 用途范围         |
| --------- | --- | --------------- |
| tinyint    | 1B   | 小数值           |
| smallint   | 2B   | 大数值           |
| mediumint  | 3B   | 大数值           |
| int/integr | 4B   | 大数值           |
| bigint     | 8B   | 超大数值         |
| folat      | 4B   | 单精度<br>浮点数 |
| double     | 8B   | 双精度<br>浮点数 |

### 字符串类型

| 类型       | 大小          | 用途范围                      |
| --------- | ------------ | ---------------------------- |
| char       | 255           | 定长字符串                    |
| varchar    | 65 535        | 变长字符串                    |
| tinyblob   | 255           | 不超过255个字符的二进制字符串 |
| tinytext   | 255           | 短文本字符串                  |
| blob       | 65 535        | 二进制形式的长文本数据        |
| text       | 65 535        | 长文本数据                    |
| mediumblob | 16 777 215    | 二进制形式的中等长度文本数据  |
| mediumblog | 16 777 215    | 中等长度文本数据              |
| longblob   | 4 294 967 295 | 二进制形式的极大文本数据      |
| longtext   | 4 294 967 295 | 极大文本数据                  |

### 时间类型

| 类型      | 大小 | 格式               | 用途范围                 |
| --------- | ---- | ----------------- | ----------------------- |
| DATE      | 3    | YYYY-MM-DD         | 日期值                   |
| TIME      | 3    | HH:MM:SS           | 时间值或持续时间         |
| YEAR      | 1    | YYYY               | 年份值                   |
| DATETIME  | 8    | YYYY-MM-DDHH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4    | YYYYMMDDHHMMSS     | 混合日期和时间值，时间戳 |

## MySQL基本语句

```mysql
# 查看数据时间
select now() ;

# 查询mysql版本
select version() ;

# 查看所有库 show + 库名
show databases ;

# 查看库的创建信息 show create + 库名
show create database mysql ;

# 查看错误信息
show warnings ;

# 创建数据库 create database + 库名
create database learn_db ;
create database if not exists learn_db ; # 如果没有该库则创建,
create database if not exists learn_db character set utf8 ; #如果没有该库则创建, 如果有则跳过, 库编码为:utf8
# create database learn_db2;
#
# 删除数据库 drop database + 库名
# drop database learn_db2;

show create database learn_db ;
# 修改数据库信息(编码) alter database
alter database learn_db character set 'gbk' ; #编码为'gbk'
alter database learn_db character set  'utf8' ; #编码为'utf8'
show create database learn_db ;

# 使用/切换数据库 use + 库名
use learn_db ;
select database() ; # 检查当前在哪个库

# 创建数据表 create + 表名(表字段用逗分隔) + 类型[约束]
/*
    primary key-->主键
    auto_increment-->自增
    not null-->不为空
    default-->默认
    unique-->唯一
    foreign key-->外键
    comment-->注释

*/
use learn_db ;
create table people(
    id tinyint primary key auto_increment,
    name varchar(20),
    gender boolean,
    age int,
    department varchar(255),
    salary double(7,2)
) ;

create table dis_department(
    id tinyint primary key auto_increment not null ,
    department varchar(255) not null
) ;

create table test(
    id tinyint primary key auto_increment not null

) ;
# 查看建建表信息 show create + 表名
show create table people ;

# 查看表结构 desc + 表名
desc people ;

# 添加字段 alter table + 表名 add +字段名 + 类型[约束]
alter table people add is_married tinyint(1) ;
alter table people add entry_data date not null ;
alter table people add A int,add B varchar(20) ;

# 删除字段 alter table + 表名 drop +字段名
alter table people drop A ;
alter table people drop B,drop entry_data ;

# 修改字段类型 alter table + 表名 modify + 新的字段名 + 类型[约束] [修改字段的位置]
/*
    修改字段的位置:
    first-->移动到首位
    after 字段名-->移动到改字段名的后面

*/
alter table people modify gender tinyint(1) not null default 0 comment '0:保密, 1:男, 2:女' after age;

# 修改字段名 alter table + 表名 change + 原字段名 + 新字段名 + 类型[约束]
alter table people change department depart varchar(255);

# 修改表名 rename table + 愿表名 to + 新表名
rename table dis_department to people_department ;

# 删除表 drop table + 表名
drop table test ;

# 查看表结构
desc people;
select * from people

```

## MySQL基本查询语句

```mysql
create table dis_exam(
    id int primary key auto_increment not null ,
    student_name varchar(20) not null ,
    exam_python double default 0 ,
    exam_linux double default 0 ,
    exam_mysql double default 0
);

insert into dis_exam values (1,'1号', 98, 98, 98),
                            (2,'2号',35, 98, 67),
                            (3,'3号',59, 59, 62),
                            (4,'4号', 88, 89, 82),
                            (5,'5号', 88, 98, 67),
                            (6,'6号', 86, 100, 55);

insert into dis_exam(student_name) values ('1号'), ('2号'), ('1号');

use learn_db;

# 查询所有 select * from + 表名
select * from dis_exam ;

# 去重 select distinct + 字段名 from 表名
select distinct student_name from dis_exam ;

# 查询一到多个字段的值 select 字段名1,字段名2,...,字段名n form + 表名
select student_name from dis_exam ;
select id, student_name from dis_exam ;

# 查询时显示(临时更改, 一般用于数值型数据)
select student_name, exam_linux from dis_exam ;
select student_name, exam_linux+10 from dis_exam ;

# 查询临时显示表头(别名) select +字段名 as + 别名 from 表名
select student_name as name from dis_exam ;

# 条件判断
/*
    比较运算符: > < >= <= <> !=
        between x and y -->在x到y之间
        in(x, y, z) -->值是x或y或z
        line '张三%' --> 等于..., %表示通配符, %->表示任意多的字符; _->表示一个字符
    逻辑运算符: and or not

    is null -->查询空值

*/
select * from dis_exam where exam_linux>60 ;
select * from dis_exam where exam_python!=88 ;
select * from dis_exam where exam_mysql between 80 and 100 ;
select * from dis_exam where exam_python in (88, 98) ;
select * from dis_exam where student_name like '1%' ;

```

## MySQL增删改查

```mysql
use learn_db ;

# 查询所有字段已经信息 select * from + 表名
select * from people ;

# 增
# 添加数据
/*
    insert into + 表名(要增加的字段名) values(对应字段的数据)
    insert into + 表名 values(全部字段对应的值)
    insert into + 表名 set
*/
insert into people(id, name, age, gender, department, salary, is_married) values(2, 'alex', 18, 1, '技术部门', 5000, 1) ;
insert into people(name, age, department, salary) values ('张三', 38, '技术部门', 500.5) ;
insert into people(name, age, department, salary) values ('李四', 22, '销售部', 3000),('王五', 23, '技术部门', 20000);

insert into people values (6, '赵六', 1, 30, '财务部', 4000, 2);
insert into people set name='孙七', gender=1, age=33, department='销售部', salary=3000, is_married=2;
insert into people set name='周八', age=22, department='服务部', salary=3500;

insert into people(name, age, department, salary) values ('张三1', 38, '技术部门', 500.5), ('张三2', 38, '技术部门', 500.5) ;
insert into people(name, age, department, salary) values ('张三1', 18, '技术部门', 500.5), ('张三2', 33, '技术部门', 500.5) ;

# 删
# 删除数据
/*
    delete from + 表名 -->全部删除数据
    delete from + 表名 where 字段名=值 -->只删除where条件所筛选的行
        and -->与
        or -->或
        not -->非
*/
# delete from people ;
delete from people where name='张三1' and age=38 or name='张三2' and age=38 ;

# 改
# 修改数据 update + 表名 set + 字段名= + 新值 where 字段=/like 该字段值 # where表示条件筛选
update people set salary=salary+20000 where name='alex' ;
update people set salary=20000 where name='张三' ;
update people set gender=2 where name like '张三' ;


# 查
# 查询数据 select 字段名/(*) from 表名 [where条件语句]
select name from people;
select * from people ;

```

## 多表查询

```mysql
use learn_db ;

# 创建主表
create table class_teachers(
    id int primary key auto_increment not null comment '班主任ID',
    name varchar(20) not null comment '班主任名字' ,
    gender boolean default 0 comment '0:保密,1:男,2:女' ,
    age int comment '班主任年龄'

) ;
# 主表插入数据
insert into class_teachers(name, gender, age) values ('冰冰', 2, 22),
                                                     ('丹丹', 2, 25),
                                                     ('歪歪', 1, 27),
                                                     ('珊珊', 2, 26),
                                                     ('小雨', 2, 30);

# 创建子表
create table class_students(
    id int primary key auto_increment not null comment '学生ID',
    name varchar(20) not null comment '学生姓名' ,
    teacher_id int not null comment '班主任ID'

) engine = innodb ;

# 子表插入数据
insert into class_students(name, teacher_id) values ('学生1', 2),
                                                    ('学生2', 4),
                                                    ('学生3', 1),
                                                    ('学生4', 3),
                                                    ('学生5', 1),
                                                    ('学生6', 3),
                                                    ('学生7', 2) ;

# 查询主表
select * from class_teachers ;
# 查询子表
select * from class_students ;

# 创建外键(在子表中创建)
drop table class_students;
show tables ;

create table class_students(
    id int primary key auto_increment not null comment '学生ID',
    name varchar(20) not null comment '学生姓名' ,
    teacher_id int not null comment '班主任ID' ,
    foreign key (teacher_id) references class_teachers(id)

) engine = innodb ;

insert into class_students(name, teacher_id) values ('学生1', 2),
                                                    ('学生2', 4),
                                                    ('学生3', 5),
                                                    ('学生4', 3),
                                                    ('学生5', 5),
                                                    ('学生6', 3),
                                                    ('学生7', 2) ;

# teacher
select * from class_teachers ;
# student
select * from class_students ;


# 插如外键
show create table class_students2 ;
alter table class_students2 add constraint teacher_id foreign key (teacher_id) references class_teachers(id) ;
show create table class_students2 ;
# 删除外键
alter table class_students2 drop foreign key teacher_id ;
show create table class_students2 ;

# 连级删除
/*
    foreign key (子表字段) references + 主表(主表字段) on delete cascade
    -- 删除连级
    alter table + 表名 drop foreign key + 外键名
    -- 连接删除后子表对应字段为Null ,子表的外键不能设置not null
    alter table + 表名 add constraint + 外键名 foreign key (子表字段) references + 主表(主表字段)

*/

select * from class_students, class_teachers ;
select * from class_teachers, class_students ;

select * from class_teachers, class_students where class_teachers.id=class_students.teacher_id ;
select * from class_students,class_teachers where class_students.teacher_id=class_teachers.id ;
select class_students.name, class_teachers.name from class_students,class_teachers where class_students.teacher_id=class_teachers.id and class_students.name='学生3';

```

## 连表查询

```mysql
# 内连接查询
select * from 表1 inner join 表2 on [条件]

```

## 高级查询

```sql
use learn_db;

# 排序查询
/*
    select + 字段名/*(多个字段名用逗号隔开) from + 表名 order by + 要排序的字段名
        order by + 字段名 [asc | desc]
        order by 默认为升序
        order by + 字段名 desc -->倒序
*/

select student_name, exam_python
from dis_exam
order by exam_python;
select student_name, exam_python
from dis_exam
order by exam_python desc;

select student_name, exam_python + exam_mysql + exam_linux as sum
from dis_exam
order by sum desc;

# 分组查询 select + 字段名/(*) from 表名 group by + 按哪个字段分类
select student_name
from dis_exam
group by student_name;

# 分组后条件筛选用having select + 字段名/(*) from 表名 group by + 按哪个字段分类 having [条件]
select student_name, sum(exam_mysql)
from dis_exam
group by student_name
having sum(exam_mysql) > 100;

# 计数查询 select count(字段名/(*)) from 表名 [条件]
select count(*)
from dis_exam;

# 平均值查询 select avg(字段名) from + 表名
select sum(exam_mysql) / count(student_name)
from dis_exam;
select avg(exam_mysql)
from dis_exam;

# 最值
# 最大值 select max(字段名/(*)) from + 表名
select max(exam_mysql)
from dis_exam;
select max(exam_mysql + exam_linux + exam_python)
from dis_exam;
#最小值 select min(字段名/(*)) from + 表名
select min(exam_mysql)
from dis_exam;
select min(exam_mysql + exam_linux + exam_python)
from dis_exam;

# 查看表中的前x行 select * from + 表名 limit x
select *
from dis_exam
limit 3;
# 查看跳过x行显示y行 select * from + 表名 limit x,y
select *
from dis_exam
limit 2,1;
```

