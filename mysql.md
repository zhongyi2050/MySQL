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

5.创建表
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

**mysql 的启动和停止**

mysql 的启动和停止
方式一：通过命令行
net start 服务名
net stop 服务名
方式二：计算机--右击--管理--服务


MySQL服务的登录和退出
登录：mysql 【-h 主机名 -p 端口号】 -u 用户名 -p密码

退出：exit 或 ctrl+C
