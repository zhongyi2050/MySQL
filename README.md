# something

# mysql的常见命令
1.查看当前所有的数据库
show databases;
2.打开指定的库
use 库名
3.查看当前库的所有表
show tables;
4.查看其它库的所有表
show tables from 库名；
5。创建表
create table 表名（
列名 列类型，
列名 列类型，
。。。
）
6.查看表结构
desc 表名；
7.查看服务器的版本
方式一：登录到mysql服务端
select version();
方式二：没有登录到mysql服务端 cmd
mysql --version或mysql -V

#mysql 的语法规范
1.不区分大小写，但是建议关键字大写，表名、列名小写
2.每条命令最好用分号结尾
3.每条命令根据需要，可以进行缩进或者换行
4.注释
单行注释：#注释文字
单行注释：- - 注释文字（注意- - 后有空格）
多行注释：/*  注释文字  */

#进阶1：基础查询 
/* 
语法： 
select 查询列表 from 表名； 
类似于：System.out.println（打印东西）； 
特点: 
1、查询列表可以是：表中的字段、常量值、表达式、函数 
2、查询的结果是一个虚拟的表段 
*/ 
USE myemployees; 
#1.查询表中的单个字段 
SELECT last_name FROM employees; 
#2、查询表中的多个字段 
SELECT last_name,salary,email FROM employees; 
#3、查询表中的所有字段 
SELECT`first_name`,`last_name`,`email`,`phone_number`,`job_id` FROM employees; 
SELECT * FROM employees; 
\ 
SELECT `name` FROM stuinfo; 
#4、查询常量值 
SELECT 1000; 
SELECT 'john'; 
#5、查询表达式 
SELECT 100%98; 
#6.查询函数 
SELECT VERSION(); 

#7、起别名 
/* 
①便于理解 
②如果要查询的字段有重名的情况，使用别名可以区分开来 
*/ 
#方式一： 
SELECT 100%98 AS 结果; 
SELECT last_name AS 姓，first_name AS 名 FROM employees； 
#方式二： 
SELECT last_name 姓,first_name 名 FROM employees; 
案例：查询salary，显示结果为 OUT put 
SELECT salary AS "out put" FROM employees; 
#8、去重 
#案例；查询员工表中涉及到得所有部门编号 
SELECT DISTINCT departmen_id FROM employees; 
#9、+号的作用 
/* 
java 中的+号： 
①运算符，两个操作数都未数值型 
②连接符，只要有一个操作数为字符串 
mysql中的+号 
仅仅只有一个功能：运算符 
select 100+90;两个操作数都为数值型，则做加法运算 
select '123'+90;其中一方为字符型，试图将字符型数值转换成数值型 
如果转换成功，则继续做加法运算 
如果转换失败，则将字符型数值转换成0 
select 'john' +90 
select null + 10;只要其中一方为null，则结果肯定为null 
*/ 
#案例：查询员工名和姓链接成一个字段，并显示为姓名 
SELECT CONCAT('a','b','c') 
SELECT CONCAT ('last_name','first_name') AS 姓名 
FROM employees; 

SELECT last_name,jib_id,salary AS sal 
#4、显示表departments的结构，并查询其中的全部数据 
DESC departments; 
SELECT *FROM `departments`; 
#5、显示出表employees中的全部job_id（不能重复） 
SELECT DISTINCT job_id FROM employees; 
#6、显示出表employees的全部列。各个列之间用逗号链接，列头显示成out_put 
SELECT 
IFNULL(commission_pct,0) AS 奖金率, 
commission_pct 
FROM 
employees; 
SELECT 
CONCAT(`first_name`,',',`last_name`,',',`email`,',',IFNULL(commission_pct,0)) AS out_put 
FROM 
employees;


  #进阶2：条件查询 
