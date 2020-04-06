# 生成表：
```
CREATE TABLE acquaintance(
friend1 varchar(20),
friend2 varchar(20),
class varchar(20)
);
```
# 题目二：
## 1
```
with t as (
select f1 m from acquaintance
union 
select f2 from acquaintance
)
select t1.m m1, t2.m m2
from t t1, t t2
where t1.m<>t2.m
and not exists (select * from acquaintance where (f1=t1.m and f2=t2.m) or (f2=t1.m and f1=t2.m));
``` 
## 2
```
with t as (
select f1 m,class from acquaintance
union 
select f2,class from acquaintance
)
select m from t t1 where not exists(select * from t where m=t1.m and class<>t1.class);
```
## 3
```

```
## 4
```
with t as (
select M,class,sum(cn) as cnt from 
(select friend1 AS M,class,count(*) as cn from acquaintance group by friend1,class
union all
select friend2 AS M,class,count(*) as cn from acquaintance group by M,class)
as t group by M,class)
select * from t AS t1 where cnt=(select max(cnt) from t where t1.class=t.class)
ORDER BY class;
```
## 5
```
```
## 6
```
with cte as (           --取出中间人及对应的关系
select t1.friend2 as name1, t2.friend2 as name2,t1.friend1 as 中间人
from acquaintance t1 inner join acquaintance t2 on t1.friend1=t2.friend1
union all
select t1.friend2 as name1, t2.friend1 as name2,t1.friend1 as 中间人
from acquaintance t1 inner join acquaintance t2 on t1.friend1=t2.friend2
union all
select t1.friend1 as name1, t2.friend2 as name2,t1.friend1 as 中间人
from acquaintance t1 inner join acquaintance t2 on t1.friend2=t2.friend1
union all
select t1.friend1 as name1, t2.friend1 as name2,t1.friend1 as 中间人
from acquaintance t1 inner join acquaintance t2 on t1.friend2=t2.friend2)
 
select top 1 中间人 from (           --取中间人连接最多的
select 中间人,count(*) cn from       --排除已经认识的
(select * from cte t where not exists (select 1 from acquaintance 
where (friend1=t.name1 and friend2=t.name2) or (friend2=t.name1 and friend1=t.name2) )) t
group by 中间人 order by cn desc)t
```
## 7
```
select f1,f2,class from acquaintance t1
where not exists (select * from acquaintance t2
where not exists(select * from acquaintance where class=t2.class 
and ((f1=t1.f1 and f2=t1.f2) or (f1=t1.f2 and f2=t1.f1))));
```
