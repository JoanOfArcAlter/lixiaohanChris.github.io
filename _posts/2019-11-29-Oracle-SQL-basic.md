---
layout: post
title:  "Oracle的使用及sql基本语法"
date:   2019-11-29 11:18:00 +0800
categories: 数据库
tags: Oracle SQL 
author: lxh
mathjax: true
---

* content
{:toc}
Oracle的使用及sql基本语法



## oracle的安装

> (how2j提供，网址：[how2j的oracle教程](http://how2j.cn/k/oracle/oracle-install/210.html?p=43061))

###### [oracle数据库下载](http://how2j.cn/frontdownload?bean.id=194)

> 安装路径不要有中文和括号！！管理员密码一般输入sys。

###### [oracle客户端下载](http://how2j.cn/frontdownload?bean.id=195)

> 产品码：

> KF46-6HV4-2UAC-YYEB-R82Y-6JB4-JVNH-WH 

> QWLJ-S6UT-WC93-5XW7-WK3Q-F7KA-SBEU-XL 

###### 登录

- 账号: sys 
- 密码: sys 
- 数据库: XE
- connect as: SYSDBA

> 数据库是这个oracle安装软件自带的XE，不填也是默认的

> (Normal:普通身份;SYSDBA:系统管理员身份;SYSOPER:系统操作员身份）

## 基本sql语句

> 表都是按以上步骤安装自带的，打开客户端，左上角file→new→SQL window，输入sql语句，F8执行，单行数据执行不用加分号，多行执行要加上

###### 基本查询 select from

```
select * from hr.employees --查询整个表，hr.emloyees是表名
select * from hr.employees e --结果同上，e是自己起的别名
select salary from hr.employees --查询月薪，字段salary是月薪
```

###### 算术表达式

```
select salary*12 from hr.employees 
--查询年薪，年薪可以用月薪乘十二表示
select salary/22 from hr.employees
--查询日薪，日薪可以用月薪除以二十二表示（假设每月工作22天）
```

###### 别名 

```
select salary*12 "年薪" from hr.employees --给salary*12起个名字：年薪
select salary*12 as "年薪" from hr.employees --同上
select salary*12 年薪 from hr.employees --同上
select salary*12 as 年薪 from hr.employees --同上
```

###### 字符串拼接 ||,concat

```
/* || */
select last_name,first_name from hr.employees
--查询姓和名，并显示在不同字段中
select last_name || first_name from hr.employees 
--查询姓和名并拼接在一起
select last_name || ' ' || first_name from hr.employees
--拼接的时候在姓和名中间加一个空格

/* concat */
select concat(last_name,first_name)from hr.employees 
--查询姓和名并拼接在一起
select concat(last_name,concat(' ',first_name))from hr.employees 
--拼接的时候在姓和名中间加一个空格
```

###### ||和concat的区别

> oracle中的concat只能传两个参数，想要多个值连接只能多重嵌套，会影响性能，所以一般会使用||。mysql中的concat可以传多个参数

###### 消除重复行 distinct

```
select department_id from hr.employees
--查询员工的部门，有许多员工在同一个部门，所以查出很多重复值
select distinct department_id from hr.employees
--消除重复行，每个部门id只显示一次
```

###### 条件限定 where

```
/* = 等于; > 大于; < 小于; >= 大于等于; <= 小于等于; <> 不等于 */
select * from hr.employees where salary > 5000 
--查询月薪大于5000的员工；
select * from hr.employees where salary <> 24000
--查询月薪不等于24000的员工
/* between and 在...之间 */
select * from hr.employees where salary between 3000 and 5000 
-- 查询月薪在3000与5000之间的员工
/* in() 在这些值中，括号里可以写若干个值 */
select * from hr.employees where salary in(24000,17000,6000)
--查询月薪为24000或者17000或者6000的员工
/* like 模糊查询，%代表任意匹配，有几个、有没有都无所谓；_代表有并且只有一个字符(区分大小写) */
select * from hr.employees where first_name like '%a_'
--查询名字倒数第二个字母是a的员工；
/* is null 为空 */
select * from hr.employees where department_id is null
--查询没有被分配部门的员工
```

###### 逻辑判断

```
/* and 与 */
select * from hr.employees where salary >= 3000 and salary <= 5000
--查询月薪在3000与5000之间的员工,与between那个结果相同
/* or 或 */
select * from hr.employees where first_name like '%a_' or first_name like '_a%'
--查询名字第二字母是a或者倒数第二个字母是a的员工
/* not 非 */
select * from hr.employees where first_name not like '%a_'
--查询名字倒数第二个字母不是a的员工
```

---

###### 阶段练习1
> 1.查询所有员工的姓名（last_name+' '+first_name）,工资，年终奖金（工资的百分之八十 乘以 commission_pct 在加500）别名（年终奖）。commsission_pct是年终奖系数.

```
select last_name || ' ' || first_name 姓名,salary 工资,salary*commission_pct+500 年终奖 
from hr.employees
-- 由于这个表commission_pct没有值，如果我们想让空值默认为0可以这样
select last_name || ' ' || first_name 姓名,salary 工资,salary*nvl(commission_pct,0)+500 年终奖 
from hr.employees -- 这样就算没有年终奖系数就按0计算

```

> oracle的nvl(expr1,expr2)函数：第一个参数为空的化就返回第二个参数，不为空就返回第一个参数;oracle的nvl2(expr1,expr2,expr3)函数：第一个参数为空就返回第三个参数，不为空就返回第二个参数


> 2.查询员工的姓名，工资，岗位(JOB_ID)，要求工资为2000~3000并且JOB_ID以‘ERK’结束

```
select last_name || ' ' || first_name 姓名,salary 工资,job_id 岗位 
from hr.employees 
where salary >=2000 and salary <= 3000 and job_id like '%ERK'
```

> 3.查询所有有人员的岗位编号，要求岗位(JOB_ID)中包含‘L’同时岗位(JOB_ID)名称以‘N’或‘K’结束，去掉重复行。


```
select distinct job_id 岗位编号 
from hr.employees 
where job_id like '%L%' and (job_id like '%N' or job_id like '%K') 
-- 因为与的优先级大于或，所以要加括号
```

---

###### 排序查询 order by

```
select * from hr.employees order by salary desc --倒序
select * from hr.employees order by salary asc --正序
select * from hr.employees order by salary --默认正序

```

###### 关联查询 left join/right join/inner join

```
/* 别名是为了区分出两个表中的字段 */
select em.first_name,d.department_name 
from hr.employees em left join hr.departments d 
on em.department_id = d.department_id
--查询所有员工的姓名以及所在的部门名称：使用左外连接,左边的员工不管有没有部门都会全部显示出来，没有员工的部门将不会显示
select em.first_name,d.department_name 
from hr.employees em right join hr.departments d 
on em.department_id = d.department_id
--查询出所有部门的名称以及部门员工的姓名：使用右外连接，右边的部门会不管有没有员工都全部显示出来，没有部门的员工将不会显示
select em.first_name,d.department_name 
from hr.employees em inner join hr.departments d 
on em.department_id = d.department_id
--查询出所有有部门的员工的员工姓名及所在部门的名称：使用内连接，只会显示出有部门的员工和有员工的部门
select em.first_name,d.department_name 
from hr.employees em left join hr.departments d 
on em.department_id = d.department_id 
union select em.first_name,d.department_name 
from hr.employees em right join hr.departments d
on em.department_id = d.department_id
-- 查询出所有员工和部门的名称：使用全连接，包括没有部门的员工以及没有员工的部门
```

###### 统计函数

```
select count(*) from hr.employees where salary > 5000
-- 统计月薪高于5000的员工有多少人
select max(salary),min(salary) 
from hr.employees 
where department_id = 100
-- 统计id为100的部门的最高工资和最低工资是多少
select avg(salary) from hr.employees
-- 统计平均月薪
select sum(salary) from hr.employees 
-- 统计月薪总和

```

###### 分组查询group by

```
/*分组进行进行统计查询*/
select avg(salary),department_id 
from hr.employees group by department_id
-- 查询每个部门的平均月薪和id，只能查询统计函数或部门id这种分组内一致的数据，其它比如员工姓名等不能查询
select avg(salary),department_id 
from hr.employees group by department_id 
having avg(salary) >6000
-- 查询平均月薪大于6000的部门的平均月薪和部门id
```

---

###### 阶段练习2

> 1查询部门编号大于等于50小于等于90的部门中工资小于5000的员工的编号、部门编号和工资

```
select employee_id,department_id,salary 
from hr.employees 
where salary < 5000 and department_id >= 50 and department_id <=90
```

> 2.显示员工姓名加起来一共15个字符的员工

```
select * from hr.employees where last_name || first_name like '_______________'
```

> 3.显示姓名不带有“ R ”的员工

```
select * from hr.employees where last_name || first_name not like '%R%'
```

> 4.查询所有员工的部门的平均工资，要求显示部门编号，部门名，部门所在地（需要多表关联查询： employees, department, location）

```
select avg(em.salary), em.department_id,d.department_name,l.street_address  
from hr.employees em
left join hr.departments d
on em.department_id = d.department_id
left join hr.locations l
on d.location_id = l.location_id
group by em.department_id ,d.department_name,l.street_address
-- 必须给统计函数约束范围，并约束范围时要列出查询时除了统计函数以外的所有字段
```

---

###### 子查询 ()

```
select count(*) from hr.employees where salary > 
(
 select salary from hr.employees where first_name = 'Bruce'
)
//查询出工资比Bruce高的人数
```

###### 分页查询 rownum

```
select * from
(select rownum r, e1.* from --添加伪列r，rownum是从1开始，用于给查询出的内容加上序号
  (
   select * from hr.employees order by salary desc --按月薪倒序查询
  ) e1 --命名为e1
) e2 where e2.r >=6 and e2.r<=10 -- 加上伪列的查询结果命名为e2，并查询出伪列序号为6到10的行
-- 查询工资第6到第10高的员工信息
```

---

###### 阶段练习3

> 1.查询所在部门所在城市为'South San Francisco'的员工

```
select * from hr.employees e 
inner join hr.departments d 
on e.department_id = d.department_id
inner join hr.locations l
on d.location_id = l.location_id
where l.city = 'South San Francisco'
```

> 2.查找和143在同一个部门和同一个岗位的比他工资高的员工。

```
select * from hr.employees 
where department_id = (select department_id from hr.employees where employee_id = 143)
and job_id = (select job_id from hr.employees where employee_id = 143)
and salary > (select salary from hr.employees where employee_id = 143)
```

> 3.查询总工资最多的部门，显示部门编号，部门名

```
select department_id,department_name 
from hr.departments 
where department_id in
    (select department_id from
        (
        select sum(salary) s,e1.department_id  
        from hr.employees e1 
        group by e1.department_id
        )/*查询出各个部门的总工资和id*/
     where s in 
        (select max(s) from
            (
            select sum(salary) s,e1.department_id  
            from hr.employees e1 
            group by e1.department_id
            ) /*查询出各个部门的总工资和id*/
        )/*查询出最高的总工资*/
    )/*查询出总工资最高的部门id*/
```

---

###### 创建表 create table

```
create table hero( --hero表名
  id number, --number数字类型
  name varchar2(30), --varchar2(30)可变长字符类型，最大长度30
  hp number,
  mp number,
  damage number,
  armor number,
  speed number
) --创建完成后可以select验证一下，不过表里还没有数据
```

###### varchar和varchar2的区别

> 两种都是变长的，对于汉字(unicode)的处理有区别：varchar如果放的是英文和数字，就用一个字节存放，如果是汉字(unicode)，就用两个字节；varchar2 统一使用两个字节。一般都直接使用varchar2，使用varchar很少了

###### 插入数据 insert into

```
insert into hero(id,name,hp,mp,damage,armor,speed) values (1,'锤石',5000,1000,800,500,500)
```

###### 序列 sequence

```
/* 创建序列 create sequence */
create sequence hero_seq 
  increment by 1 --每次增加1
  start with 1 --从1开始
  maxvalue 9999999 --最大值9999999
  
/* 获得hero_seq下一个值 */
select hero_seq.nextval from dual --dual是伪表
-- 每次获取都加1

/* 获得heor_seq当前值 */
select hero_seq.currval from dual

/* 把hero_seq的下一个值作为id存入hero表 */
insert into hero (id,name,hp,mp,damage,armor,speed)
values(hero_seq.nextval,'金克斯',2000,1000,800,200,500)
-- 多执行几次，可以select查看一下，hero_seq.nextval每次递增
```

###### 伪表 dual

```
/* 查询999*999的值 */
select 999*999 from dual
/* 字符串拼接 */
select concat('010-','88888888')||'转23' 高乾竞电话 from dual;
```

> 可以看到，这两条语句和表没什么关系，但是想要通过select得到一些数据必须要有from，所以借助dual这个伪表来保证查询语句的完整

###### 修改数据 update set

```
update hero set damage = 46 where id = 17
-- 修改一个数据
update hero set hp = 1,mp = 2 where id = 15
-- 修改多个数据
```

> 一般情况都会加上where根据id指定要修改的数据，如果不加where就是修改所有数据中该字段内容

###### 删除数据 delete from

```
delete from hero where id = 4
--删除hero表中id为4的数
delete from hero
--删除hero表中的所有数据
```

###### 舍去表 truncate table

```
truncate table hero
```

> truncate 比delete更快，更彻底，并且不能恢复，不能回滚。所以只有在确定要删除一个表所有记录的时候，才能执行这个操作

###### 修改表结构 alter

```
/* 增加字段 add */
alter table hero add(kills number)
--增加类型为数值的kills字段

/* 修改字段 modify */
alter table hero modify(name varchar2(300))
--修改name字段的最大长度为300

/* 删除字段 drop column */
alter table hero drop column kills
--删除字段kills，这句需要权限
```

###### 删除表 drop table

```
drop table hero
-- 删除hero表，这句不能回滚，使用时一定要非常小心
```

###### 约束

```
/* 唯一约束 unique */
create table hero1(
   id number,
   name varchar2(30) unique,-- 添加unique约束
   hp number,
   mp number,
   damage number,
   armor number,
   speed number
)
insert into hero1 (name)values('锤石') --执行两次试试
-- 创建完成后插入有相同name的数据就会提示出错：违反唯一约束条件（后面的约束名字是系统自动分配的）

/* 添加约束并命名 constraint */
create table hero2(
   id number,
   name varchar2(30),
   hp number,
   mp number,
   damage number,
   armor number,
   speed number,
   constraint uk_hero2_name unique (name) -- 给name添加unique约束并命名
)
-- 创建完成后再插入有相同name的数据时就会提示违反了uk_hero2_name的约束
-- uk代表unique，hero2表名，name约束的字段，约束名称是自定义的，但是为了提高易读性通常会这么命名

/* 给已经存在的表添加唯一约束 primary key */
alter table hero1 add constraint pk_hero1_id primary key (id)
-- 添加主键的字段不能有NULL值

/* 删除约束 */
alter table hero1 drop constraint pk_hero1_id 

/* 添加外键约束 foreign key  references */
create table kill_record(
   id number, --id
   killerid number, --杀人的人的id
   killedid number, --被杀的人的id
   number_ number, --次数
   constraint fk_killer_hero foreign key (killerid) references hero1(id), --必须是主键或者有唯一约束的字段
   constraint fk_killed_hero foreign key (killedid) references hero1(id)
)
-- 用来记录杀人关系
insert into hero1 (id,name) values (1,'锤石');
insert into hero1 (id,name) values (2,'安妮');
insert into kill_record (id,killerid,killedid,number_) values(1,1,2,1);
-- 代表锤石击杀了安妮一次
```

###### 视图 view

```
create view hero1simple as(
       select id,name from hero1
)
-- 创建一个只有hero1表中的id和name两个字段的简易表
select * from hero1simple
-- 之后就可以像一个普通的表一样操作了
```

> 修改表里的值，视图里也会跟着修改;修改视图里的值，表里的值也会跟个修改。









**李晓晗**

**更新于2019-11-29 中午**