/* 
语法 
select 
查询列表 
from 
表名 
where 
筛选条件； 
分类： 
一、按条件表达式筛选 
条件运算符：> < = ！= <>不等于 >= <= 
二、按逻辑表达式筛选 
逻辑运算符： 
作用：用于链接条件表达式 
&& || ！ 
and or not 
&&和and：两个条件都未true，结果为true，反之为false 
||或or：只要有一个条件为true，结果为true，反之为false 
！或not：如果链接的条件本身为false，结果为true，反之为false 
三、模糊查询 
like 
between and 
is null 
*/ 
#一、按条件表示筛选 
#案例1：查询工资>12000的员工信息 
SELECT * 
FROM 
employees 
WHERE salary >12000; 
#案例2：查询部门编号不等于90号的员工名和部门编号 
SELECT 
last_name, 
department_id 
FROM 
employees 
WHERE 
department_id<>90; 
#二、按逻辑表达式筛选 
#案例1：查询工资在10000到20000之间的员工名、工资以及奖金 
SELECT 
last_name,salary,commission_pct 
FROM 
employees 
WHERE 
salary>=10000 AND salary <=20000; 
#2:查询部门编号不是在90到110 之间，或者工资高于15000的员工信息 
SELECT 
* 
FROM 
employees 
WHERE 
department_id<90 OR department_id>110 OR salary>15000; 
#三、模糊查询 
/* 
like 
⑴一般和通配符搭配使用 
通配符： 
% 任意多个字符 
_ 任意单个字符 
\ 转义符号和 ESCAPE '$' 
between and 
in 
is null|is not null 
*/ 
#1.like 
#案例1：查询员工名中包含字符a的员工信息 
SELECT 
* 
FROM 
employees 
WHERE 
last_name LIKE '%a%'; 
#案例2：查询员工名中第三个字符为e，第五个字符为a的员工名和工资 
SELECT 
last_name,salary 
FROM 
employees 
WHERE 
last_name LIKE '__n_l%'; 
#案例3：查询员工命中第二个字符为——的员工名 
SELECT 
last_name 
FROM 
employees 
WHERE 
last_name LIKE '_$_%' ESCAPE '$'; 
#2.between and 
/* 
⑴使用between and 可以提高语句的简洁度 
⑵包含临界值 
⑶两个临界值不要调换顺序 
⑷两个值得类型必须一致 
*/ 
#案例1：查询员工编号在100到120之间的员工信息 
SELECT 
* 
FROM 
employees 
WHERE 
employee_id >=100 AND employee_id<=120； 
#-------- 
SELECT 
* 
FROM 
employees 
WHERE 
employee_id BETWEEN 100 AND 120； 
#3.in 
/* 
含义：判断某字段的值是否属于in列表中的一项 
特点： 
⑴使用in提高语句简洁度 
⑵in列表的值类型必须统一或兼容（‘123’ 123） 
⑶不支持通配符 
*/ 
#案例：查询员工的工种编号是 it_prog、ad_vp、ad_pres 中的一个员工名和工种编号 
SELECT 
last_name, 
job_id 
FROM 
employees 
WHERE 
job_id = 'it_prog' OR job_id = 'ad_vp' OR job_id ='ad_pres'; 
#-------------------- 
SELECT 
last_name, 
job_id 
FROM 
employees 
WHERE 
job_id IN('it_prog','ad_vp','ad_pres'); 
#4.is null 
/* 
=或<>不能用于判断null值 
is null 或is not null 可以判断null值 
is 不能判断其他值 
*/ 
#案例1：查询没有奖金的员工名和奖金率 
SELECT 
last_name,commission_pct 
FROM 
employees 
WHERE 
commission_pct IS NOT NULL; 
#安全等于 <=> 
#案例1：查询没有奖金的员工名和奖金率 
SELECT 
last_name,commission_pct 
FROM 
employees 
WHERE 
commission_pct <=> NULL; 
#is null pk <=> 
IS NULL:仅仅可以判断null值，可读性较高，建议使用 
<=> :既可以判断null值，又可以判断普通的数值，可读性较低 
#进阶3：排序查询 
/* 
引入: 
select * from employees 
语法: 
select 查询列表 
from 表 
【where 筛选条件】 
order by 排序列表 【asc|desc】

特点： 
1、asc代表的是圣墟，desc 代表的是降序 
如果不写，默认是圣墟 
2、order by 子句中可以支持单个字段、多个字段、表达式、函数、别名 
3、order by 子句一般是放在查询语句的最后面，limit子句除外 
*/ 
#案例1:查询员工信息，要求工资从高到底排序 
SELECT * FROM employees ORDER BY salary DESC; 
SELECT * FROM employees ORDER BY salary ASC;

