# 20210203 MySQL学习笔记

## 1.1基础语法

```
show databases； #查看当前所有数据库
create database 库名； # 创建新库
use 库名；            #转入某库
show tables；       #查看当前库的所有表
show tables from 库名； #查看其他库的所有表
create table 表名（列名 类型名，列名2 类型名，。。。）； #创建表规范例如
CREATE TABLE students(
 `id` INT(11) NOT NULL AUTO_INCREMENT, 
 `student_number` VARCHAR(32) NOT NULL DEFAULT '' COMMENT "学号", 
 `name` VARCHAR(32) NOT NULL DEFAULT '' COMMENT '姓名', 
 `department` VARCHAR(32) NOT NULL DEFAULT '' COMMENT '院系', 
 `del` TINYINT(1) NOT NULL DEFAULT 0 COMMENT '是否删除',
  `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL COMMENT '创建时间', 
  `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP NOT NULL COMMENT '更新时间',
   PRIMARY KEY (id), 
   UNIQUE INDEX  `idx_number` (`student_number`), 
   INDEX `idx_name_partment` (`name`, `department`) 
   ) ENGINE=INNODB DEFAULT CHARSET=utf8; 
CREATE TEMPORARY TABLE 表名；   #建立临时表    
desc 表名；  #查看表结构
select version（）；  #查看服务器的版本
drop database 库名;  #删除某一个库
drop table 表名；   #删除某一个表
```

## 2.DQL语言

### 2.1基础查询

```
select 组名 from 表；  #查询表中单个字段
select 组名1，组名2 from 表； #查询表中多个字段
select * from 表；   #查询表中所有字段
select 100； #查询常量值 eg：select  ‘john’；
select 100%98； #查询表达式
select version（）； #查询函数
select 100%98 as 别名1，组名 as 新组名 from 表；  #起别名 as可替换为空格
select distinct 组名1 from 表；  #去重
select contact（str1，str2。。。) as 组合名； #字符拼接 +只做数字运算符
select ifnull（组名，0） from 表； #如果组名该值为null，则替换为0
```

### 2.2条件查询

```
select 查询列表 from 表名 where 筛选条件；
```

#### 2.2.1按条件表达式筛选

简单条件运算符（7个）
< > = != <> >= <=
eg：

```
where 列名>30;
```

;

#### 2.2.2按逻辑表达式筛选

逻辑运算符（3个)
&&（and）、||（or）、！（not）
eg:

```
where 列名1>30 && 列名2<50;      #查询条件一个或多个
```

#### 2.2.3模糊查询

like # %匹配任意多个字符 下划线_匹配任意单个字符 

可在delete 和update时使用

eg：

```
where 列名 like '%我_';   #可以匹配该列中的 我是我的 
```

between and

eg：

```
where 列名 between 20 and 40；  #匹配该列在20-40中
```

in

is null <=> 安全等于

### 2.3排序查询

```
select  列表 from 表 where 筛选条件 order by 排序列表【asc（升）|desc（降）】
```

```
select  * from 表名

union 【all |distinct】

select * from 表名2；
```



### 2.4常见函数

#### 2.4.1字符函数

```
1. length(str);  #获取参数值的字节数
2. concat(str1,str2...); #拼接字符串
3. upper(str); #小写->大写
4. lowwer(str); #大写->小写
5. substr(str,索引（起始，长度）);
6. instr('str1','str2'); #返回字串第一次出现的索引，找不到则返回0
7. trim('str'); #去除左右空白格
8. trim('a' from 'str'); #去除左右‘a'字符
9. lpad('str',len,'*'); #用指定’*‘字符左填充len长度
10. rpad('str',len,'*'); #同上
11. replace('str1','str2','str3'); #在str1中用str3代替str2
```

正则表达式

```
select * from 表 where 列 regexp '正则式'   #匹配某一列符合表达式的数据
```

| 模式       | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| ^          | 匹配输入字符串的开始位置。如果设置了 RegExp 对象的 Multiline 属性，^ 也匹配 '\n' 或 '\r' 之后的位置。 |
| $          | 匹配输入字符串的结束位置。如果设置了RegExp 对象的 Multiline 属性，$ 也匹配 '\n' 或 '\r' 之前的位置。 |
| .          | 匹配除 "\n" 之外的任何单个字符。要匹配包括 '\n' 在内的任何字符，请使用像 '[.\n]' 的模式。 |
| [...]      | 字符集合。匹配所包含的任意一个字符。例如， '[abc]' 可以匹配 "plain" 中的 'a'。 |
| [^...]     | 负值字符集合。匹配未包含的任意字符。例如， '[^abc]' 可以匹配 "plain" 中的'p'。 |
| p1\|p2\|p3 | 匹配 p1 或 p2 或 p3。例如，'z\|food' 能匹配 "z" 或 "food"。'(z\|f)ood' 则匹配 "zood" 或 "food"。 |
| *          | 匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。 |
| +          | 匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。 |
| {n}        | n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o。 |
| {n,m}      | m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。 |

#### 2.4.2数学函数

```
1. round(num,位数); #四舍五入
2. ceil(num); #向上取整 >=
3. floor(num); #向下取整 <=
4. truncate(num,位数); #按位数在小数点后截断
5. mod(num1,num2); #取余
6. rand(); #获取随机数
```

#### 2.4.3日期函数

```
1. now(); #返回当前系统日期+时间
2. curdate(); #返回当前系统日期，不包含时间
3. curtime(); #返回当前时间，不包含日期
4. year(); #month();....返回年月日时分秒
5. str_to_date(9-13-1999','%m-%d-%y); #1999-09-13
6. date-format('2018/6/6','%y年%m月%d日'); #2018年06月06日
```

#### 2.4.4流程控制函数

1. if(判断语句,''是,'否'); #if判断语句
2. case语句

### 2.5分组函数

```
select 列名 from 表名 where 筛选条件 group by 分组列表 having 分组后筛选 order by 排序；
```

常用函数：
sum 求和 avg 求平均 max 求最大 min 求最小 count 计算个数

## 3.DML语言

### 3.1插入

```
1. insert into 表(列1，列2...)values(值1，值2...);
2. insert into 表 set 列名=值，列名2=值2...;
```

### 3.2修改

```
update 表名 set 列名=新值，列2=新值2... where 筛选条件;     #更新条件里的数据
```

```
ALTER TABLE 表  DROP 列;    #对于表结构添加某一列
ALTER TABLE 表 ADD 列 类型;   #对于表结构删除某一列
ALTER TABLE 表 MODIFY 列 新类型; #对于表结构修改某一列
ALTER TABLE 表 change 列 新列名 新类型; #对于表结构修改某一列
```

```
ALTER TABLE 表 ALTER 列 SET DEFAULT 1000;  #修改字段默认值
ALTER TABLE 表 ALTER 列 DROP DEFAULT;      #删除字段默认值
```

```
ALTER TABLE 表 RENAME TO 新表名;            #修改表名
```

### 3.3删除

```
1. delete from 表名 where 筛选条件;    #删除条件数据（没有where将会清除表中所有数据）
2. truncate table 表名;      #清空表数据
```