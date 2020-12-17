易混淆的点：

COUNT函数的结果根据参数的不同而不同。COUNT(*)会得到包含NULL的数据行数，而COUNT(<列名>)会得到NULL之外的数据行数。
聚合函数会将NULL排除在外。但COUNT(*)例外，并不会排除NULL。
MAX/MIN函数几乎适用于所有数据类型的列。SUM/AVG函数只适用于数值类型的列。
想要计算值的种类时，可以在COUNT函数的参数中使用DISTINCT。
在聚合函数的参数中使用DISTINCT，可以删除重复数据。


HAVING子句用于对分组进行过滤，可以使用数字、聚合函数和GROUP BY中指定的列名（聚合键）。
-- 数字
SELECT product_type, COUNT(*)
  FROM product
 GROUP BY product_type
HAVING COUNT(*) = 2;
-- 错误形式（因为product_name不包含在GROUP BY聚合键中）
SELECT product_type, COUNT(*)
  FROM product
 GROUP BY product_type
HAVING product_name = '圆珠笔';

GROUP BY 子句中不能使用SELECT 子句中定义的别名，但是在 ORDER BY 子句中却可以使用别名。

FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY


习题：
2.1
编写一条SQL语句，从 product（商品）表中选取出“登记日期（ regist 在2009年4月28日之后”的商品，查询结果要包含 product_name 和 regist_date 两列。
select product_name, regist_date
from product
where regist > "2009-4-28"

2.2
请说出对product 表执行如下3条SELECT语句时的返回结果。

①
SELECT *
  FROM product
 WHERE purchase_price = NULL;
②
SELECT *
  FROM product
 WHERE purchase_price <> NULL;
③
SELECT *
  FROM product
 WHERE product_name > NULL;

均不返回结果

2.3
代码清单2-22（2-2节）中的SELECT语句能够从product表中取出“销售单价（saleprice）比进货单价（purchase price）高出500日元以上”的商品。请写出两条可以得到相同结果的SELECT语句。执行结果如下所示。

product_name | sale_price | purchase_price 
-------------+------------+------------
T恤衫         |   1000    | 500
运动T恤       |    4000   | 2800
高压锅        |    6800   | 5000

SELECT product_name,sale_price,purchase_price
FROM product
WHERE sale_price-purchase_price >= 500;

2.4
请写出一条SELECT语句，从product表中选取出满足“销售单价打九折之后利润高于100日元的办公用品和厨房用具”条件的记录。查询结果要包括product_name列、product_type列以及销售单价打九折之后的利润（别名设定为profit）。
提示：销售单价打九折，可以通过saleprice列的值乘以0.9获得，利润可以通过该值减去purchase_price列的值获得。

select product_name,product_type,sale_price*0.9-purchase_price as profit
from product
where sale_price*0.9-purchase_price >= 100

2.5
请指出下述SELECT语句中所有的语法错误。

SELECT product_id, SUM（product_name）
--本SELECT语句中存在错误。
  FROM product 
 GROUP BY product_type 
 WHERE regist_date > '2009-09-01';

改正如下
SELECT product_type, SUM(product_name)
  FROM product 
 WHERE regist_date > '2009-09-01'
 GROUP BY product_type 

2.6
请编写一条SELECT语句，求出销售单价（sale_price 列）合计值大于进货单价（purchase_price 列）合计值1.5倍的商品种类。执行结果如下所示。

product_type | sum  | sum 
-------------+------+------
衣服         | 5000 | 3300
办公用品     |  600 | 320

select product_type,sum(sale_price),sum(purchase_price)
from product
group by product_type
having sum(sale_price) > 1.5 * sum(purchase_price)


2.7
此前我们曾经使用SELECT语句选取出了product（商品）表中的全部记录。当时我们使用了ORDERBY子句来指定排列顺序，但现在已经无法记起当时如何指定的了。请根据下列执行结果，思考ORDERBY子句的内容。

 ORDER BY regist_date desc,purchase_price