#案例2:查询部门编号>=90 的员工信息，按入职时间的先后进行排序 
SELECT * 
FROM employees 
WHERE department_id>=90 
ORDER BY hiredate ASC;

#案例3：按年薪的高低显示员工的信息和年薪【按表达式/别名排序】 
SELECT *,salary*12*(1+IFNULL(commission_pct,0))年薪 
FROM employees 
ORDER BY salary*12*(1+IFNULL(commission_pct,0)) DESC; 
SELECT *,salary*12*(1+IFNULL(commission_pct,0))年薪 
FROM employees 
ORDER BY 年薪 DESC;

#案例5：按姓名的长度显示员工的姓名和工资【按函数排序】 
SELECT LENGTH (lsat_name) 字节长度，last_name,salary 
FROM employees 
ORDER BY lengt(last_name) DESC;

#案例6：查询员工信息，要求先按工资排序，再按员工编号排序【按多个字段排序】 
SELECT * 
FROM employees 
ORDER BY salary ASC,employees_id DESC; 

#查询员工的姓名和部门号,年薪，按年薪降序，按姓名升序 
SELECT last_name,department_id,salary*12*(1+IFNULL(commission_pct,0)) 年薪 
FROM employees 
ORDER BY 年薪 DESC,last_name ASC; 
#选择工资不在 8000 到 17000 的员工的姓名和工资，按工资降序 
SELECT last_name,salary 
FROM employees 
WHERE salary NOT BETWEEN 8000 AND 17000 
ORDER BY salary ASC; 
#查询邮箱中包含 e 的员工信息，并先按邮箱的字节数降序，再按部门号升序 
SELECT *,LENGTH(email) 
FROM employees 
WHERE email LIKE '%e%' 
ORDER BY LENGTH(email)DESC,department_id ASC;

