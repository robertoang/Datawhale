视图是一个虚拟的表，不同于直接操作数据表，视图是依据SELECT语句来创建的（会在下面具体介绍），所以操作视图时会根据创建视图的SELECT语句生成一张虚拟表，然后在这张虚拟表上做SQL操作。

3.1.2 视图与表有什么区别
《sql基础教程**第2版》用一句话非常凝练的概括了视图与表的区别—“是否保存了实际的数据”。所以视图并不是数据库真实存储的数据表，它可以看作是一个窗口，通过这个窗口我们可以看到数据库表中真实存在的数据。所以我们要区别视图和数据表的本质，即视图是基于真实表的一张虚拟的表，其数据来源均建立在真实表的基础上。

“视图不是表，视图是虚表，视图依赖于表”。

CREATE VIEW <视图名称>(<列名1>,<列名2>,...) AS <SELECT语句>
需要注意的是在一般的DBMS中定义视图时不能使用ORDER BY语句。

1.
create VIEW ViewPractice5_1(product_name, sale_price, regist_date)
 as
select product_name, sale_price, regist_date
from product
where sale_price >= 1000 and regist_date = '2009-09-20';

2.
create or replace View ViewPractice5_2(product_name, sale_price, regist_date, product_id, product_type)
 as
select product_name, sale_price, regist_date,product_id, product_type
from product
where sale_price >= 1000 and regist_date = '2009-09-20';

insert into ViewPractice5_2 values (' 刀子 ', 300, '2009-11-02',100, '办公用品');
select * from ViewPractice5_2;

3.
select product_id, product_name, 
product_type, sale_price, 
(select avg(sale_price) from product) as sale_price_all
from product

4.
select product_id, product_name, product_type, sale_price , (SELECT AVG(sale_price)
 FROM product as table2
 where table2.product_type = table1.product_type
 group by product_type) as avg_sale_price
 from product as table1

5. √

7. SELECT   
    sum(case when sale_price <= 1000 then 1 else 0 end) as low_price,
    sum(case when sale_price > 1001 and sale_price < 3001 then 1 else 0 end) as low_price,
    sum(case when sale_price > 3001 then 1 else 0 end)  as high_price
  from product







