Java：JDK就是Java Development Kit.简单的说JDK是面向开发人员使用的SDK，它提供了Java的开发环境和运行环境。SDK是Software Development Kit 一般指软件开发包，可以包括函数库、编译程序等。
JRE是Java Runtime Enviroment是指Java的运行环境，是面向Java程序的使用者，而不是开发者。只要你的电脑安装了JRE，就可以正确运行Java应用程序
java环境变量的配置：
1.JAVA_HOME   C:\Program Files\Java\jdk1.8.0_102
2.PATH  %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;%JAVA_HOME%\lib\tools.jar

oracle中涉及的注意项：
1.数据库中的表tab（tablename）相当于一个数据字典。
2.字符大小写，日期格式敏感；
3.null值：（1）包含null的表达式都为null,
（2） null永远!=null
（3）如果集合中含有null，查询中不能使用not in,但可以使用in；
（4）null值排序：oracle中，null值最大
    一般降序：select * from emp order by comm desc  nulls last
（5）组函数，多行函数：自动滤空，即忽略空值。（可以嵌套滤空函数，屏蔽滤空的功能）比如：count（nvl（comm，0））；
4.oracle是自动开启事务的（可以直接rollback，commit），mysql是手动开启的（start transaction）。
5.sql优化：
 （1）尽量用列名查询，而非*
 （2）where中（and 和or）查询规则--从右往左
  （3）尽量使用where（当having和where可以同时使用，where效率更高）
6.order by 可按照序号（select后面列的位数）排列，若有多个列时，desc作用于离他最近的列，升序则作用于所有列。
7.修改日期格式：alter session（若为dba则为system） set NLS_DATE_FORMAT='yyyy-mm-dd';//会话已更改。
8.转义字符：\   select * from emp where ename like '%\_%' escape '\'
9. instr函数返回一个字符串在另一个字符串中首次出现的位置index（从1开始）。
10.通用函数：适用于任何数据类型，NVL（1,2），NVL2（1,2,3）---当1为空，返回3，不为空，返回2；
                        NullIF(1,2)--当1=2时，返回null，不等时返回1；
                        COALESCE(1,,,n)--从左到右找到第一个不为null的值;
11.条件表达式：DECODE函数(job,'',exp1,'',exp2,...)
12.分组函数（多行函数）：avg，count（distinct expr），sum，min，max；
13.在select列表中所有未包含在组函数中的列都应该包含在group by子句中，反之，包含在group by子句中的列不必包含在select列表中。
       在多个列进行分组时，依次从第一个列开始分组。
      group by的增强：group by rollup（列...）例如：  select deptno,job，sum(sal) from emp  group by rollup(deptno,job)
       主要应用与各种报表：比如工资报表
 14.--地址符 &，伪列：currval/nextval
SQL> insert into emp(empno,ename,sal,deptno) values(&empno,&ename,&sal,&deptno);
15. 海量拷贝数据
SQL> 1. 数据泵(datapump) ---> plsql
SQL> 2. SQL*Loader
SQL> 3. (数据仓库)外部表
SQL> 4. 可传输的表空间
16.delete和truncate的区别:
SQL> 1. delete逐条删除；truncate先摧毁表，再重建
SQL> 2.(根本)delete是DML，truncate是DDL
SQL>     （可以回滚）          （不可以回滚）
SQL> 3. delete不会释放空间 truncate会
SQL> 4. delete可以闪回  truncate不可以
SQL>    (flashback)
SQL> 5. delete会产生碎片（去掉碎片的方法：alter table 表名 move或者导出和导入）；truncate不会
17. 事务的标致
SQL> 1. 起始标志：事务中第一条DML语句
SQL> 2. 结束标志：提交： 显式 commit
SQL>                     隐式 正常退出（exit），DDL，DCL
SQL>              回滚:   显式 rollback
SQL>                      隐式 非正常退出，掉电，宕机
18.oracle中隔离级别支持三种：a.read committed b.seriizable串行化 c.read only（设置事务级别：set transaction read only;）
(数据库隔离级别有四种：除了a.b,还有read uncommitted  ，repeatable read)
19.行地址 rowid，相当于一个指针，直接一一对应数据文件的数据；
20.--修改表：追加新列，修改列，删除列，重命名列，重命名表
alter table test3 add photo blob/modify tname varchar2(40);/drop column photo/rename column tname to username;/rename test3 to test5。
21.--Oracle的回收站（-注意：管理员没有回收站）
SQL> show recyclebin
purge recyclebin;//回收站已清空
22.物化视图---可以缓存数据。
23.序列可以提高效率（存储在内存中），
24.索引：1.B树索引（insert，update。delete----oltp--联机事务处理）2.位图索引--select操作--数据仓库（olap--联机分析处理--比如热销商品-主流技术--hadhoop）
25.逻辑层面/概念层面：数据库通常更关注业务交易处理（OLTP），而数据仓库更关注数据分析层面（OLAP）
  1、oltp面对业务，一个是对业务分析
  2、使用者也不同，一个是面对业务人员，一个是决策人员
  5、oltp强调并发性，olap查询
  6、oltp针对当天的数据进行操作，olap针对当前和历史数据做处理。