#进阶4：常见函数 
/* 
概念：类似于JAVA的方法，将一组逻辑语句封装再方法体重，对外暴露方法名 
好处：1、隐藏了实现细节 2、提高代码的重用性 
调用：select 函数名(实参列表)【from 表】； 
特点： 
⑴叫什么（函数名） 
⑵干什么（函数功能） 
分类： 
1、单行函数 
如 concat、length、ifnull等 
2、分组函数 
功能：做统计使用，又称为统计函数、聚合函数、组函数 
*/ 
#一、字符函数 
#1.length 获取参数值得字节个数 
SELECT LENGTH('张三丰hahaha') 
SHOW VARIABLES LIKE '%char%' 
#2.concat 拼接字符串 
SELECT concat（last_name,'_',first_name) 姓名 FROM employees; 
#3.upper、lower 
SELECT UPPER('jion'); 
SELECT LOWER('jion'); 
#示例、将姓变大写，名变小写，然后拼接 
SELECT CONCAT(UPPER(last_name),LOWER(first_name)) 姓名 FROM employees; 
#4.substr、substring 
注意：索引从1开始 
#截取从指定索引处后面的所有字符 
SELECT SUBSTR('李莫愁爱上了陆展元',7) out_put; 
#截取从指定索引处指定字符长度的字符 
SELECT SUBSTR('李莫愁爱上了陆展元',1,3) out_put; 
#案例：姓名中首字符大写，其他字符小写然后用—_拼接，显示出来 
SELECT CONCAT(UPPER(SUBSTR(last_name,1,1)),'_',LOWER(SUBSTR(last_name,2))) out_put 
FROM employees; 
#5.instr 返回子串第一次出现的索引，如果找不到，则返回0 
SELECT INSTR('杨不悔爱上了殷六侠','殷六侠'） AS out_put； 
#6.trim 去掉收尾字符串的的指定字符或默认字符 
SELECT LENGTH(TRIM(' 张翠山 ')) AS out_put; 
SELECT TRIM('a' FROM 'aaaa张aaaa翠山aaaaaa') AS out_put; 
#7.lpad 用指定的字符实现左填充指定长度 
SELECT LPAD('殷素素',5,'*') AS out_put; 
#8.rpad 用指定的字符实现右填充指定长度 
SELECT RPAD('殷素素',5,'*') 
#9.replace 替换 
SELECT REPLACE('周芷若周芷若周芷张无忌爱上了周芷若','周芷若','赵敏') AS out_put; 
#二、数学函数 
#round 四舍五入 
SELECT ROUND(-1.55); 
SELECT ROUND(1.567,2); 
#ceil 向上取整,返回>=该参数的最小整数 
SELECT CEIL(1.002); 
#floor 向下取整，返回<=该参数的最大整数 
SELECT FLOOR(-9.99); 
#truncate 截断 
SELECT TRUNCATE(1.69999,1); 
#mod 取余 
/* 
mod(a,b) :a-a/b*b 
select mod(10,-3); 
select 10%3; 
*/ 
#三、日期函数 
#now 返回当前系统日期+时间 
SELECT NOW(); 
#curdate 返回当前系统日期，不包含时间 
SELECT CURTIME(); 
#可以获取指定的部分，年、月、日、小时、分钟、秒 
SELECT YEAR(NOW()) 年; 
SELECT YEAR('1998-1-1') 年; 
SELECT yea(hiredate) 年 FROM employees; 
SELECT MONTH(NOW())月; 
SELECT MONTHNAME(NOW()) 英文月; 
#str_to_date:将日期格式的字符转换成指定格式的日期

#查询入职日为1992-4-3的员工信息 
SELECT * FROM employees WHERE hiredate = '1992-4-3'; 
SELECT * FROM employees WHERE hiredate = STR_TO_DATE('1992-4-3','%c-$%d %Y'); 
#date_format 将日期转换成字符 
SELECT DATE_FORMAT(NOW(),'%y年%m月%d日') AS out_put; 
#查询有奖金的员工名和入职日期(xx月/xx日 xx年) 
SELECT last_name,DATE_FORMAT(hiredate,'%m月/%d日 %y年') 入职 
FROM employees 
WHERE commission_pct IS NOT NULL; 
#四、其他函数 
/* 
select version(); #查看当前版本 
select database(); #查看当前的库 
select user(); #查看当前用户 
*/ 
#五、流程控制函数 
#1.if函数: if else 的效果 
SELECT IF(10>5,'大','小'); 
SELECT last_name,commission_pct,IF(commission_pct IS NULL,'没奖金，呵呵','有奖金，嘻嘻') 备注 
FROM employees; 
#2.case函数的使用一： switch case 的效果 
/* 
java中 
switch(变量或表达式){ 
case 变量1: 语句1;break; 
... 
default:语句n；break; 
} 
mysql中 
case 要判断的字段或表达式 
when 常量1 then 要显示的值1或语句1; 
when 常量2 then 要显示的值2或语句2; 
... 
else 要显示的值n或语句n; 
end 
*/ 
/*案例：查询员工的工资，要求 
部门号=30，显示的工资为1.1倍 
部门号=40，显示的工资为1.2倍 
部门号=50，显示的工资为1.3倍 
其他部门，显示的工资为原工资 
*/ 
SELECT salary 原始工资,department_id, 
CASE department_id 
WHEN 30 THEN salary*1.1 
WHEN 40 THEN salary*1.2 
WHEN 50 THEN salary*1.3 
ELSE salary 
END AS 新工资 
FROM employees; 
#3.case 函数的使用：类似于 多重if 
/* 
java中 
if(条件1){ 
语句1; 
}eles if(条件2){ 
语句2; 
} 
... 
else{ 
语句n; 
} 
mysql中： 
case 
when 条件1 then 要显示的值1或 语句1(值不用加分号，语句要); 
when 条件2 then 要显示的值2或 语句2 
。。。 
else 要显示的值n或语句n 
end 
*/ 
#案例：查询员工的工资的情况 
如果工资>20000,显示A级别 
如果工资>15000,显示B级别 
如果工资>10000,显示C级别 
否则，显示D级别 
SELECT last_name,salary, 
CASE 
WHEN salary>20000 THEN 'A' 
WHEN salary>15000 THEN 'B' 
WHEN salary>10000 THEN 'C' 
ELSE 'D' 
END AS 工资级别 
FROM employees ORDER BY salary DESC; 

